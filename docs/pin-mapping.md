# Pin mapping — NUCLEO-G474RE ↔ BOOSTXL-DRV8323RHX

Board confirmed from schematic: **BOOSTXL-DRV8323RH** (MDBU017 Rev A),
hardware interface (no SPI), input range **8–54 V**.

## Sources to cross-check

- TI **SLAU732** — BOOSTXL-DRV8323Rx EVM User's Guide
- BoostXL schematic **MDBU017A** (read directly — values below)
- ST **UM2505** — NUCLEO-G474RE Arduino Uno V3 connector pinout
- **DRV8323 datasheet** — current sense amplifier gain table

## Hardware-configured values (all confirmed from schematic + datasheet)

| Parameter | Value | Status | What to set in MC Workbench |
|-----------|-------|--------|-----------------------------|
| PWM mode | **6x PWM** (INHx/INLx separate) | ✅ confirmed from schematic | 6-PWM |
| Shunt resistor | **0.007 Ω (7 mΩ)** (R6/R8/R10) | ✅ confirmed from schematic | Enter **0.007 Ω** |
| CSA gain | **10 V/V** | ✅ confirmed | Enter **10 V/V** |
| Input voltage range | 8–54 V | ✅ confirmed | — |

> **CSA gain resolved:** GAIN pin (DRV8323 pin 32) has **R22 = 47 kΩ tied to
> AGND**. Per the DRV8323 datasheet (Current Sense Amplifier, H/W Device):
> *GAIN = 47 kΩ ±5% tied to AGND → 10 V/V*. (Web search wrongly said 20 V/V;
> the datasheet is authoritative.)
>
> DRV8323RH H/W gain table for reference:
> | GAIN pin | Gain |
> |----------|------|
> | Tied to AGND (0 Ω) | 5 V/V |
> | **47 kΩ to AGND** | **10 V/V** |
> | Hi-Z (open) | 20 V/V |
> | Tied to DVDD | 40 V/V |

> **PWM mode resolved:** the schematic shows separate INHA/INLA, INHB/INLB,
> INHC/INLC inputs → **6x PWM**, fully compatible with X-CUBE-MCSDK FOC.
> A "Direct 1PWM Option" strap also exists but is not used here.

## Current-sense path notes (from schematic)

- Phase shunts **R6/R8/R10 = 0.007 Ω** on SPx/SNx.
- Differential CSA outputs go through **low-pass filters placed near the BP
  headers** (R18/R20 + C14/C15/C16 = 2200 pF) → SOA/SOB/SOC.
- DC-bus (VDRAIN) sense divider: **R28 82 kΩ / R32 4.99 kΩ** → VSENVM.

## BoostXL header pinout (BoosterPack) — VERIFY against schematic J1–J4

| FOC function | BoostXL header (to confirm) |
|--------------|------------------------------|
| PWM A / B / C | J4.13 / J4.14 / J4.31 |
| Current sense SOA / SOB / SOC | J1.4 / J1.5 / J1.6 |
| DC bus voltage sense | J1.2 |
| nFAULT | J2.19 |
| EN_GATE (enable) | J4.7 |
| DRVOFF | J4.40 |

## NUCLEO-G474RE Arduino pinout (from UM2505) — verify too

FOC-relevant pins, with the motor-control timer **TIM1** in mind:

| Arduino pin | G474 port/pin | Useful peripheral for FOC |
|-------------|---------------|---------------------------|
| D7 | **PA8** | **TIM1_CH1** (PWM phase A) |
| D8 | **PA9** | **TIM1_CH2** (PWM phase B) |
| D2 | **PA10** | **TIM1_CH3** (PWM phase C) |
| D9 | PC7 | TIM (verify) |
| D11 | PA7 | TIM1 complementary (verify) |
| D12 | PA6 | (verify) |
| D13 | PA5 | (verify) |
| A0 | **PA0** | ADC (current sense) |
| A1 | **PA1** | ADC (current sense) |
| A2 | PA4 | ADC / DAC |
| A3 | **PB0** | ADC (current sense) |
| A4 | PC1 | ADC |
| A5 | PC0 | ADC |

**Decoupling:** probably minimal, but put a **100 nF** close to each
current-sense line at the ADC input — that is where it actually helps.

## Filling this table

Final per-pin table to build (columns):
`BoostXL signal | BoostXL pin | Nucleo Arduino pin | G474 port/pin | G4 peripheral | FOC function | Aligned? / Jumper`
