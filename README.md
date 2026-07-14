# esphome-device-configs
My personal 'tailored' configs for EspHome devices.
The release binaries are fully functional when added to Home Assistant, but these pre-compiled firmware images don't have any credentials for OTA updates
and web access. 
The common approach is to add them to ESPHome device builder and extended the configuration with credentials for at least OTA and Web Server.
(examples below)

### Note 1: 
When adding devices to ESPHome device builder, a configuration file will be generated. Just add the credential part to that config.
Optional the API key can be removed from the generated config (like the examples).
Without api key, the network traffic is not encrypted, but the device is also easier transferred to other home assistant setups.

### Note 2: 
 - Use the firmware `factory` binary to initially flash a new device over USB [i.e ESPHome online flasher](https://web.esphome.io/)
 - Use 'OTA' binary for updating an existing esphome device via the web interface.

## General WiFi board functions:
Without manually configured or hard coded network credentails (e.g. release images. initial run) a Captive Portal will be launched to configure the WiFi.
When a configured board can't connect to the WiFi, the Captive Portal will only be launched after 1 day (security). 
Pressing the boot button for 5-15 seconds will erase all configured network credentials (factory reset).
After a factory reset, the board will reboot:
- My precompiled factory images will then launch the captive portal. 
- Adopted versions with hard coded WiFi credentials will reboot and try connect the WiFi first (for 1 day)
The on-board status led will blink, when the board is not connected to either Home Assistant or other clients.

## Simple passive BT-Proxy:
The board hosts a passive Bluetooth proxy. 

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
  # optionally hardcode Wi-Fi credentials (be aware those survive a factory reset)
  # otherwise the captive portal will allow to (re)configure WiFi credentials
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:
  auth:
    username: !secret web_server_username
    password: !secret web_server_password
```

## Passive BT-Proxy with RTTTL player:
This image adds both a RTTTL player and the passive BT proxy version. 
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
  # optionally hardcode Wi-Fi credentials (be aware those survive a factory reset)
  # otherwise the captive portal will allow to (re)configure WiFi credentials
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:
  auth:
    username: !secret web_server_username
    password: !secret web_server_password
```
