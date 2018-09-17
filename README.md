## How to install OpenWrt or LEDE on TL-WA854RE
## French Language Version:[Version française](https://github.com/vincentoop/tech/blob/master/README-fr.md)

* I have successfully installed OpenWrt LEDE on my `TP-Link TL-WA854RE V2 EU` WiFi Range Extender. So I decide to write this article for those who have such device.  On the original firmware, wifi hangs up easily if several devices connected. But on OpenWrt, it is very very stable.

The link I've follwed is https://lists.openwrt.org/pipermail/openwrt-devel/2017-February/006197.html . But this firwmare don't have luci web interface, and don't  configed for failsafe.

So I build my own firmware from the source code.

The WiFi is enabled by default, and luci installed, and above all, I write a script for failsafe, if you make a mistake in configure so the wifi dissapears, just unplug and replug it, `it will return to default in case of wifi down`.

## OK, Let's begin
If your device is `WA854RE V1`, download and update this firmware: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v1-squashfs-factory.bin

If your device is `WA854RE V2`, download and update this firmware: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v2-squashfs-factory.bin

If your device is `WA854RE V2 EU`, download and update this firmware: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v2-squashfs-factory-eu.bin

If your device is `WA854RE V2 US`, download and update this firmware: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v2-squashfs-factory-us.bin

## If you want to `install latest openwrt`
you can just download and flash the wa850re squashfs-sysupgrade.bin file, wa850re and wa854re have the same hardware, but this file don't have luci web interface , don't configed for failsafe. So you have to build you own squashfs-sysupgrade.bin file, bu changing two files: rc.local and mac80211.sh .

If you want to `build your own firmware`, the lede 17.04 source for wa854re is https://github.com/vincentoop/tech/blob/master/source-master-lede-17.04-wa854re.zip , your have to change two files: rc.local and mac80211.sh . 

## Caution, `use at your own risk`
I don't know how to return to stock TP-Link firmware.

## edited /etc/rc.local :
```bash
# Retrun to default if Wifi does not appear in 3 minites.
sleep 180
if ifconfig br-lan | grep 'RX bytes' | grep '(0.0 B)'
    then
    mount_root
    umount /overlay
    firstboot -y
    reboot
fi
exit 0
```
## edited mac80211.sh :
```bash
#delete this line or set it to 0, to enable wifi on boot :
set wireless.radio${devidx}.disabled=0
```
## Thanks to:
* https://lists.openwrt.org/pipermail/openwrt-devel/2017-February/006197.html
* http://gadow.freifunk.net:8004/srv2/lede/tplink854/
* [OpenWrt](https://openwrt.org)
