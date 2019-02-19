# Raspberry-pi-Bluetooth-MusicBox-using-Buildroot
Raspberry-pi Bluetooth, WiFi MusicBox using Buildroot

## Music Installation
- `make menuconfig`  
- Toolchain --> C library --> glibc
- Target Packages --> Audio and Video applications --> alsa-utils 
 ![](https://github.com/hananabilabd/Raspberry-pi-Bluetooth-MusicBox-using-Buildroot/blob/master/Images/alsa-utils.png)
 
- Target Packages --> Audio and Video applications --> aumix, mpg123, pulseaudio.
- Target Packages --> Libraries --> Audio/Sound 
  select alsa-lib then select all included in it
  ![](https://github.com/hananabilabd/Raspberry-pi-Bluetooth-MusicBox-using-Buildroot/blob/master/Images/alsa-lib.png)
  
  select bcg729
  libid3tag
  libmad
  architecture-specific optimization
  libsndfile
  portaudio --> alsa support
  C++ binding
  speex
  - Append in package/rpi-firmware/config.txt
    dtparam=audio=on
  - To use Shuffle in our MusicBox edit package/busibox/busibox.config
    CONFIG_SHUF=y
    
## Text to Speech
 - Target Packages --> Audio and Video applications --> espeak --> alsa via portaudio (as a backend)
  
## Filesystem Images
 - This to increase our image so that it can handle the extra package we chose
 - Filesystem images --> exact size --> 300M 
  
## Wifi
 - Target packages -> Hardware handling -> Firmware -> rpi-wifi-firmware
 - Target packages -> Networking applications -> ifupdown scripts
                                                  - openssh
                                                  - wpa_supplicant -> Enable nl80211 support , Enable Autoscan
                                                  
## Bluetooth
 - Target packages -> Hardware handling -> Firmware -> rpi-bt-firmware
 - Target packages -> Networking applications -> 
                             - bluez-tools
                             - bluez-utils 5.x (select all items under it)
                             
## Send messages to logges users
 - Target Packages --> system tools --> util-linux --> - wall 
                                                       - write
