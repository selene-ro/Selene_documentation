#  🔛 Barrido de leds

> Selene Román Celis - 27/08/2025


## Pulsera para detección de hipoglucemia nocturna
 La idea es un wearable que detecte signos fisiológicos de hipoglucemia nocturna usando sensores y un microcontrolador. Cuando identifica un episodio, la pulsera despierta al paciente mediante vibración, alarma sonora o incluso enviando una alerta al celular de un familiar.


## Microcontroladores

- **Seeed Studio XIAO nRF52840 Sense**
- **ESP32-C3**
- **Raspberry Pi Pico 2 (RP2350)**
- **nRF52840 Pro Micro**


## Tabla comparativa

| Característica     | Raspberry Pi Pico 2        | XIAO nRF52840 Sense    | ESP32-C3              | NRF52840 Pro-Micro   |
|--------------------|----------------------------|-------------------------|-----------------------|----------------------|
| Periféricos        | UART, SPI, I2C, PWM, PHY USB 1.1, PIO | UART, IIC, SPI, NFC, SWD, GPIO, ADC | GPIO, ADC, UART, I2C, SPI | ADC, PWM, SPI, I2C, UART, USB, GPIO |
| Memoria            | SRAM: 520 KB<br>Flash: 4 MB | RAM: 256 KB<br>Flash: 1 MB<br>QSPI: 2 MB | SRAM: 400 KB<br>Flash: 4 MB | Flash: 1 MB<br>RAM: 256 KB |
| Ecosistema         | Python, C/C++              | Arduino, Mbed, Zephyr  | ESP-IDF, Arduino, Mbed| Arduino, Zephyr      |
| Costos             | $150–$180                  | $300–$500              | $80–$100              | $170–$200            |
| Arquitectura       | Cortex-M33                 | Cortex-M4              | RISC-V                | Cortex-M4            |
| Velocidad de trabajo | 150 MHz                   | 64 MHz                 | 160 MHz               | 64 MHz               |

---

## Ranking

**1. Seeed Studio XIAO nRF52840 Sense**
:Este microcontrolador es la mejor opción para el proyecto por su tamaño compacto y gran potencia para procesar los datos de los sensores. Además, ya incluye modulo Bluetooth, lo cual es esencial para enviar las alertas a un celular cercano.

**2. ESP32-C3**
:Tiene como ventaja que incorpora Wi-fi y Bluetooth, lo que permite que la pulsera pueda enviar datos a un servidos en tiempo real. Además es muy económico. Tiene mayor consumo energético a comparación del XIAO nRF52840.

**3. Raspberry Pi Pico 2 (RP2350)**
:Tiene una gran potencia de procesamiento y es económica. Se tendría que añadir un módulo BLE o Wi-Fi, lo que incrementa el tamaño y consumo. 

**4. nRF5284O Pro - Micro**
:Está basado en el mismo chip nRF52840 que el XIAO, pero en un formato más grande y con menos integración de sensores. Aun así, tiene BLE integrado y un buen desempeño.
