# CanSat Payload: Telemetry Link & Custom PCB Integration
**A miniaturized satellite payload featuring a custom STM32-based PCB stack and a dual-antenna 2.4 GHz telemetry system, reaching the IN-SPACe / ISRO National CanSat Finals.**

**Tech Stack & Tools:** STM32F103, XBee Pro S2C, I2C/SPI, MATLAB Antenna Toolbox, PCB Layout, SMT Soldering.

---

## System Overview
The objective was to design, build, and launch a shock-survivable CanSat capable of capturing and transmitting real-time atmospheric and inertial data during descent. As the lead for PCB design and the RF telemetry link, I architected the hardware stack and computed the end-to-end link budget to ensure uninterrupted communication while the payload tumbled.

>
>*Caption: High-level system architecture and data flow.*

---

## Hardware & PCB Integration
To meet strict mass and volume constraints while ensuring survival through a 15G launch and 30G impact, the hardware was split across a multi-board PCB stack.

*   **Microcontroller & Sensors:** Integrated an STM32F103 MCU interfacing with a BMP280 (pressure/temperature) and MPU6050 (IMU) via I2C and SPI buses.
*   **Power & RF Coexistence:** Designed power regulation circuitry to isolate sensitive RF components from digital noise, incorporating an RTC and GPS module.
*   **Bring-Up:** Hands-on experience with schematic capture, layout, SMT soldering, and using laboratory instruments (oscilloscopes, multimeters) to debug interface signaling and validate board functionality.

>
>*Caption: Custom multi-board stack designed for high-shock survivability.*

---

## RF Design & Link Budget
Maintaining a link with a tumbling object requires careful antenna selection. I designed both ends of the 2.4 GHz telemetry link using the MATLAB Antenna Toolbox to guarantee a 1.5–2 km range.

### Dual-Antenna Strategy
1.  **CanSat (Transmitter):** Designed a 2.4 GHz microstrip patch antenna (5.94 dBi). A patch was chosen over a standard monopole to avoid the 0 dBi null at 90° altitude, ensuring peak broadside gain pointed toward the ground station.
2.  **Ground Station (Receiver):** Designed a 17.5-turn axial-mode helical antenna (11.1 dBi). The circular polarization of the helical antenna was critical to capture the signal regardless of the CanSat's orientation during its tumbling descent.

>
>*Caption: Simulated radiation patterns for the payload patch (left) and ground station helix (right).*

### Computed Link Budget
| Parameter | Value | Notes |
| :--- | :--- | :--- |
| **Transmit Power** | 8 dBm | XBee Pro S2C |
| **TX Antenna Gain** | 5.94 dBi | Simulated microstrip patch |
| **Free-Space Loss (FSL)** | ~110.14 dB | Computed at 2.4 GHz for a 2 km range |
| **RX Antenna Gain** | 11.1 dBi | Simulated axial-mode helix |
| **RX Sensitivity** | -101 dBm | Receiver threshold |
| **Received Power** | -84.14 dBm | At maximum range |
| **Link Margin** | **~17 dB** | Highly robust for the target descent |

---

## Key Engineering Trade-offs & Challenges
*   **Time-to-Flight vs. Custom Fabrication:** While the microstrip patch was fully designed and simulated, fabrication lead times threatened our launch readiness. I made the engineering decision to fly the XBee’s verified COTS (Commercial Off-The-Shelf) antenna for the actual flight, prioritizing mission success while applying the antenna design principles to optimize the ground station receiver.
*   **Descent Stabilization:** To minimize severe tumbling that could drop the RF link, I also led the parachute subsystem. We implemented a two-stage servo-actuated release (triggering at 500 m) with a spill-hole tuned semi-ellipsoid ripstop-nylon chute, successfully reducing the descent rate to a stable 1–3 m/s.

**Result:** The mission was a success, recovering continuous telemetry data through a 493-meter landing footprint.
