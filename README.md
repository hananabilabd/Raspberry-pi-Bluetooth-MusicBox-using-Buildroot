# Raspberry-pi-Bluetooth-MusicBox-using-Buildroot
Raspberry-pi Bluetooth, WiFi, MusicBox using Buildroot
## Video
 [Link](https://youtu.be/OiDO2pVhzgk)
## Project Requirements
![](https://github.com/hananabilabd/Raspberry-pi-Bluetooth-MusicBox-using-Buildroot/blob/master/Images/Advanced_MP3_Player_0.png)
![](https://github.com/hananabilabd/Raspberry-pi-Bluetooth-MusicBox-using-Buildroot/blob/master/Images/Advanced_MP3_Player_1.png)
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
- `aplay -D bluealsa:DEV=74:E5:43:F4:C5:F4,PROFILE=a2dp,HCI=hci0 /root/superMusic/wallada.wav `
## Send messages to logges users
 - Target Packages --> system tools --> util-linux -->  wall, write
 - Reference --> [link](https://www.tecmint.com/send-a-message-to-logged-users-in-linux-terminal/)
                                           
## Audio via Bluetooth
- - Target Packages --> Audio and Video applications --> bluez-alsa (and all under it)

# Usage 
- to regenerate the project, First download builroot and 
- copy playmusic folderti `buildroot/board/raspberrypi3-64/`
- replace post-build.sh in `buildroot/board/raspberrypi3-64/`with post-build.sh from my repository
- replace raspberrypi3_64_defconfig in `buildroot/configs/`  with the one from my repository
- `$ make raspberrypi3_64_defconfig`
- Enjoy
# Description 


1. On power-up rpi3, it scans all connected Mass storage devices (sd card, usb, hdd) for MP3 files and output on the speaker the number of mp3 files found 
   * On Power-up it executes script called  S50-playmusic-daemon, this script execute 1 script and 2 daemons .
   * First script "/root/superMusic/playmusic" to play a welcome music.
   * First daemon run script /root/superMusic/detectUSB  this daemon enter an infinite loop to scan if any USB devices has been plugged and mount this USB . 
   * Second daemon "/root/superMusic/playmusic-daemon"  enter an infinite loop to detect if any button is pressed to play or stop music playlist 
2. There is a flag that is  shared between the 2 daemons so that when detectUSB daemon found a USB and mount it , it raises a flag to playmusic-daemon says that something changed in the system so that playmusic-daemon should search again for all mp3 files and create a new bash array with these new found files  
   * same thing happens you unplug usb storage device , detectUSB daemon detects Something changed and raise this flag to         playmusic-daemon , so that playmusic daemon can detect that it should refresh the playlist with the new files 
3. If the user plug a USB device , rpi 3 detects it and mounts it on media/sd*/  and rpi output on the speaker number of mp3 files found on this USB device, it's type and it's size 
4. Music playlist is always all the mp3 files found on the Rpi ( SD card + USB storage)  , this playlist is contollable through 3 buttons previous, pause, play . When user reach the end of the playlist the next button stop working  and the same to the previous button when the first song is being played 
5. User can control the RPi through wifi or cable   .. I set static ip 192.168.1.201 to the wifi  ,, but it's noticed that rpi can't be set to static ip through wifi and ethernet at the same time 
6. On connecting to Rpi via SSH , Rpi prints the name, status of the mp3 playlist , so it prints either nothing is played when rpi has booted and none pushed play button , song1 is being played , song2 is paused , it prints these on all connected terminals with "wall" command
7. Rpi is able to play music via bluetooth ( may be it was the toughest task) . I was able to play .wav through bluetooth , but I wasn't able to make the bluetooth as the default rpi speaker (( i tried to create asound.conf to globaly set bluetooth as rpi default speaker but it didn't work, although i think it can be done)) 
8. The main problem in the bluetooth is that bluez-utils5 has conflicts with alsa in fact they say in bluez-utils5 they remove support to alsa so if you choose bluez-utils5 you have to choose bluez-alsa in addition to run audio bluetooth but if you choose bluez-utils 3 audio support is included in it  ... I worked with bluez-utils5 anyway.

#### please note that I use these links as a reference

* [Link1](https://www.youtube.com/watch?v=MxKzwvF_eBA)

* [Link2 --> bluetooth-on-raspberry-pi-with-buildroot](https://tewarid.github.io/2014/10/29/bluetooth-on-raspberry-pi-with-buildroot.html)

* [Link3 --> Bluez-alsa](https://github.com/Arkq/bluez-alsa)
* [Link4 --> Raspberry stackExchange](https://raspberrypi.stackexchange.com/questions/90267/how-to-stream-sound-to-a-bluetooth-device-from-a-raspberry-pi-zero)
