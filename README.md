openwrt-wireless
==============

configure wireless aspects of your openwrt system.
compare: [http://wiki.openwrt.org/doc/uci/wireless]

Dependencies
------------

* [openwrt-uci](https://github.com/flandiGT/openwrt-uci)

Role Variables
--------------

| variable name     | type             | default | element structure       |
|-------------------|------------------|---------|-------------------------|
| wifi-devices      | array of objects | []      | see documentation below |
| wifi-ifaces       | array of objects | []      | see documentation below |

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
| mode           | option as text      | (see official documentation)                                                    |
| ssid           | text                | "mySSID", "OpenWRT" (any non-empty text )                                       |
| encryption     | option as text      | (see official documentation)                                                    |
| device         | reference as text   | reference to wifi-device's name ("radio0, "radio1")                             |
| macfilter      | option as text      | disable / allow / deny                                                          |
| maclist        | array of strings    | mac addresses to allow or deny / ["XX:XX:XX:XX:XX:XX","XX:XX:XX:XX:XX:XX"]      |

Example Playbook
----------------

```
- role: openwrt-wireless
  wifi_devices:
    - name: radio0
      channel: 36
      txpower: 17
      country: DE
      enabled: true
    - name: radio1
      enabled: true
  wifi_ifaces:
  - device: radio0
    mode: ap
    ssid: OpenWrt_secured
    encryption: psk2+ccmp
    key: my_ultra_secret_key
    network: lan
  - device: radio1
    mode: ap
    ssid: OpenWrt_open
    encryption: none
  - device: radio0
    mode: ap
    ssid: OpenWrt_with_macfilter
    encryption: none
    macfilter: allow
    maclist:
    - '01:23:45:67:89:AB'
    - 'FE:DC:BA:98:76:54'
```

Official documentation:
* [OpenWRT Wiki / Wireless configuration](http://wiki.openwrt.org/doc/uci/wireless)

Got idea from:
* [lefant/ansible-openwrt-wireless](https://github.com/lefant/ansible-openwrt-wireless)
