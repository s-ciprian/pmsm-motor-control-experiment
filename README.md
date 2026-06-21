# PMSM Motor Control Experiment

A personal, spare-time experiment in PMSM field-oriented control (FOC)
on STM32G4 with a TI BoostXL-DRV8323RHX power stage.

This is a learning project. No guarantees, no roadmap promises.

## Hardware under test

- MCU board: NUCLEO-G474RE (MB1367, rev C04)
- Power stage: TI BOOSTXL-DRV8323RHX (hardware interface, no SPI)
- Motor: Trinamic / Analog Devices QBL4208-100-04-025 (24 V, 4 pole pairs, ~104 W)
- Encoder: Kübler 8.3700.1332.0500 (500 PPR / 2000 CPR after quadrature)
- Debug DAC: TI DAC128S085EVM
- Bench supply: 30 V / 5 A adjustable (current-limited during bring-up)
- Reference DSP board (optional, "golden" setup): TI LAUNCHXL-F280025C
- Interposer: custom protoboard planned between Nucleo and BoostXL
  (keeps the analog current-sense traces short, allows pin re-routing)

## Current focus

Pin identification / mapping between NUCLEO-G474RE and BOOSTXL-DRV8323RHX.
See [`docs/pin-mapping.md`](docs/pin-mapping.md).

## Documentation

- [`docs/st-software-setup.md`](docs/st-software-setup.md) — ST tools to install and in what order
- [`docs/pin-mapping.md`](docs/pin-mapping.md) — NUCLEO-G474RE ↔ BoostXL pin map (work in progress)
- [`docs/motor-params.md`](docs/motor-params.md) — motor / encoder parameters for MC Workbench

## Binaries

Build binaries (`.elf`, `.bin`, `.hex`, `.map`) are **not** committed to Git
(see `.gitignore`). They are published as **GitHub Releases** instead, so they
are available without rebuilding and without bloating the Git history.

## Important safety notes

- Current-sense gain on the **RH** variant is **hardware-strapped** on the
  BoostXL and must be read from the schematic and entered identically in
  MC Workbench. A wrong gain value makes the current loop compute wrong
  amps → dangerous behaviour.
- PWM mode is hardware-strapped on the BoostXL (to be confirmed from schematic:
  3x vs 6x — this affects MC Workbench configuration directly).
- During first bring-up, keep the bench supply current limit low (~1 A) as a
  physical safety net.
