# 🔧 STM32F446RE: Output HSI Clock via MCO1 (PA8)

This bare-metal STM32 project configures the **MCO1 pin (PA8)** to output the **internal HSI clock (16 MHz)** using direct register access — no HAL or Cube drivers.

---

## ✅ Overview

- **Board**: STM32F446RE Nucleo
- **Pin Used**: `PA8` (MCO1 output)
- **Clock Source**: `HSI` (internal 16 MHz oscillator)
- **Mode**: Alternate Function 0 (AF0)
- **Observation Tool**: Logic Analyzer or Oscilloscope

---

## 🔧 Register-Level Configuration

| Step | Description                         | Register / Bitfield                |
|------|-------------------------------------|------------------------------------|
| 1    | Enable GPIOA clock                  | `RCC_AHB1ENR |= (1 << 0)`          |
| 2    | Set PA8 to Alternate Function mode  | `GPIOA_MODER[17:16] = 0b10`        |
| 3    | Set PA8 to AF0                      | `GPIOA_AFRH[3:0] = 0b0000`         |
| 4    | Select HSI as MCO1 source           | `RCC_CFGR[22:21] = 0b00`           |

---

## 🧪 Output Verification

1. Connect a logic analyzer or oscilloscope probe to **PA8**
2. You should see a square wave at **16 MHz** (HSI)
3. You can optionally prescale the clock using `MCO1PRE[26:24]`

---

## 🚀 Optional Enhancements

To add a prescaler (e.g., HSI / 4):
```c
*rcc_config_addr &= ~(0x7 << 24);  // Clear MCO1PRE
*rcc_config_addr |=  (0x2 << 24);  // Set divide by 4

