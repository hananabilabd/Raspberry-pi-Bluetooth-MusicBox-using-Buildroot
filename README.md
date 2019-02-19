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
- Target Packages --> Libraries --> Audio/Sound 

                                                   select
                                                   - bcg729
                                                   - libid3tag
                                                   - libmad
                                                   - architecture-specific optimization
                                                   - libsndfile
                                                   - portaudio --> alsa support
                                                   - C++ binding
                                                   - speex
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
 - Target packages -> Networking applications ->  
 
                                                  - ifupdown scripts
                                                  
                                                  - openssh
                                                  
                                                  - wpa_supplicant -> Enable nl80211 support , Enable Autoscan
                                                  
## Bluetooth
 - Target packages -> Hardware handling --> Firmware -> rpi-bt-firmware
 - Target packages -> Networking applications --> 
 
                                               - bluez-tools
                                               - bluez-utils 5.x (select all items under it)
 - change buildroot/output/images/rpi-firmware/cmdline.txt
  to 
  `root=/dev/mmcblk0p2 rootwait console=serial0,115200 console=tty1 `
- To connect a device to bluetooth
                                                                       
        sudo bluetoothctl
        power on
        agent on
        default-agent
        scan on
        [NEW] Device AA:BB:CC:DD:EE:FF XYZ
        Now I Press the pairing button on the device (disabling bluetooth on any nearby devices)
        connect AA:BB:CC:DD:EE:FF
        trust AA:BB:CC:DD:EE:FF
        exit
- after that to connect this device just 
  `bt-device -c 5C:0E:8B:03:6E:4E`
## Send messages to logges users
 - Target Packages --> system tools --> util-linux -->  wall, write
 - Reference --> [link](https://www.tecmint.com/send-a-message-to-logged-users-in-linux-terminal/)
                                           
## Audio via Bluetooth
- - Target Packages --> Audio and Video applications --> bluez-alsa (and all under it)


### please note that I use these links as a reference

* [Link1](https://www.youtube.com/watch?v=MxKzwvF_eBA)

* [Link2 --> bluetooth-on-raspberry-pi-with-buildroot](https://tewarid.github.io/2014/10/29/bluetooth-on-raspberry-pi-with-buildroot.html)

* [Link3 --> Bluez-alsa](https://github.com/Arkq/bluez-alsa)
