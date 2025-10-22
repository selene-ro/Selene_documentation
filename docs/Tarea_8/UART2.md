# Terminal con instrucción "LED ON - LED OFF"
> Selene Román Celis - 22/10/2025
 
Este código permite controlar el encendido y apagado de un LED desde el monitor serial.
El usuario escribe los comandos “LED ON” o “LED OFF” en la terminal, y el microcontrolador ejecuta la acción correspondiente.
 
**Código Recepción**
 
```C++
 
#include "pico/stdlib.h"
#include "hardware/pwm.h"
 
 
#define BOTON1 16
#define BOTON2 19
#define MOTOR 0
#define F_PWM_HZ 2000   // 2 kHz: fuera del rango visible
#define TOP 1023        // 10 bits de resolución
 
int BOTON1ESTADO=0;
int BOTON2ESTADO=0;
 
 
int main() {
    stdio_init_all();
 
    gpio_set_function(MOTOR, GPIO_FUNC_PWM);
    uint slice = pwm_gpio_to_slice_num(MOTOR);
    uint chan  = pwm_gpio_to_channel(MOTOR);
 
    // Calcular divisor
    float f_clk = 150000000.0f; // 125 MHz
    float div = f_clk / (F_PWM_HZ * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_wrap(slice, TOP);
 
    pwm_set_chan_level(slice, chan, 1023);
    pwm_set_enabled(slice, true);
 
 
    gpio_init(BOTON1); gpio_set_dir(BOTON1,0);
    gpio_init(BOTON2); gpio_set_dir(BOTON2,0);
 
    int velocidades[3]={250,512,1023};
    int velocidad_actual=0;
 
    while (true) {
        BOTON1ESTADO=gpio_get(BOTON1);
        BOTON2ESTADO=gpio_get(BOTON2);
 
        if (BOTON1ESTADO==1){
            velocidad_actual++;
            if (velocidad_actual>2){
                velocidad_actual=0;
            }
            pwm_set_chan_level(slice, chan, velocidades[velocidad_actual]);
            sleep_ms(300);
        }
 
        if (BOTON2ESTADO==1){
            velocidad_actual--;
            if (velocidad_actual<0){
                velocidad_actual=2;
            }
            pwm_set_chan_level(slice, chan, velocidades[velocidad_actual]);
            sleep_ms(300);
        }
 
        sleep_ms(10);
 
 
    }
}
 
```
  
**Código Envío**
 
 
```C++
 
#include "pico/stdlib.h"
#include "hardware/uart.h"
#include <stdio.h>
#include <string>
 
#define UART_ID uart0
#define BAUD_RATE 115200
#define TX_PIN 0
#define RX_PIN 1
 
using namespace std;
 
string mensaje = "";
 
int main() {
    stdio_init_all();
 
    uart_init(UART_ID, BAUD_RATE);
    gpio_set_function(TX_PIN, GPIO_FUNC_UART);
    gpio_set_function(RX_PIN, GPIO_FUNC_UART);
    uart_set_format(UART_ID, 8, 1, UART_PARITY_NONE);
 
    sleep_ms(2000);
    printf("\nConexión lista. Escribe 'on;' o 'off;'.\n");
 
    while (true) {
        int ch_int = getchar_timeout_us(10000);
 
        if (ch_int == PICO_ERROR_TIMEOUT) {
            continue;
        }
 
        char ch = (char)ch_int;
 
        if (ch == '\r' || ch == '\n') {
            continue;
        }
 
        mensaje += ch;
 
        if (ch == ';') {
            string comando = mensaje.substr(0, mensaje.length() - 1);
 
            if (comando == "on" || comando == "off") {
                printf("Instrucción: %s\n", mensaje.c_str());
                uart_puts(UART_ID, mensaje.c_str());
            } else {
                printf("Instrucción inválida: '%s'\n", comando.c_str());
            }
 
            mensaje = "";
        }
    }
    return 0;
}
 
 
```
 
## Video
 
<iframe width="560" height="315" src="https://www.youtube.com/embed/hhR79yF1-ag?si=bAnLFJztqygP1KK_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
