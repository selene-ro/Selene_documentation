# ⚙️🔄 Control de Servomotores con comandos
> Selene Román Celis - 23/10/2025 

## Hardware mínimo

- 1 × servomotor en un pin PWM (50 Hz).

- 3 × botones:

    - **BTN_MODE:** cambia el modo activo (cíclico: Entrenamiento → Continuo → Step → …).

    - **BTN_NEXT:** avanza a la siguiente posición (sólo en Step).

    - **BTN_PREV:** retrocede a la posición anterior (sólo en Step).

- Pi pico 2

## Modos de operación

1)  **Modo Entrenamiento**
Se recibe texto por USB-serial con los comandos siguientes (se aceptan minúsculas/mayúsculas indistintamente y también sus alias en inglés):

1. **Borrar** (alias: clear, borrar)

    - Sintaxis: Borrar

    - Efecto: elimina la lista completa de posiciones.

    - Respuesta: OK.

2. **Escribir** (alias: write, escribir)

    - Sintaxis: Escribir, v1, v2, ..., vn

        - vi son enteros en 0–180.

    - Efecto: sobrescribe la lista con los valores dados en ese orden.

    - Respuesta: OK si todos son válidos y la lisa de posiciones; si alguno está fuera de rango o la lista queda vacía → Error argumento invalido.

3. **Reemplazar** (alias: replace, reemplazar)

    - Sintaxis: Reemplazar, i, v

        - Índice i en base 1 (1 = primera posición).

        - v en 0–180.

    - Efecto: reemplaza el elemento i por v.

    - Respuesta: OK. Si i no existe → Error indice invalido. Si v fuera de rango → Error argumento invalido.

2) **Modo Continuo**
- Recorre todas las posiciones de la lista en orden, moviendo el servo e imprimiendo cada 1.5 s:

    - Formato: posX: V (por ejemplo, pos1: 90), donde X es base 1.

- Si la lista está vacía: imprimir cada 1.5 s Error no hay pos y no mover el servo.

- Al cambiar a otro modo, el ciclo se detiene inmediatamente.

3) **Modo Step**
- BTN_NEXT: avanza una posición (si ya está en la última, se mantiene en esa última).

- BTN_PREV: retrocede una posición (si ya está en la primera, se mantiene en la primera).

- En cada cambio de posición:

    - mover el servo a la posición seleccionada;

    - imprimir posX: V.

- Si la lista está vacía: al presionar BTN_NEXT o BTN_PREV, imprimir Error no hay pos y no mover el servo.

!!! tip "INFO IMPORTANTE"
    El movimiento de un servo requiere alimentacion 5-6v y en el pin de signal, un pwm a 50 HZ con un pulso de 1-2ms que representa 0-180 grados

 
