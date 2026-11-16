# Game of Life on iceBlinkPico

A SystemVerilog implementation of Conway's Game of Life running on an iceBlinkPico iCE40UP5K FPGA and an 8x8 WS2812B LED matrix.

Each LED represents one cell in a 64-cell toroidal grid. The design computes the next generation using the standard Game of Life rules, updates one color channel at a time, and continuously streams RGB data to the WS2812B matrix.

## Features

- 64-cell Conway's Game of Life simulation.
- Wraparound edges: cells on one edge consider the opposite edge adjacent.
- WS2812B serial timing implemented in SystemVerilog.
- Configurable display color using the board switches.
- Initial pattern loaded from the included `spiral/` RGB memory files.
- FPGA synthesis and programming targets for the iceBlinkPico board.

## Hardware

- iceBlinkPico board with an iCE40UP5K-SG48 FPGA.
- 8x8 WS2812B LED matrix connected to the board's matrix output pins.
- A 12 MHz clock supplied by the iceBlinkPico.

The included pin file maps the matrix output to `_48b` and `_45a`. Confirm the LED matrix wiring and power requirements before connecting hardware.

## Repository layout

- `top.sv` - top-level design and color-channel/game-state integration.
- `life.sv` - combinational Game of Life generation step.
- `controller.sv` - frame and pixel transmission controller.
- `ws2812b.sv` - WS2812B waveform generator.
- `memory.sv` - initial pattern memory loader.
- `spiral/` - initial red, green, and blue channel data.
- `iceBlinkPico.pcf` - board pin constraints.
- `Makefile` - synthesis, programming, and cleanup commands.
- `LICENSE` - BSD 2-Clause license inherited from the source project.

## Toolchain

Install the open-source FPGA toolchain:

- Yosys
- nextpnr-ice40
- icepack
- dfu-util

The project uses the iCE40 UP5K SG48 target.

## Build

From the repository root:

```sh
make build
```

This produces the synthesis JSON, place-and-route output, ASCII bitstream, and final `.bin` file.

## Program the board

```sh
make prog
```

The programming target uses `dfu-util` to load the bitstream into SRAM. Connect the board in the expected DFU mode before running it.

## Clean generated files

```sh
make clean
```

## Game of Life implementation

For each of the 64 cells, `life.sv` counts the eight neighboring cells. Live cells survive with two or three neighbors; dead cells become alive with exactly three neighbors. The modulo arithmetic makes the grid wrap at all four edges.

## Attribution

This project was isolated from the `examples/led_matrix` Game of Life example in [kennethxiong23/iceBlinkPico](https://github.com/kennethxiong23/iceBlinkPico). The board files and original project are by Bradley Minch and are distributed under the BSD 2-Clause License.