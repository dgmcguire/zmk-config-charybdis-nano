# zmk-config-charybdis-nano

ZMK config for a Charybdis Nano right-half bring-up on **Seeed XIAO BLE** with a **PMW3610** trackball.

This repo currently builds a **USB-only trackball test** shield (`charybdis_tb_test`) with no key matrix. Plug the XIAO in over USB and moving the ball should move the host cursor.

## Hardware

| Signal | XIAO | nRF52840 | PMW3610 |
|--------|------|----------|---------|
| NCS | D6 | P1.11 | NCS |
| MOTION | D7 | P1.12 | MOT |
| SCLK | D8 | P1.13 | SCL / SCLK |
| SDIO | D10 | P1.15 | SDI / SDIO |
| Power | 3V3 | — | **VDDIO only** |
| Ground | GND | — | GND |

- Leave module **VDD** open (onboard LDO).
- Leave **NRESET** floating.
- Board target: `seeeduino_xiao_ble`

## Build

GitHub Actions builds on push. Download the `firmware` artifact:

- `charybdis_tb_test-seeeduino_xiao_ble-zmk.uf2` — flash this for the trackball test
- `settings_reset-seeeduino_xiao_ble-zmk.uf2` — only if you need to clear settings/bonds

## Flash

1. Double-tap reset on the XIAO BLE (mounts as a UF2 drive).
2. Copy `charybdis_tb_test-seeeduino_xiao_ble-zmk.uf2` onto the drive.
3. Plug into the host over USB and move the ball.

## Axis tuning

If the cursor moves on the wrong axis or direction, edit
`boards/shields/charybdis_tb_test/charybdis_tb_test.overlay` under the
`trackball` node and add/remove:

- `swap-xy;`
- `invert-x;`
- `invert-y;`

Then rebuild and reflash.

## Out of scope (for now)

- Charlieplex / 17-key matrix / wake pin
- Split left half / BLE central-peripheral
- Scroll / snipe / automouse layers
