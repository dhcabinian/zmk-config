# Building the firmware
## Default Keymap

Shield name comes from `Kconfig.shield`: `dactyl_left` and `dactyl_right`

```
west build -d build/dactyl_left -p -b nice_nano_v2 -- -DSHIELD="dactyl_left nice_view_adapter nice_view"
west build -d build/dactyl_right -p -b nice_nano_v2 -- -DSHIELD="dactyl_right nice_view_adapter nice_view"
```

## Custom Keymap
Shield name comes from `Kconfig.shield`: `dactyl_left` and `dactyl_right`

```
west build -d build/dactyl_left -p -b nice_nano_v2 -- -DSHIELD="dactyl_left nice_view_adapter nice_view" -DZMK_CONFIG="/workspaces/zmk-config/[yourName]/leeloo_v2/config"
west build -d build/dactyl_right -p -b nice_nano_v2 -- -DSHIELD="dactyl_right nice_view_adapter nice_view" -DZMK_CONFIG="/workspaces/zmk-config/[yourName]/leeloo_v2/config"
```


# Pinouts
## [Nice!Nanov2](https://nicekeyboards.com/docs/nice-nano/pinout-schematic)

### RGB
- Docs use &spi3 on P0.06 (D1)
- Must use high frequency pin
- RGB strips only use 3 pins
  - VCC
  - GND
  - Din = D1


### Nice!View
- P0.06 is taken up by RGB.
- Leelo (which this was templated from) uses (D4) for spi0 CS pin
- The remaining data pins come from the `nice_view_adapter`
  - SCK = NRF20 (D3?)
  - MOSI = NRF17 (D2?)
  - MISO = NRF25 (also known as AC21 which does not have an output on NiceNanov2)


### Keys
- What pins are available?
  - D0
  - D5-D9
  - D10-21

## [Nice!View](https://nicekeyboards.com/docs/nice-view/pinout-schematic)
Left to Right (througholes below screen):
  - MOSI
  - SCK
  - VCC
  - GND
  - CS
    - Create a bodge wire from the CS pin to the Arduino Digital 1 (D1) Pro Micro pin (P0.06/006 on the nice!nano)
    - If the D1 pin is unavailable, you'll need to override the cs-gpios on the adapter or define your own &nice_view_spi bus without the nice_view_adapter.

- nice!view is a SSD1306 OLED replacement
- similar pinout to SSD1306 OLEDs with one extra pin
- 3-wire SPI protocol
- 3.3V voltage

# Protocols

## [SPI](https://embeddedexplorer.com/nrf52-spi-tutorial/)
- 4 Wires
  - SCK
  - MISO
  - MOSI
  - CS
- nRF52832 has up to 3 SPI instances: SPI0,SPI1,SPI2
- SPIM is SPI w/ DMA (SPI is technically deprecated)
- Max SPI frequency is based on the chip: nRF52832 is max 8 MHz
- Nice!Nano is a Nordic nRF52840
