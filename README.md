# sram-breadboard-adapter

An 8 MB SRAM board for a breadboard: eight Lyontek LY62W10248ML (1M × 8, TSOP-II-44) in two banks of
four, on a 4-layer board with +5V and GND planes. J0 carries the address and control lines, J1 the data.
Built for [Sparky1](https://github.com/sparkybrit/sparky1), a 68030 breadboard computer.

## Things the KiCad files do not tell you

**The byte lanes are numbered for a little-endian CPU.** In each bank, chip 0 is on D7–D0 and chip 3 on
D31–D24:

| Chip enable | J0 pin | Data lines |
|---|---|---|
| `/CEA0`, `/CEB0` | 24, 28 | D7–D0 |
| `/CEA1`, `/CEB1` | 25, 29 | D15–D8 |
| `/CEA2`, `/CEB2` | 26, 30 | D23–D16 |
| `/CEA3`, `/CEB3` | 27, 31 | D31–D24 |

The 68030 is big-endian: its byte 0 is on D31–D24. So on Sparky1 the chip selects are crossed over on the
breadboard: the select for the CPU's lane 0 goes to `/CEx3`, lane 1 to `/CEx2`, and so on. The data pins are
wired straight, D0 to D0.

**The fitted board has extra bypass capacitors that are not in the layout.** 100 nF 0402 parts were soldered
by hand straight across the supply pins of each RAM chip (2026-10-02; Sparky1's notes record one across
pins 11/12 and one across 33/34 on all eight). The layout's own capacitors sit off the end of each package,
too far from the pins: with them alone the board gave an occasional single-bit read error, and with the
extra ones it ran 127 hours clean. A new revision should place two capacitors per chip under the pins; see
"Improve decoupling capacitor placement" in Sparky1's `CLAUDE.md`.
