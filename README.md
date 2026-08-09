# zmk-config-charybdis-nano

ZMK config for a Charybdis Nano right-half bring-up on **Seeed XIAO BLE** with a **PMW3610** trackball.

This repo currently builds **USB-only trackball test** shields with no key matrix. Plug the XIAO in over USB and moving the ball should move the host cursor.

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

## Builds

GitHub Actions builds on push. Download the `firmware` artifact:

| UF2 | Use |
|-----|-----|
| `charybdis_tb_test-seeeduino_xiao_ble-zmk.uf2` | **High power** — force-awake sensor, snappy USB bring-up |
| `charybdis_tb_lp-seeeduino_xiao_ble-zmk.uf2` | **Low power** — normal sensor downshift + ZMK sleep |
| `settings_reset-seeeduino_xiao_ble-zmk.uf2` | Clear settings/bonds only if needed |

### Suggested test order

1. Flash **high power** first and confirm the cursor moves.
2. Flash **low power** and confirm the cursor still moves (may feel slightly less snappy; idle sleep after 15s).

Same wiring for both — only firmware differs.

## Flash

1. Double-tap reset on the XIAO BLE (mounts as a UF2 drive).
2. Copy the UF2 you want onto the drive.
3. Plug into the host over USB and move the ball.

## Axis tuning

If the cursor moves on the wrong axis or direction, edit
`boards/shields/charybdis_tb_test/charybdis_tb_test.dtsi` under the
`trackball` node and add/remove:

- `swap-xy;`
- `invert-x;`
- `invert-y;`

Then rebuild and reflash.

## Out of scope (for now)

- Charlieplex / 17-key matrix / wake pin
- Split left half / BLE central-peripheral
- Scroll / snipe / automouse layers
