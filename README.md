# Setup Raspberry Pi as media server with torrent download and smb server. (Setup done on Raspberry Pi 4B)
I have created a simple step by step process used to install apps and packages on Raspberry Pi.

  1. [Update all the packages to the latest version](https://github.com/chetanjain2099/Setup_Raspberry_Pi/blob/main/README.md#1-update-all-the-packages)
  2. [Setup time zone]([url](https://github.com/chetanjain2099/Setup_Raspberry_Pi/blob/main/README.md#1-update-all-the-packages))
  3. [Automatically mount USB Drive](https://github.com/chetanjain2099/Setup_Raspberry_Pi/blob/main/README.md#1-update-all-the-packages)
  4. [Install smb Server using Samba](https://github.com/chetanjain2099/Setup_Raspberry_Pi/blob/main/README.md#1-update-all-the-packages)
  5. [Install Speedtest to test the internet speed](https://github.com/chetanjain2099/Setup_Raspberry_Pi/blob/main/README.md#1-update-all-the-packages)
  6. Over Clocking the CPU to boost the performance
  7. StressTest
  8. QBittorrent
  9. Jellyfin

## 1. Update all the packages
```
sudo apt update && sudo apt upgrade
```

## 2. Setup date and time correctly (Set time zone).
> Run the command -> Select Localization option from the menu -> Select Timezone -> Select Area -> Select City/Region
```
sudo raspi-config
```

## 3. Auto mount usb drives
#### - Create a diretry to mount the USB Drive.
```
mkdir /mnt/usb
```

#### - Find the UUID of the USB drive to mount (Notedown UUID and Type of the drive you want to mount)
```
sudo blkid
```

#### - Create a backup of the file and edit it.
```
sudo cp /etc/fstab /etc/fstab.back
sudo nano /etc/fstab
```
#### - Use one of the below code to mount the drive. Don't forget to change UUID and the mount point. Close and Save the file
  #####   FAT `UUID=FC05-DF26 /mnt/usb0 vfat defaults,auto,users,rw,nofail,umask=000 0 0`
  #####   NTFS `UUID=FC05-DF26 /mnt/usb0 ntfs defaults,auto,users,rw,nofail,umask=000 0 0`
  #####   exFAT `UUID=FC05-DF26 /mnt/usb0 exfat defaults,auto,users,rw,nofail 0 0`
  #####   EXT4	`UUID=FC05-DF26 /mnt/usb0 ext4 defaults,auto,users,rw,nofail 0 0`
#### Test the mounted directory by checking the files on the drive
```
cd /mnt/usb
ls
```

## 4. Install & Setup Samba Server
#### - Install Samba Server
```
sudo apt-get install samba samba-common-bin
```
#### - Edit samba config file to Add Path/drive you want to share
```
sudo nano /etc/samba/smb.conf
```
#### - Add below line at the end of the file then close and save the file.
> `[sharedDrive]` defines the share itself (example: //raspberrypi/sharedDrive.
> `path = /mnt/usb` path to mounted drive. 
> `writeable=Yes` (it will allow the folder to be writable). 
> `create mask=0777` and `directory mask=0777` (allows users to read, write, and execute).
```
[sharedDrive]
path = /mnt/usb
writeable=Yes
create mask=0777
directory mask=0777
public=no
```
#### - Creat a samba user to access the server
> Enter the desired password
```
sudo smbpasswd -a pi
```
#### - Restart the Samba Server to apply the config change
```
sudo systemctl restart smbd
```
> On Windows add a network drive as \\raspberryIPAddress\sharedDrive

## 5. Install Speedtest to test the internet speed
#### - Install some packages for SpeedTest
```
sudo apt install apt-transport-https gnupg1 dirmngr
```
#### - Add the GPG key for Ookla’s Speedtest repository to the keychain
```
curl -L https://packagecloud.io/ookla/speedtest-cli/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/speedtestcli-archive-keyring.gpg >/dev/null
```
#### - Add the Ookla repository to our sources list
```
echo "deb [signed-by=/usr/share/keyrings/speedtestcli-archive-keyring.gpg] https://packagecloud.io/ookla/speedtest-cli/debian/ $(lsb_release -cs) main" | sudo tee  /etc/apt/sources.list.d/speedtest.list
```
#### - Updating the package list
``` 
sudo apt update 
```
#### - Install Speedtest CLI
```
sudo apt install speedtest
```
#### - Run the speed test to check the internet speed
```
speedtest
```
