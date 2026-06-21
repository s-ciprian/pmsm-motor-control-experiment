# PMSM Motor Control Experiment

An experiment in PMSM field-oriented control (FOC)
on STM32G4 with a TI BoostXL-DRV8323RHX power stage.


## Hardware under test

- MCU board: NUCLEO-G474RE (MB1367, rev C04)
- Power stage: TI BOOSTXL-DRV8323RHX (hardware interface, no SPI)
- Motor: Trinamic / Analog Devices QBL4208-100-04-025 (24 V, 4 pole pairs, ~104 W)
- Encoder: Kübler 8.3700.1332.0500 (500 PPR / 2000 CPR after quadrature)
- Debug DAC: TI DAC128S085EVM
- Bench supply: 30 V / 5 A adjustable

## TODO

Pin identification / mapping between NUCLEO-G474RE and BOOSTXL-DRV8323RHX.
See [`docs/pin-mapping.md`](docs/pin-mapping.md).

## Documentation

- [`docs/st-software-setup.md`](docs/st-software-setup.md) — ST tools to install and in what order
- [`docs/pin-mapping.md`](docs/pin-mapping.md) — NUCLEO-G474RE ↔ BoostXL pin map (work in progress)
- [`docs/motor-params.md`](docs/motor-params.md) — motor / encoder parameters for MC Workbench