```C++
#include "pico/stdlib.h"
#include "hardware/pwm.h"
#include <stdio.h>
#include <string>

using namespace std;

#define SERVO_PIN 0
#define SERVO_MIN 1000
#define SERVO_MAX 2000
#define FREC 50

#define B_MODE 1
#define B_NEXT 3
#define B_PREV 2

#define MAX_POS 10

string mensaje = "";
int posiciones[MAX_POS];
int total_pos = 0;
int modo = 0;
int pos_actual = 0;
uint32_t ultimo_continuo = 0;

// Función para mover el servo
void mover_servo(int angulo) {
    if (angulo < 0) angulo = 0;
    if (angulo > 180) angulo = 180;

    float pulso = SERVO_MIN + (angulo / 180.0f) * (SERVO_MAX - SERVO_MIN);
    float periodo = 1000000.0f / FREC;
    uint16_t duty = (uint16_t)((pulso / periodo) * 65535);
    pwm_set_gpio_level(SERVO_PIN, duty);
}

int main() {
    stdio_init_all();

    // Configurar PWM
    gpio_set_function(SERVO_PIN, GPIO_FUNC_PWM);
    uint slice = pwm_gpio_to_slice_num(SERVO_PIN);
    pwm_set_wrap(slice, 65535);
    pwm_set_clkdiv(slice, 125000000.0f / (FREC * 65536));
    pwm_set_enabled(slice, true);

    gpio_init(B_MODE); 
    gpio_set_dir(B_MODE, GPIO_IN); 
    gpio_pull_up(B_MODE);

    gpio_init(B_NEXT); 
    gpio_set_dir(B_NEXT, GPIO_IN); 
    gpio_pull_up(B_NEXT);

    gpio_init(B_PREV); 
    gpio_set_dir(B_PREV, GPIO_IN); 
    gpio_pull_up(B_PREV);

    while (true) {
        // Cambio de modo
        if (!gpio_get(B_MODE)) {
            modo = (modo + 1) % 3;
            pos_actual = 0;
            if (modo == 0) printf("Modo Entrenamiento\n");
            else if (modo == 1) printf("Modo Continuo\n");
            else printf("Modo Step\n");
            sleep_ms(300);
        }

        // Modo Entrenamiento
        if (modo == 0) {
            int ch = getchar_timeout_us(10000);
            if (ch != PICO_ERROR_TIMEOUT) {
                char c = (char)ch;
                if (c == '\r' || c == '\n') continue;
                mensaje += c;

                if (c == ';') {
                    string comando = mensaje.substr(0, mensaje.length() - 1);
                    string cmd_lower = comando;
                    for (auto &ch : cmd_lower) ch = tolower(ch);

                    // Comando clear/borrar
                    if (cmd_lower == "clear" || cmd_lower == "borrar") {
                        total_pos = 0;
                        printf("OK\n");
                    }
                    // Comando write/escribir
                    else if (cmd_lower.substr(0,5) == "write" || cmd_lower.substr(0,7) == "escribir") {
                        int inicio = comando.find(',') + 1;
                        bool error = false;
                        while (inicio > 0 && inicio < comando.length() && total_pos < MAX_POS) {
                            int fin = comando.find(',', inicio);
                            if (fin == string::npos) fin = comando.length();
                            string val_str = comando.substr(inicio, fin - inicio);
                            int val = atoi(val_str.c_str());
                            if (val_str.empty() || val < 0 || val > 180) { error = true; break; }
                            posiciones[total_pos++] = val;
                            inicio = fin + 1;
                        }
                        if (error || total_pos == 0) printf("Error argumento invalido\n");
                        else {
                            printf("OK. Posiciones: ");
                            for (int i = 0; i < total_pos; i++) printf("%d ", posiciones[i]);
                            printf("\n");
                        }
                    }
                    // Comando replace/reemplazar
                    else if (cmd_lower.substr(0,7) == "replace" || cmd_lower.substr(0,9) == "reemplazar") {
                        int pos1 = comando.find(',') + 1;
                        int pos2 = comando.find(',', pos1);
                        if (pos1 > 0 && pos2 != string::npos) {
                            int idx = atoi(comando.substr(pos1, pos2 - pos1).c_str()) - 1;
                            int val = atoi(comando.substr(pos2 + 1).c_str());
                            if (idx < 0 || idx >= total_pos) printf("Error indice invalido\n");
                            else if (val < 0 || val > 180) printf("Error argumento invalido\n");
                            else {
                                posiciones[idx] = val;
                                printf("OK\n");
                            }
                        } else printf("Error argumento invalido\n");
                    }
                    else printf("Código no reconocido\n");

                    mensaje = "";
                }
            }
        }

        // Modo Continuo
        if (modo == 1) {
            uint32_t now = to_ms_since_boot(get_absolute_time());
            if (now - ultimo_continuo >= 1500) {
                if (total_pos == 0) printf("Error no hay pos\n");
                else {
                    mover_servo(posiciones[pos_actual]);
                    printf("pos%d: %d\n", pos_actual + 1, posiciones[pos_actual]);
                    pos_actual = (pos_actual + 1) % total_pos;
                }
                ultimo_continuo = now;
            }
        }

        // Modo Step
        if (modo == 2) {
            if (!gpio_get(B_NEXT)) {
                if (total_pos == 0) printf("Error no hay pos\n");
                else if (pos_actual < total_pos - 1) pos_actual++;
                if (total_pos > 0) {
                    mover_servo(posiciones[pos_actual]);
                    printf("pos%d: %d\n", pos_actual + 1, posiciones[pos_actual]);
                }
                sleep_ms(200);
            }
            if (!gpio_get(B_PREV)) {
                if (total_pos == 0) printf("Error no hay pos\n");
                else if (pos_actual > 0) pos_actual--;
                if (total_pos > 0) {
                    mover_servo(posiciones[pos_actual]);
                    printf("pos%d: %d\n", pos_actual + 1, posiciones[pos_actual]);
                }
                sleep_ms(200);
            }
        }

        sleep_ms(10);
    }
}

```
## Video