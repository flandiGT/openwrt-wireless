openwrt-wireless
==============

configure wireless aspects of your openwrt system.
compare: [http://wiki.openwrt.org/doc/uci/wireless]

Role Variables
--------------

| variable name     | type             | element structure                                                                                        | default |
|-------------------|------------------|----------------------------------------------------------------------------------------------------------|---------|
| wifi-devices      | array of objects | {name: {{device_name}}, enabled: {{is_device_enabled}}}                                                  | <empty> |
| wifi-ifaces       | array of objects | {mode: {{iface_mode}}, ssid: {{iface_ssid}}, encryption: {{iface_encryption}}, device: {{iface_device}}} | <empty> |

Role Variable elements
----------------------

wifi-device attributes:

| attribute name | property type       | valid values / examples                                                      |
|----------------|---------------------|------------------------------------------------------------------------------|
| name           | text                | "radio0", "radio1" (any non-empty text, used to reference device)            |
| enabled        | boolean             | true / false                                                                 |
| channel        | option as int       | 36, 40, 44, 48, 52, 56, 60, 64, 149, 153, 157, 161, 165                      |
| txpower        | option as int       | 0, 4, 5, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17                             |
| country        | option as text      | Use ISO/IEC 3166 alpha2 country codes (US for United States, DE for Germany) |

wifi-iface attributes:

| attribute name | property type       | valid values / examples                                                         |
|----------------|---------------------|---------------------------------------------------------------------------------|
| index          | number              | optional: index of the wifi-iface                                               |
| mode           | option as text      | * ap = Access point                                                             |
|                |                     | * sta = Client                                                                  |
|                |                     | * mesh = 802.11s                                                                |
|                |                     | * ahdemo = Pseudo Ad-Hoc                                                        |
|                |                     | * monitor = Monitor                                                             |
| ssid           | text                | "mySSID", "OpenWRT" (any non-empty text )                                       |
| encryption     | option as text      | * none = No Encryption                                                          |
|                |                     | * wep-open = WEP Open System                                                    |
|                |                     | * wep-shared = WEP Shared Key                                                   |
|                |                     | * psk = WPA-PSK / Auto cipher                                                   |
|                |                     | * psk+tkip = WPA-PSK / Force TKIP                                               |
|                |                     | * psk+ccmp = WPA-PSK / Force CCMP (AES)                                         |
|                |                     | * psk+tkip+ccmp = WPA-PSK / Force TKIP and CCMP (AES)                           |
|                |                     | * psk2 = WPA2-PSK / Auto cipher                                                 |
|                |                     | * psk2+tkip = WPA2-PSK / Force TKIP                                             |
|                |                     | * psk2+ccmp = WPA2-PSK / Force CCMP (AES)                                       |
|                |                     | * psk2+tkip+ccmp = WPA2-PSK / Force TKIP and CCMP (AES)                         |
|                |                     | * psk-mixed = WPA-PSK/WPA2-PSK Mixed Mode                                       |
|                |                     | * psk-mixed+tkip = WPA-PSK/WPA2-PSK Mixed Mode / Force TKIP                     |
|                |                     | * psk-mixed+ccmp = WPA-PSK/WPA2-PSK Mixed Mode / Force CCMP (AES)               |
|                |                     | * psk-mixed+tkip+ccmp = WPA-PSK/WPA2-PSK Mixed Mode / Force TKIP and CCMP (AES) |
| device         | reference as text   | reference to wifi-device's name ("radio0, "radio1")                             |
| macfilter      | option as text      | * disable = no macfilter active                                                 |
|                |                     | * allow = Allow listed only                                                     |
|                |                     | * deny = Allow all except listed                                                |
| maclist        | array of strings    | mac addresses to allow or deny / ["XX:XX:XX:XX:XX:XX","XX:XX:XX:XX:XX:XX"]      |

Dependencies
------------

* openwrt-uci

Example Playbook
----------------

```
- role: openwrt-wireless
  wifi_devices: [{
    name: radio0,
    channel: 36,
    txpower: 17,
    country: DE,
    enabled: true
  }, {
    name: radio1,
    enabled: false
  }]

- role: openwrt-wireless
  wifi_ifaces: [{
    device: radio0,
    mode: ap,
    ssid: OpenWrt,
    encryption: psk2+ccmp,
    key: my_ultra_secret_key,
    macfilter: allow,
    maclist: [
      '01:23:45:67:89:AB',
      'FE:DC:BA:98:76:54'
    ]
  }, {
    device: radio1,
    mode: ap,
    ssid: OpenWrt,
    encryption: none
  }]
```

[http://wiki.openwrt.org/doc/uci/wireless]: http://wiki.openwrt.org/doc/uci/wireless
[https://github.com/lefant/ansible-openwrt-wireless]: https://github.com/lefant/ansible-openwrt-wireless
