# SPWM-STM32F103RB

Three-phase Sinusoidal Pulse Width Modulation (SPWM) implementation on the STM32F103RB (Nucleo-F103RB) using Advanced Timer 1 (TIM1) with hardware dead time and complementary outputs via STM32 HAL.

---

## Features

- **Timer / Mode**: TIM1 in Center-Aligned PWM Mode 1 (`TIM_COUNTERMODE_CENTERALIGNED1`).
- **Switching Frequency**: 10 kHz carrier frequency ($f_{carrier} = \frac{72\text{ MHz}}{2 \times 3600} = 10\text{ kHz}$).
- **Hardware Dead Time**: 3000 ns ($3.0\ \mu\text{s}$) generated internally via `BDTR` (`DeadTime = 172`) to prevent shoot-through.
- **3-Phase Displaced Outputs**: 6 complementary gate drive signals (Phase A, B, C shifted by $120^\circ$ and $240^\circ$).
- **Fixed-Point DDS Accumulator**: 32-bit phase accumulator updated synchronously in the TIM1 period elapsed ISR.
- **User-Editable Control Parameters**:
  - `MODULATION_INDEX`: Controls voltage amplitude / modulation depth ($0.0$ to $1.0$).
  - `MOTOR_FREQUENCY`: Controls fundamental output frequency (e.g., $50\text{ Hz}$, $60\text{ Hz}$).

---

## Pinout & Wiring (Nucleo-F103RB)

With the TIM1 Partial Remap:

| Signal | MCU Pin | Function / Switch | Nucleo Header Location |
| :--- | :--- | :--- | :--- |
| **TIM1_CH1** | **PA8** | Phase A High-Side (HIN1 / Q1) | Arduino **D7** / CN10 Pin 23 |
| **TIM1_CH1N** | **PA7** | Phase A Low-Side (LIN1 / Q2) | Arduino **D11** / CN10 Pin 15 |
| **TIM1_CH2** | **PA9** | Phase B High-Side (HIN2 / Q3) | Arduino **D8** / CN10 Pin 21 |
| **TIM1_CH2N** | **PB0** | Phase B Low-Side (LIN2 / Q4) | Arduino **A3** / CN10 Pin 31 |
| **TIM1_CH3** | **PA10** | Phase C High-Side (HIN3 / Q5) | Arduino **D2** / CN10 Pin 33 |
| **TIM1_CH3N** | **PB1** | Phase C Low-Side (LIN3 / Q6) | Morpho **CN10 Pin 24** (inner column) |
| **GND** | **GND** | Logic Ground reference | Any Nucleo GND pin |

---

## User-Configurable Parameters (`Core/Src/main.c`)

Located in `USER CODE BEGIN PD`:
```c
/* ==========================================================================
 * USER-EDITABLE SPWM PARAMETERS (ONLY EDIT THESE TWO)
 * ========================================================================== */
#define MODULATION_INDEX  1.0f    /* Modulation index M: 0.0f to 1.0f (Controls voltage amplitude) */
#define MOTOR_FREQUENCY   50.0f   /* Fundamental output frequency in Hz (Controls motor speed)     */
```

---

## How to Build & Flash

1. Open **STM32CubeIDE**.
2. Go to **File -> Open Projects from File System...** and select this project directory.
3. Press **Ctrl + B** to build the project.
4. Connect the Nucleo-F103RB board via USB and click **Run** or **Debug**.
