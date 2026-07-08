# esphome-device-configs
My personal tailored configs for EspHome devices.
The release binaries are fully functional when added to Home Assistant,
but when using the compiled firmware images, devices don't have any credentials for OTA updates
and web access. 
The common approach is to add them to ESPHome device builder 
and extended the configuration with your credentials.
(examples below)

### Note 1: 
When adding devices to ESPHome device builder, a
configuration file will be generated. Just add the 
credential part to that config.
Optional the API key can be removed from the generated config (like the examples).
Without api key, the network traffic is not encrypted, but
the device is also easier transferred to other 
home assistant setups.

### Note 2: 
 - Use the firmware `factory` binary to initially flash a new device over USB [i.e ESPHome online flasher](https://web.esphome.io/)
 - Use 'OTA' binary for updating an existing esphome device via the web interface.


## Simple passive BT-Proxy:
The board hosts a passive Bluetooth proxy. 
When it can't connect to the configured WiFi, 
a Captive Portal will be launched for max 10 minutes. 
after that it will do a retry connecting the WiFi.
The on-board status led will blink, when the board
is not connected to either Home Assistant or other clients.
The boot button will erase user configured network credentials (factory reset)
After a factory reset, the precompiled firmware will reboot
and launch the captive portal. 
Adopted versions with hard coded credentials Will reboot using those.

Example device config:
```YAML
substitutions:
  name: bluetooth-proxy-<MAC-Suffix>
  friendly_name: BT-Proxy-1

esphome:
  name_add_mac_suffix: false
  comment: ${device_description}

packages:
  erkr.esphome-device-configs: github://erkr/esphome-device-configs/ESPHome/BT-Proxy.yaml@main

ota:
  platform: esphome
  password: !secret ota_password
   
wifi:
  # optionally hardcode Wi-Fi credentials (survive factory reset)
  # otherwise (re)configure in captive portal
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:
  auth:
    username: !secret web_server_username
    password: !secret web_server_password
```

## Passive BT-Proxy with RTTTL player:
This image adds an RTTTL player to the BT proxy version. 
When starting a song (RTTTL syntax) it will abort what was still playing.

Example device config:
```YAML
substitutions:
  name: bluetooth-proxy-<MAC-Suffix>
  friendly_name: BT-Proxy-4
  
esphome:
  name_add_mac_suffix: false
  comment: ${device_description}

packages:
  erkr.esphome-device-configs: github://erkr/esphome-device-configs/ESPHome/BT-Proxy-RTTTL.yaml@main
  
ota:
  platform: esphome
  password: !secret ota_password
 
  
wifi:
  # optionally hardcode Wi-Fi credentials (survive factory reset)
  # otherwise (re)configure in captive portal
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:
  auth:
    username: !secret web_server_username
    password: !secret web_server_password
```
