# 🖲️ Compuertas básicas AND / OR / XOR con 2 botones

> Selene Román Celis - 01/09/2025 

## Qué debe hacer
Con dos botones A y B (pull-up; presionado=0) enciende tres LEDs que muestren en paralelo los resultados de AND, OR y XOR. En el video muestra las 4 combinaciones (00, 01, 10, 11).

```codigo
#include "pico/stdlib.h"

#define BTN_A   5   
#define BTN_B   4   
#define LED_AND 0   
#define LED_OR  1   
#define LED_XOR 2   

int main() {
    
    const uint32_t BTN_MASK = (1u << BTN_A) | (1u << BTN_B);
    const uint32_t LED_MASK = (1u << LED_AND) | (1u << LED_OR) | (1u << LED_XOR);

    gpio_init_mask(BTN_MASK | LED_MASK);
    gpio_set_dir_out_masked(LED_MASK);  
    gpio_set_dir_in_masked(BTN_MASK);   
    gpio_pull_up(BTN_A);
    gpio_pull_up(BTN_B);

    while (true) {
        
        int a = !gpio_get(BTN_A);
        int b = !gpio_get(BTN_B);

        
        int val_and = a & b;
        int val_or  = a | b;
        int val_xor = a ^ b;

        
        uint32_t leds = (val_and << LED_AND) | (val_or << LED_OR) | (val_xor << LED_XOR);

        gpio_put_masked(LED_MASK, leds);

        sleep_ms(20);
    }
}
```
## Esquemático
![Diagrama del sistema](images/esquema2.png)

## Video 
<iframe width="560" height="315" src="https://www.youtube.com/embed/3kYsAk8cVfE?si=u0389eEojDJMeK3b" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>