#  🔛 Barrido de leds

> Selene Román Celis - 27/08/2025 

## Que debe hacer
 
Correr un “1” por cinco LEDs P0..P3 y regresar (0→1→2→3→2→1…)
 
``` Código
#include "pico/stdlib.h"
#include "hardware/structs/sio.h"
 
#define LED0 0
#define LED1 1
#define LED2 2
#define LED3 3
 
int main() {
   // Máscara
   const uint32_t MASK = (1u<<LED0) | (1u<<LED1) | (1u<<LED2) | (1u<<LED3);
 
   // Inicializar pines
   gpio_init(LED0);
   gpio_init(LED1);
   gpio_init(LED2);
   gpio_init(LED3);
 
   // Configurar como salida
   sio_hw->gpio_oe_set = MASK;
 
   int pos = 0;       // LED inicial
   int dir = 1;       // Dirección: 1→derecha, -1→izquierda
 
   while (true) {
       // Apagar todos
       sio_hw->gpio_clr = MASK;
 
       // Encender solo el LED actual
       sio_hw->gpio_set = (1u << pos);
 
       sleep_ms(200);
 
       // Mover posición
       pos += dir;
 
       // Rebotar en los extremos
       if (pos == 3) dir = -1;  
       if (pos == 0) dir = 1;
   }
}
```
## Esquemático
![Diagrama del sistema](images/esquema.png)
 
 
## Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/crPhcQlOhkA?si=fEG9RoWUUCJGJQFm" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

 