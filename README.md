# STM32G431RB Bare-Metal Traffic Light Controller 🚦

This project implements a bare-metal, register-level traffic light controller on an **STM32 Nucleo-G431RB** development board without using high-level hardware abstraction layers (HAL) or CMSIS headers. 

The goal is to demonstrate low-level firmware architecture, direct memory-mapped register access, and bitwise hardware manipulation on ARM Cortex-M4 microcontrollers.

---

## 📌 Hardware Overview & Pin Mapping

- **Microcontroller:** STM32G431RB (ARM Cortex-M4)
- **Peripherals:** 3x LEDs (Red, Yellow, Green), 3x Resistors, Breadboard
- **Pin Configuration:**
  - **PA0:** Green LED
  - **PA1:** Yellow LED
  - **PA4:** Red LED

---

## 🛠️ Low-Level Technical Details

### 1. Memory-Mapped Base Addresses & Registers
Direct pointers were derived using absolute physical addresses specified in the STM32G4 Reference Manual (RM0440):
- `RCC_BASE`: `0x40021000UL`
- `GPIOA_BASE`: `0x48000000UL`
- `RCC_AHB2ENR`: Base + `0x4C` (Enables GPIOA Clock Signal)
- `GPIOA_MODER`: Base + `0x00` (GPIO Port Mode Register)
- `GPIOA_BSRR`: Base + `0x18` (Bit Set/Reset Register)

### 2. Atomic Pin Toggling with `BSRR`
Instead of performing read-modify-write operations on the `ODR` (Output Data Register), output states are set atomically using the `BSRR` register:
- **Bits 0–15:** Write `1` to Set corresponding pin `HIGH` (3.3V)
- **Bits 16–31:** Write `1` to Reset corresponding pin `LOW` (0V)

This eliminates potential race conditions during hardware bit toggling.

---

## 🚦 Traffic Light State Sequence

1. **State 1 (RED):** Red LED ON (3 seconds)
2. **State 2 (RED + YELLOW):** Red and Yellow LEDs ON (1 second)
3. **State 3 (GREEN):** Green LED ON (3 seconds)
4. **State 4 (YELLOW):** Yellow LED ON (1 second)

---

## 🚀 How to Build and Flash

1. Clone the repository:
   ```bash
   git clone [https://github.com/hilal-5/STM32G431-BareMetal-TrafficLight.git](https://github.com/hilal-5/STM32G431-BareMetal-TrafficLight.git)
