# Motor and encoder parameters (for MC Workbench)

## Motor — Trinamic / Analog Devices QBL4208-100-04-025

| Parameter | Value | Source / note |
|-----------|-------|---------------|
| Type | 3-phase PMSM/BLDC, 42 mm (NEMA 17) | datasheet |
| Pole pairs | **4** (8 poles) | datasheet |
| Rated voltage | 24 VDC | datasheet |
| Rated power | ~104 W | datasheet |
| Rated speed | 4000 RPM | datasheet |
| Rated torque | 0.25 Nm | datasheet |
| Rated current | ~5–6 A | **read exact value from datasheet, do not guess** |
| Phase resistance Rs | TBD | from datasheet — needed for tuning |
| Phase inductance Ls | TBD | from datasheet — needed for tuning |
| BEMF constant Ke | TBD | from datasheet — needed for sensorless |

> Fill Rs / Ls / Ke from the official datasheet before tuning. MC Workbench can
> also estimate some of these with its motor profiler, but having the datasheet
> values as a sanity check is strongly recommended.

## Encoder — Kübler 8.3700.1332.0500

| Parameter | Value | Note |
|-----------|-------|------|
| Type | Incremental | |
| Resolution | 500 PPR | pulses per revolution |
| Counts after quadrature | **2000 CPR** | 500 × 4 |
| Use in MC Workbench | Encoder (quadrature) feedback | set 2000 CPR |

> 500 PPR is modest for low-speed smoothness. Fine for first bring-up and
> position loop; magnetic encoders can be evaluated later for the custom board.

## Power stage strap values (see docs/pin-mapping.md)

- CSA gain: **10 V/V** (verify on schematic)
- Shunt: **0.01 Ω** (verify on schematic)
- PWM mode: **3x vs 6x — confirm on schematic**
