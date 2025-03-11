Instructions taken from: https://github.com/victorlucachi/zmk-keyboards-totem

# Geist TOTEM ZMK Module

## Setting up your TOTEM

- Open the following [link](https://github.com/zmkfirmware/unified-zmk-config-template), click the green `Use this template` button in the upper right corner, and create a new repository
- Navigate to your newly created repository
- Edit the `config/west.yml` file and add the TOTEM module:

```
manifest:
  remotes:
  - name: zmkfirmware
    url-base: https://github.com/zmkfirmware
  projects:
  - name: zmk
    remote: zmkfirmware
    revision: main
    import: app/west.yml
  - name: zmk-keyboards-totem
    url: https://github.com/victorlucachi/zmk-keyboards-totem
    revision: main
    path: modules/zmk-keyboards-totem
  self:
    path: config
```

- Copy [the contents of the default keymap file](boards/shields/totem/totem.keymap), create a new `config/totem.keymap` file, paste and edit to customize the keymap
- Edit the `build.yml` file and add the TOTEM to the build list:

Dongle setup:
```
include:
  - board: seeeduino_xiao_ble
    shield: totem_dongle
    snippet: studio-rpc-usb-uart
    cmake-args: -DCONFIG_ZMK_STUDIO=y  
  - board: seeeduino_xiao_ble
    shield: totem_left
    cmake-args: -DCONFIG_ZMK_SPLIT_ROLE_CENTRAL=n
  - board: seeeduino_xiao_ble
    shield: totem_right
  - board: seeeduino_xiao_ble
    shield: settings_reset
```

Classic setup:
```
include:
  - board: seeeduino_xiao_ble
    shield: totem_left
    snippet: studio-rpc-usb-uart
    cmake-args: -DCONFIG_ZMK_STUDIO=y  
  - board: seeeduino_xiao_ble
    shield: totem_right
  - board: seeeduino_xiao_ble
    shield: settings_reset
```

> [!NOTE]  
> If you're using a nice!nano controller for the dongle, replace the board for your totem_dongle shield in the section above, from `seeeduino_xiao_ble` to `nice_nano_v2`

- After you're done customizing your keymap push the changes, and open the Github `Actions` tab of your config repository
- Open the latest workflow run, and after it's done building click on the `firmware` link under the Artifacts section of the page

> [!CAUTION]
> Double check the names of the .uf2 files prior to copying them to your controller. Flashing the wrong reset firmware (eg: seeeduino_xiao_ble reset flashed on a nice_nano_v2 controller) might brick your controller, requiring additional hardware in order to fix it.

## Misc

#### ZMK Studio

The shield is ZMK Studio enabled; ZMK Studio is in early alpha, the web gui (works only with the central half connected via USB; for BLE Studio you have to use one of the ZMK Studio native apps available [here](https://github.com/zmkfirmware/zmk-studio/actions)) is available [here](https://zmk.studio/).

If you want to skip the unlocking step, create a `config/totem.conf` file, and add `CONFIG_ZMK_STUDIO_LOCKING=n` to it, otherwise assign the `&studio_unlock` binding to your keymap.
