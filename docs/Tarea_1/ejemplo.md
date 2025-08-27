# 💾 Tabla comparativa de Microcontroladores 

> Selene Román Celis - 27/08/2025

---

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

## 4) Requisitos

**Software**
- _SO compatible (Windows/Linux/macOS)_
- _Python 3.x / Node 18+ / Arduino IDE / etc._
- _Dependencias (p. ej., pip/requirements, npm packages)_

**Hardware (si aplica)**
- _MCU / Sensores / Actuadores / Fuente de poder_
- _Herramientas (multímetro, cautín, etc.)_

**Conocimientos previos**
- _Programación básica en X_
- _Electrónica básica_
- _Git/GitHub_

---

## 5) Instalación

```bash
# 1) Clonar
git clone https://github.com/<usuario>/<repo>.git
cd <repo>

# 2) (Opcional) Crear entorno virtual
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# 3) Instalar dependencias (ejemplos)
pip install -r requirements.txt
# o, si es Node:
npm install


```