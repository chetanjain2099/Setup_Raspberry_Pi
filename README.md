## Setup Raspberry Pi as media server with torrent download and smb server. 
I have created a simple step by step process used to install apps and packages on Raspberry Pi.
Setup Process used below is:
  1. Update all the packages to the latest version
  2. Setup time zone
  3. Automatically mount USB Drive
  4. Install Samba Server to create smb server
  5. Install Speedtest to test the speed
  6. Over Clocking the CPU to boost the performance
  7. StressTest
  8. QBittorrent
  9. Jellyfin

### 1. First Update all the packages
```
sudo apt update && sudo apt upgrade
```

### 2. Setup date and time correctly (Set time zone).
> Run the command -> Select Localization option from the menu -> Select Timezone -> Select Area -> Select City/Region
```
sudo raspi-config
```

### 3. Auto mount usb drives
#### 1. Create a diretry to mount the USB Drive.
```
mkdir /mnt/usb
```

#### 2. First find the UUID of the USB drive to mount (Notedown UUID and Type)
```
sudo blkid
```

#### 3. Create a backup of the file and edit it.
```
sudo cp /etc/fstab /etc/fstab.back
sudo nano /etc/fstab
```
#### 4. Use one of the below code to mount the drive. Don't forget to change UUID and the mount point.
> FAT `UUID=FC05-DF26 /mnt/usb0 vfat defaults,auto,users,rw,nofail,umask=000 0 0`
> NTFS `UUID=FC05-DF26 /mnt/usb0 ntfs defaults,auto,users,rw,nofail,umask=000 0 0`
> exFAT `UUID=FC05-DF26 /mnt/usb0 exfat defaults,auto,users,rw,nofail 0 0`
> EXT4	`UUID=FC05-DF26 /mnt/usb0 ext4 defaults,auto,users,rw,nofail 0 0`
