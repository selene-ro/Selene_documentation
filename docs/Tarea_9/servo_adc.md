# ⏲️⚙️ Servo con ADC
> Selene Román Celis - 03/11/2025 

## Qué debe hacer

Crear un codigo para mover un servo usando un potenciometro y un adc que vaya 0-180 grados

!!! tip "INFO IMPORTANTE"
    El movimiento de un servo requiere alimentacion 5-6v y en el pin de signal, un pwm a 50 HZ con un pulso de 1-2ms que representa 0-180 grados

## Código

```C++
#include <stdio.h>
#include "pico/stdlib.h"
#include "hardware/adc.h"
#include "hardware/pwm.h"

#define SERVO_PIN 1    
#define ADC_INPUT 0     
#define N_muestras 16    

#define TOP 1023         
#define F_SERVO_HZ 50    

#define SERVO_MIN 51      
#define SERVO_MAX 102   

int main() {
    stdio_init_all();
    adc_init();
    adc_gpio_init(27);
    adc_select_input(ADC_INPUT);

    gpio_set_function(SERVO_PIN, GPIO_FUNC_PWM);
    uint slice = pwm_gpio_to_slice_num(SERVO_PIN);
    uint chan = pwm_gpio_to_channel(SERVO_PIN);

    float f_clk = 125000000.0f;
    float div = f_clk / (F_SERVO_HZ * (TOP + 1)); 
    pwm_set_wrap(slice, TOP);
    pwm_set_clkdiv(slice, div);
    pwm_set_enabled(slice, true);

    uint16_t buffer[N_muestras];
    uint32_t suma = 0;
    uint8_t indice = 0; 
    uint8_t cuenta = 0;


    while (true) {
        uint16_t lectura = adc_read();

        if (cuenta < N_muestras) {
            buffer[indice] = lectura;
            suma += lectura;
            cuenta++;
            indice++;
        } else {
            suma -= buffer[indice];
            buffer[indice] = lectura;
            suma += lectura;
            indice++;
            if (indice >= N_muestras) indice = 0;

            uint16_t promedio = suma / N_muestras;
            uint16_t pulso = SERVO_MIN + (promedio * (SERVO_MAX - SERVO_MIN)) / 4095;
            pwm_set_chan_level(slice, chan, pulso);

            uint16_t grados = (promedio * 180) / 4095;

            printf("Grados: %u°\n", grados);

            sleep_ms(200);
        }
    }
}

```

## Esquema
![Diagrama del sistema](images/servo_adc.jpeg)

## Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/b_UafZgxlZc?si=eqvA62fFPKNtrjsG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>