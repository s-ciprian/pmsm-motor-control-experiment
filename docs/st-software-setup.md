# ST software to install (and in what order)

Since 2023, the **Motor Control Workbench** runs as a plugin inside
**STM32CubeMX**, so there is no separate Workbench installer anymore.

| # | Software | Role | Note |
|---|----------|------|------|
| 1 | **STM32CubeMX** | Configurator (pins / clock / peripherals) and host for MC Workbench | Needs Java; use the "full" installer |
| 2 | **X-CUBE-MCSDK** (6.x) | Motor Control SDK: MC Workbench (CubeMX plugin) + Motor Pilot + FOC libraries | Everything motor-related lives here. Free ST account required |
| 3 | **STM32CubeIDE** | Compile + flash + debug | Simplest path; Keil/IAR also possible |
| 4 | **Motor Pilot** | Live monitoring / tuning GUI | Ships with X-CUBE-MCSDK |

## Install order

1. Install **STM32CubeIDE** (it bundles CubeMX internally, but also install
   standalone CubeMX — the Workbench prefers the standalone one).
2. Install **STM32CubeMX** standalone.
3. Install **X-CUBE-MCSDK** (free ST registration). The installer adds the
   Workbench as a CubeMX plugin and installs Motor Pilot separately.
4. On first MC Workbench run, let it download the MC firmware package for the
   STM32G4 family.

## Interface for Programming and Communication

NUCLEO-G474RE has an on-board **ST-LINK V3** for flash and debug.
Motor Pilot link all go through the board's USB.
