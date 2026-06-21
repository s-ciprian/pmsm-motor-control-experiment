# Pin mapping — NUCLEO-G474RE ↔ BOOSTXL-DRV8323RHX

## Sources to cross-check

- TI **SLAU732** — BOOSTXL-DRV8323Rx EVM User's Guide + schematic
  (Table 4-1 "Header Pinout", GAIN0/1 straps, MODE strap, R20/R21/R22 shunts)
- ST **UM2505** — NUCLEO-G474RE Arduino Uno V3 connector pinout
  (Tables for CN5/CN6/CN8/CN9)
- **DRV8323RH datasheet** — gain options (5/10/20/40 V/V) and PWM modes

## Hardware-configured values to read from the BoostXL schematic

| Parameter | Value (from SLAU732, via web) | Where to verify | What to set in MC Workbench |
|-----------|-------------------------------|-----------------|-----------------------------|
| CSA gain | **10 V/V** (GAIN1=H, GAIN0=L) | Schematic, GAIN0/GAIN1 pins | Enter **10 V/V** |
| Shunt resistor | **10 mΩ (0.01 Ω)** | R20 / R21 / R22 near phases | Enter **0.01 Ω** |
| PWM mode | ⚠️ **3x vs 6x — UNCONFIRMED** | MODE pin strap | Must match the strap |

> ** X-CUBE-MCSDK FOC normally uses **6x PWM**
> (independent high/low per leg). If the BoostXL is strapped for **3x**,
> they are not compatible without changing one side. Read the MODE pin on the
> schematic first and decide: re-strap to 6x on the interposer, or configure
> MCSDK for 3x.

## BoostXL header pinout (BoosterPack) — VERIFY

| FOC function | BoostXL header (web — unconfirmed) |
|--------------|-------------------------------------|
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
