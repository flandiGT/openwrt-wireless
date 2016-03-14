openwrt-wireless
==============

configure wireless aspects of your openwrt system.
compare: [http://wiki.openwrt.org/doc/uci/wireless]

Role Variables
--------------

| variable name     | type                   | default |
|-------------------|------------------------|---------|
| wifi-devices      | array of objects       | <empty> |
| wifi-ifaces       | array of objects       | <empty> |


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
