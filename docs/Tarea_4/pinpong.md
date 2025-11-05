# 🏓 Mini-Pong
> Selene Román Celis - 04/09/2025 

## Qué debe hacer

Programar un mini-Pong con 5 LEDs en línea y 2 botones usando interrupciones (ISR) para registrar el “golpe” del jugador exactamente cuando la “pelota” (un LED encendido) llega al extremo de su lado.

**Reglas del juego**

1. Pelota: es un único LED encendido que se mueve automáticamente de un extremo al otro (L1→L5→L1…) a un ritmo fijo.

2. Golpe con ISR: cada botón genera una interrupción.

    * El BTN_L solo cuenta si, en el instante de la ISR, la pelota está en L1.

    * El BTN_R solo cuenta si, en el instante de la ISR, la pelota está en L5.

    - Si coincide, la pelota rebota: invierte su dirección.

    - Si no coincide (la pelota no está en el último LED de ese lado), el botón se ignora.

3. Fallo y punto: si la pelota alcanza L1 y no hubo golpe válido del lado izquierdo en ese momento, anota el jugador derecho. Análogamente, si alcanza L5 sin golpe válido, anota el jugador izquierdo.

4. Indicador de punto: al anotar, se parpadea el LED de punto 3 veces del jugador que metió el punto .

5. Reinicio tras punto: después del parpadeo, la pelota se reinicia en el centro (L3) y comienza a moverse hacia el jugador que metió el punto.

6. Inicio del juego: al encender, la pelota inicia en L3 y no se mueve hasta que se presione un boton y debera moverse a la direccion opuesta del boton presionado.

## Código

```C++
#include "pico/stdlib.h"
#include "hardware/gpio.h"   

#define LED_1 0
#define LED_2 1
#define LED_3 2
#define LED_4 3
#define LED_5 4

#define LED_J1 5
#define LED_J2 6

#define B_D 7
#define B_I 8

int led_on = 2;
int direc = 0;
int boton_d = 0;
int boton_i = 0;


void botones(uint gpio, uint32_t events) {
    if (gpio == B_D) boton_d = 1;
    if (gpio == B_I) boton_i = 1;
}

void parpadeo(int led) {
    for (int i = 0; i < 3; i++) {
        gpio_put(led, 1);
        sleep_ms(300);
        gpio_put(led, 0);
        sleep_ms(300);
    }
}

void init_leds() {
    gpio_init(LED_1);
    gpio_set_dir(LED_1, 1);
    gpio_init(LED_2);
    gpio_set_dir(LED_2, 1);
    gpio_init(LED_3);
    gpio_set_dir(LED_3, 1);
    gpio_init(LED_4); 
    gpio_set_dir(LED_4, 1);
    gpio_init(LED_5); 
    gpio_set_dir(LED_5, 1);
    gpio_init(LED_J1); 
    gpio_set_dir(LED_J1, 1);
    gpio_init(LED_J2); 
    gpio_set_dir(LED_J2, 1);
}

void init_botones() {
    gpio_init(B_D); 
    gpio_set_dir(B_D, 0);
    gpio_pull_up(B_D);
    
    gpio_init(B_I); 
    gpio_set_dir(B_I, 0);
    gpio_pull_up(B_I);

    // Interrupciones para botones
    gpio_set_irq_enabled_with_callback(B_D, GPIO_IRQ_EDGE_FALL, true, &botones);
    gpio_set_irq_enabled(B_I, GPIO_IRQ_EDGE_FALL, true); 
}

int main() {
    stdio_init_all(); 
    init_leds();
    init_botones();


    while (direc == 0) {
        gpio_put(LED_3, 1);
        if (boton_d) { direc = 1; boton_d = 0; }
        if (boton_i) { direc = -1; boton_i = 0; }
        sleep_ms(10);
    }

    while (true) {
        // Apagar LED
        gpio_put(LED_1, 0); gpio_put(LED_2, 0); gpio_put(LED_3, 0);
        gpio_put(LED_4, 0); gpio_put(LED_5, 0);

        gpio_put(led_on, 1);
        sleep_ms(500);

        if (led_on == LED_1) {
            if (boton_d) direc = 1;
            else {
                 parpadeo(LED_J2); led_on = LED_2; direc = 1; 
                }
            boton_d = 0;
        } 
        else if (led_on == LED_5) {
            if (boton_i) direc = -1;
            else { 
                parpadeo(LED_J1); led_on = LED_4; direc = -1; 
            }
            boton_i = 0;
        }

        led_on += direc;
    }
}

```
## Esquemático
![Diagrama del sistema](images/esquema4.png)

## Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/27hgnbmd6Wk?si=w_6thkJ4J-pCuQ-1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>