# Motor and encoder parameters
Will be use some (all?) parameters in MC Workbench.

## Motor — Trinamic / Analog Devices QBL4208-100-04-025

| Parameter           | Value                              | Source / note                          |
|---------------------|------------------------------------|----------------------------------------|
| Type                | 3-phase PMSM/BLDC, 42 mm (NEMA 17) | datasheet                              |
| Pole pairs          | **4** (8 poles)                    | datasheet                              |
| Rated voltage       | 24 VDC                             | datasheet                              |
| Rated power         | ~104 W                             | datasheet                              |
| Rated speed         | 4000 RPM                           | datasheet                              |
| Rated torque        | 0.25 Nm                            | datasheet                              |
| Rated current       | ~5–6 A?                            | **read exact value from datasheet**    |
| Phase resistance Rs | TBD                                | from datasheet — needed for tuning     |
| Phase inductance Ls | TBD                                | from datasheet — needed for tuning     |
| BEMF constant Ke    | TBD                                | from datasheet — needed for sensorless |


## Encoder — Kübler 8.3700.1332.0500

| Parameter               | Value                         | Note                  |
|-------------------------|-------------------------------|-----------------------|
| Type                    | Incremental                   |                       |
| Resolution              | 500 PPR                       | pulses per revolution |
| Counts after quadrature | **2000 CPR**                  | 500 × 4               |
| Use in MC Workbench     | Encoder (quadrature) feedback | set 2000 CPR          |

