# ORANGE PI PC2 - Server setup OSX

# BURNING IMAGE TO SD 
More instructions here: http://rayhightower.com/blog/2015/11/27/orange-pi-mini-2-setup-for-mac-os-x/

* Images from Orange PI site did not work for me.
* Download working image from ARMBIAN site:
`https://www.armbian.com/orange-pi-pc2/`

* Unpack image
* Insert SD Card

* See what is the correct disk to use:
`diskutil list`

* Eject disk
`diskutil unmountDisk /dev/disk(N)`

* Write image to disk
`sudo dd bs=1m if=IMAGE.img of=/dev/disk2`

# Logging into linux

* Armbian default login is:
root / 1234

* Need to change that at first login. 

* Create new user for normal user.

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

# Set static IP Address

IP address can be set in the /etc/network/interfaces file. (https://www.swiftstack.com/docs/install/configure_networking.html)

`# sudo vim /etc/network/interfaces`

Set the following values:

* Change from dhcp to static in line "iface eth0 inet dhcp"
* Set following lines to correct values:
	address 10.0.0.41
    netmask 255.255.255.0
    gateway 10.0.0.1
    dns-nameservers 10.0.0.1
	
Typically in home routers dns-nameservers value is the same as gateway value. 

# Config FTP server

See (https://wiki.debian.org/vsftpd)

Install:

`# sudo aptitude install vsftpd`

Check that FTP is running by running:

`# sudo netstat -npl`

Look for this line:

`tcp6       0      0 :::21                   :::*                    LISTEN      1678/vsftpd`

Configure server:

`# sudo vim /etc/vsftpd.conf`

* To enable write access:
 * write_enable=YES
* To restrict user to their home dir:
 * chroot_local_user=YES
* To create files with correct permissions:
 * local_umask=022
 
Restart to make config changes apply:

`sudo service vsftpd restart` 

Add user for FTP Access (Hint.. camera did not support special characters :( )):

`# sudo adduser videoftp`

Create home dir:

`# sudo mkdir /var/ftp`

`# sudo mkdir /var/ftp/videoftp`

Set as FTP HOME: 

`# sudo usermod -d /var/ftp/videoftp/ videoftp`

Set permissions to chroot:

`# sudo chown root:videoftp videoftp`

Create file for files and set permissions:

`# sudo mkdir files`

`# sudo chmod g+wrx files`

`# sudo chown videoftp:videoftp files`


# Create S3 Bucket & IAM Policy, Group & User

Login to AWS Management console.
In S3 module, create new S3 bucket with no public access.

In IAM module, create IAM Policy for writing to the new S3 bucket.

* Select S3 as service
* Grant List & Write
	* Set Resource, Bucket to filter by the new S3 bucket name.
* Name: valvontavideot-s3-upload-policy

In IAM Module, create new IAM Group:

* Name: valvontavideot-s3upload-group
* Select the valvontavideot-s3-upload-policy for the Group policy


In IAM Module, create new IAM User:

* Name: valvontavideot-s3upload-user
* Access type: Programmatic access
* Group: valvontavideot-s3upload-group

Store AccessKey & Secret to some secure place. You'll need it later.



# get s3 upload software from GIT

Create user for the software:

`# sudo adduser s3upload`

Add user to group videoftp group so that the user has access to uploaded videos:

`# sudo adduser s3upload videoftp`

Clone S3 uploader software from GitHub:

`# sudo git clone https://github.com/atteheino/s3uploader.git`

Make s3upload the owner of the directory:

`# sudo chown -R s3upload:s3upload s3uploader`

Build the software:

`# make server_linux`


# Setup AWS CLI

Let's setup AWS CLI to use Boto Profiles for the upload software to use.
Installing the 

`# sudo su - s3upload`

`# pip3 install awscli --upgrade --user`

Set path to include awscli:
Add this to .profile of s3upload user
`For AWS CLI:
PATH=~/.local/bin:$PATH`

Finally: 

`# source .profile`


Try if awscli works:

`# aws --version`

# Configure AWS CLI credentials

as s3upload user:

`# aws configure`

region = eu-west-1
output = json

