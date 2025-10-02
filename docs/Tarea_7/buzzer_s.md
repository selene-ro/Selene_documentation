# 🎶 Control de Frecuencia — Canción con Buzzer
> Selene Román Celis - 01/10/2025 

## Qué debe hacer

- Programar un buzzer piezoeléctrico para reproducir una melodía reconocible.

- Variar la frecuencia del PWM para las notas, manteniendo el duty en 50 %.

- Cada nota debe incluir su frecuencia y duración en el código.

## Notas y Frecuencias Definidas
 
| Nota | Frecuencia (Hz) |
|------|-----------------|
| DO   | 588.0 Hz |
| MI   | 660.0 Hz |
| FA   | 700.0 Hz |
| FA#  | 740.0 Hz |
| SOL  | 800.0 Hz |
| LA   | 880.0 Hz |
| SI   | 1000.0 Hz |
 
## Tiempos de duración definidos
 
| Identificador | Duración (ms) |
|---------------|---------------|
| `time_1` | 200 ms |
| `time_2` | 400 ms |
| `time_3` | 600 ms |
| `time_e` (pausa) | 200 ms |
| `time_p` (pausa más larga) | 300 ms |

## Secuencia de notas

| Estrofa | Secuencia de Notas |
|---------|---------------------|
| 1 | Si - Si |
| 2 | La - Sol - La - Si - Sol |
| 3 | La - Sol - Fa# - Si - Sol |
| 4 | La - Sol - Fa# - Si - Do - Do - Si - Sol - Fa |
| 5 | La - Sol - Fa - Sol - La - Fa |
| 6 | Sol - Fa - Mi - La - Fa |
| 7 | Sol - Fa - Mi - La - Fa - Do - Si - La - Fa |

```C++ 
#include "pico/stdlib.h"
#include "hardware/pwm.h"

#define BUZ_PIN 0
#define SOL 800.0f
#define SI  1000.0f
#define DO  588.0f
#define FA  700.0f
#define FAS 740.0f
#define MI  660.0f
#define LA  880.0f
#define TOP 1023

#define time_1 200
#define time_2 400
#define time_3 600

#define time_e 200
#define time_p 300


int main() {
    stdio_init_all();

    gpio_set_function(BUZ_PIN, GPIO_FUNC_PWM);
    uint slice = pwm_gpio_to_slice_num(BUZ_PIN);
    uint chan  = pwm_gpio_to_channel(BUZ_PIN);

    pwm_set_wrap(slice, TOP);
    pwm_set_enabled(slice, true);

    float f_clk = 150000000.0f; // 150 MHz

    while (true) {

    //Primera
    float div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_3);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_3);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);

    //Segunda
    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);

    //Tercera
    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FAS * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);

    //Cuarta
    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FAS * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (DO * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (DO * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_3);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);

    //Quinta
    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_3);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);

    //Sexta
    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (MI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);

    //Septima
    div = f_clk / (SOL * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (MI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_2);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (DO * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (SI * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (LA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_e);

    div = f_clk / (FA * (TOP + 1));
    pwm_set_clkdiv(slice, div);
    pwm_set_chan_level(slice, chan, 500);
    sleep_ms(time_1);

    pwm_set_chan_level(slice, chan, 0);
    sleep_ms(time_p);
    
    }
}
```
## Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/hLGHC6QJhFM?si=eSerq6K_Kitw_ZRh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>