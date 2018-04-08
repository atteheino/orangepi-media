# ORANGE PI - Server setup OSX

# BURNING IMAGE TO SD 
Täältä lisää ohjeita: http://rayhightower.com/blog/2015/11/27/orange-pi-mini-2-setup-for-mac-os-x/

* Lataa image orange pi sivustolta
  * Imaget Orange PI Sivustolta ei toimi.
* Lataa image ARMBIAN sivustolta:
`https://www.armbian.com/orange-pi-pc2/`

* Pura image
* Syötä sisään sd kortti

* Kurkkaa mikä on oikea disk:
`diskutil list`

* Poista disk
`diskutil unmountDisk /dev/disk(N)`

* Siirrä image levylle:
`sudo dd bs=1m if=IMAGE.img of=/dev/disk2`

# Logging into linux

Armbian default login is:
root / 1234
Need to change that at first login. 

Create new user for normal user.

# Setup after login

Update & upgrade system:
`# sudo apt-get update && sudo apt-get upgrade`

Change keyboard layout to Finnish if needed:
`# sudo dpkg-reconfigure keyboard-configuration`
`# sudo service keyboard-setup restart`
`# sudo setupcon`

Force correct layout by adding call to setupcon into rc.local:
`# sudo vim /etc/rc.local`

Might need to restart to make active.

# Config FTP server



# get s3 upload software from GIT



