# Intermediate (interposer) board

The TI BOOSTXL-DRV8323RHX and the ST NUCLEO-G474RE do **not** mate
mechanically: the BoostXL uses BoosterPack headers, the Nucleo uses the
Arduino Uno V3 header layout. An adapter (interposer) board is therefore
needed. It will be built on a prototyping PCB with the
correct connectors and the routing done in copper.

## 1. What the intermediate PCB must route (in priority order)

1. **6 PWM signals** — 3 high-side + 3 low-side — from the Nucleo
   header (TIM1: PA8/PA9/PA10 plus the complementary channels, to be confirmed)
   to the BoostXL INHx/INLx inputs on its header.
2. **3 current-sense outputs (SOA/SOB/SOC)** from the BoostXL to 3 ADC pins of
   the G4. **These are the most sensitive signals** — keep the traces short,
   close to the connector, and add ~100 nF at the ADC input.
3. **Control signals** — nFAULT, EN_GATE (enable), DRVOFF, and VSENVM
   (DC-bus voltage sense).

## 2. Design notes for the prototype board

1. **Add test points / jumpers on EN_GATE and on the 3 current-sense lines.**
   During first bring-up you want to probe them with a multimeter / scope
   without unsoldering anything.
2. **Decide the PWM ↔ TIM1 mapping now.** Ideally route the 3 high-side signals
   to TIM1_CH1/CH2/CH3 (PA8/PA9/PA10) and the 3 low-side signals to the
   complementary channels TIM1_CH1N/CH2N/CH3N. Check in UM2505 which Arduino
   pins expose CH1N/CH2N/CH3N — **this determines whether they land on the
   header directly or need flying wires.**

## Wiring table to build

Columns to fill while reading both schematics:

`Signal | BoostXL header.pin | G474 pin | TIM1 / ADC | Note`
