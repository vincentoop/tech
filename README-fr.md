## Comment installer OpenWrt ou LEDE sur TL-WA854RE
## English Language Version:[English Language](https://github.com/vincentoop/tech/blob/master/README.md)
* J'ai installé avec succès OpenWrt LEDE sur mon répéteur WiFi `TP-Link TL-WA854RE V2 EU` . Je décide donc d'écrire cet article pour ceux qui ont un tel appareil. Sur le firmware d'origine, le wifi se raccroche facilement si plusieurs appareils sont connectés. Mais sur OpenWrt, c'est très très stable.

Le lien que j'ai suivi est https://lists.openwrt.org/pipermail/openwrt-devel/2017-February/006197.html . Mais ce firewmare n’a pas d’interface web luci, et n’est pas configuré en cas de mal configuré.

Donc, je compilé mon propre firmware à partir du code source.

Le WiFi est activé par défaut, et luci installé, et surtout, j'écris un script en cas de mal configuré, si vous faites une erreur dans configure et le wifi disparaisse, il suffit de le débrancher et de le rebrancher. Il revient au défault.

## OK, commençons
Si votre appareil est `WA854RE V1`, téléchargez et mettez à jour ce micrologiciel: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v1-squashfs-factory.bin

Si votre appareil est `WA854RE V2`, téléchargez et mettez à jour ce micrologiciel: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v2-squashfs-factory.bin

Si votre appareil est `WA854RE V2 EU`, téléchargez et mettez à jour ce micrologiciel: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v2-squashfs-factory-eu.bin

Si votre appareil est `WA854RE V2 US`, téléchargez et mettez à jour ce micrologiciel: https://github.com/vincentoop/tech/blob/master/lede-ar71xx-generic-tl-wa854re-v2-squashfs-factory-us.bin

## Si vous voulez `installer le dernier version openwrt`
vous pouvez simplement télécharger et flasher le fichier wa850re squashfs-sysupgrade.bin, wa850re et wa854re avoir le même matériel, mais ce fichier n'a pas d'interface web luci, n'est pas configuré en cas de mal configuré. Vous devez donc créer votre propre fichier squashfs-sysupgrade.bin, en changeant deux fichiers: rc.local et mac80211.sh.

Si vous voulez `compiler votre propre firmware`, la source lede 17.04 pour wa854re est https://github.com/vincentoop/tech/blob/master/source-master-lede-17.04-wa854re.zip, vous devez changer deux fichiers: rc.local et mac80211.sh.

## Attention, `utiliser à vos risques et périls`
Je ne sais pas comment retourner au stock firmware du TP-Link.

## édité /etc/rc.local :
```bash
# Retourner à défaut si le Wifi n'apparaît pas dans 3 minites.
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
## édité mac80211.sh :
```bash
#enlever cette ligne ou mettez-la à 0 pour activer le wifi au démarrage
set wireless.radio${devidx}.disabled=0
```
## Grâce à:
* https://lists.openwrt.org/pipermail/openwrt-devel/2017-February/006197.html
* http://gadow.freifunk.net:8004/srv2/lede/tplink854/
* [OpenWrt](https://openwrt.org)
