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

| attribute name | property type | valid values / examples                                           |
|----------------|---------------|-------------------------------------------------------------------|
| name           | text          | "radio0", "radio1" (any non-empty text, used to reference device) |
| enabled        | boolean       | true / false                                                      |

wifi-iface attributes:

| attribute name | property type       | valid values / examples                                                         |
|----------------|---------------------|---------------------------------------------------------------------------------|
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
| maclist        | list of text values | mac addresses to allow or deny / "XX:XX:XX:XX:XX:XX,XX:XX:XX:XX:XX:XX"         |

Dependencies
------------

* openwrt-uci

Example Playbook
----------------

```
- role: openwrt-wireless
  wifi_devices: [{
    name: radio0,
    type: mac80211,
    path: "pci0000:01/0000:01:00.0",
    channel: 36,
    hwmode: 11a,
    htmode: VTH80,
    enabled: false
  }, {
    name: radio1,
    type: mac80211,
    path: "platform/qca955x_wmac",
    channel: 11,
    hwmode: 11g,
    htmode: HT20,
    enabled: false
  }]

- role: openwrt-wireless
  wifi_ifaces: [{
    mode: ap,
    ssid: OpenWrt,
    encryption: none,
    device: radio0
  }, {
    mode: ap,
    ssid: OpenWrt,
    encryption: none,
    device: radio1
  }]
```

[http://wiki.openwrt.org/doc/uci/wireless]: http://wiki.openwrt.org/doc/uci/wireless
[https://github.com/lefant/ansible-openwrt-wireless]: https://github.com/lefant/ansible-openwrt-wireless
