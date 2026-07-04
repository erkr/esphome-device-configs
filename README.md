# esphome-device-configs
My personal configs for EspHome devices

## Simple passive BT-Proxy:
Example device config:
```YAML
substitutions:
  name: bluetooth-proxy-<MAC-Suffix>
  friendly_name: BT-Proxy-1

esphome:
  name: ${name}
  name_add_mac_suffix: false
  friendly_name: ${friendly_name}

packages:
  erkr.esphome-device-configs: github://erkr/esphome-device-configs/ESPHome/BT-Proxy.yaml@main

ota:
  platform: esphome
  password: !secret ota_password
   
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:
  auth:
    username: !secret web_server_username
    password: !secret web_server_password
```

## Passive BT-Proxy with RTTTL player:
Example device config:
```YAML
substitutions:
  name: bluetooth-proxy-<MAC-Suffix>
  friendly_name: BT-Proxy-4
  
esphome:
  name: ${name}
  name_add_mac_suffix: false
  friendly_name: ${friendly_name}

packages:
  erkr.esphome-device-configs: github://erkr/esphome-device-configs/ESPHome/BT-Proxy-RTTTL.yaml@main
  
ota:
  platform: esphome
  password: !secret ota_password
 
  
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

web_server:
  auth:
    username: !secret web_server_username
    password: !secret web_server_password
```
