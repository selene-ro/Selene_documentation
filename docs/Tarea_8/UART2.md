# 💻💡 Terminal con instrucción "LED ON - LED OFF"
> Selene Román Celis - 22/10/2025

## Qué debe hacer
 
Este código permite controlar el encendido y apagado de un LED desde el monitor serial.
El usuario escribe los comandos “LED ON” o “LED OFF” en la terminal, y el microcontrolador ejecuta la acción correspondiente.
 
## **Código Recepción**
 
```C++

#include "pico/stdlib.h"
#include "hardware/uart.h"
#include <stdio.h>
#include <string>

#define UART_ID uart0
#define BAUD_RATE 115200
#define TX_PIN 0
#define RX_PIN 1
#define LED_PIN 2

using namespace std;

string mensaje = "";

int main() {
    stdio_init_all();

    gpio_init(LED_PIN);
    gpio_set_dir(LED_PIN, GPIO_OUT);
    gpio_put(LED_PIN, 0);

    uart_init(UART_ID, BAUD_RATE);
    gpio_set_function(TX_PIN, GPIO_FUNC_UART);
    gpio_set_function(RX_PIN, GPIO_FUNC_UART);
    uart_set_format(UART_ID, 8, 1, UART_PARITY_NONE);

    printf("Esperando mensajes...\n");

    while (true) {
        if (uart_is_readable(UART_ID)) {
            char ch = uart_getc(UART_ID);
            printf("%c", ch);
            mensaje += ch;

            if (ch == ';') {
                string comando = mensaje.substr(0, mensaje.length() - 1);

                if (comando == "on") {
                    gpio_put(LED_PIN, 1);
                    printf("\nLED ENCENDIDO\n");
                } else if (comando == "off") {
                    gpio_put(LED_PIN, 0);
                    printf("\nLED APAGADO\n");
                } else {
                    printf("\nMensaje no reconocido: '%s'\n", comando.c_str());
                }
                mensaje = "";
            }
        }
    }
    return 0;
}

```
  
## **Código Envío**
 
 
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
