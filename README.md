# ORANGE PI - Server setup OSX

# BURNING IMAGE TO SD 
Täältä lisää ohjeita: http://rayhightower.com/blog/2015/11/27/orange-pi-mini-2-setup-for-mac-os-x/

* Lataa image orange pi sivustolta
* Syötä sisään sd kortti

* Kurkkaa mikä on oikea disk:
`diskutil list`

* Poista disk
`diskutil unmountDisk /dev/disk(N)`

* Siirrä image levylle:
`sudo dd bs=4m if=Arch_Server_PC2_V0_9_1.img of=/dev/disk2`

