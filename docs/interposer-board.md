# Intermediate (interposer) board

The TI BOOSTXL-DRV8323RHX and the ST NUCLEO-G474RE do **not** mate
mechanically: the BoostXL uses BoosterPack headers, the Nucleo uses the
Arduino Uno V3 header layout. An adapter (interposer) board is therefore
**mandatory**, not optional. It will be built on a prototyping PCB with the
correct connectors and the routing done in copper.

## 1. What the interposer must route (in priority order)

1. **6 PWM signals** — 3 high-side + 3 low-side — from the Nucleo Arduino
   header (TIM1: PA8/PA9/PA10 plus the complementary channels, to be confirmed)
   to the BoostXL INHx/INLx inputs on its header.
2. **3 current-sense outputs (SOA/SOB/SOC)** from the BoostXL to 3 ADC pins of
   the G4. **These are the most sensitive signals** — keep the traces short,
   close to the connector, and add ~100 nF at the ADC input.
3. **Control signals** — nFAULT, EN_GATE (enable), DRVOFF, and VSENVM
   (DC-bus voltage sense).

> ⚠️ Do **not** route from the current `docs/pin-mapping.md` table yet — the
> J1–J4 / Arduino pin numbers there are still marked "VERIFY" (partly from web
> search). Before cutting copper, build the final pin-by-pin table by reading
> directly from:
> - the BoostXL header definitions in the TI schematic (J1–J4)
> - the Arduino header definitions in ST UM2505
> Otherwise a current-sense line could land on the wrong ADC pin — exactly the
> dangerous case.

## 2. Design tips for the prototype board

1. **Add test points / jumpers on EN_GATE and on the 3 current-sense lines.**
   During first bring-up you want to probe them with a multimeter / scope
   without unsoldering anything.
2. **Decide the PWM ↔ TIM1 mapping now.** Ideally route the 3 high-side signals
   to TIM1_CH1/CH2/CH3 (PA8/PA9/PA10) and the 3 low-side signals to the
   complementary channels TIM1_CH1N/CH2N/CH3N. Check in UM2505 which Arduino
   pins expose CH1N/CH2N/CH3N — **this determines whether they land on the
   header directly or need flying wires.**

## Final wiring table to build

Columns to fill while reading both schematics:

`Signal | BoostXL header.pin | Arduino pin | G474 pin | TIM1 / ADC | Note`
