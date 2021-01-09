# mycroft_pi
## Brief

The [Mycroft](https://mycroft.ai/) is an opened source Google Assistant counterpart. PiCroft is available if build from scratch is not preferred.

This collaborated the steps of

1. setup ReSpeaker 4-Mic Array
1. build the mycroft

## Hardware and Software
1. Raspi 3B+ (raspbian 10 buster)
1. Respeaker 4 mic HAT

## 1. Setup ReSpeaker 4-Mic Array
Reference:
1. https://wiki.seeedstudio.com/ReSpeaker_4_Mic_Array_for_Raspberry_Pi/

### Installation
(After ssh login)
- *git clone https://github.com/respeaker/seeed-voicecard.git (commit 8cce4e8ffa77e1e2b89812e5e2ccf6cfbc1086cf, 20200920 is used for this document)
- *cd seeed-voicecard
- *sudo ./install.sh 
- *sudo reboot now
(After ssh login)
- *sudo raspi-config
- select prefered audio out "System Options"->"Audio"
- enable SPI if LEDs is required, "Interface Options"->"SPI"->"YES"
- exit tool
- *arecord -l*, the card and device number of mic can be found
- *aplay -l*, the card and device number of speaker can be found
- create file ~/.asoundrc as following lines of code,
  ```
  pcm.!default {
      type asym
      playback.pcm "hw:<card_id>,<device_id>"
      capture.pcm "hw:<card_id>,<device_id>"
  }
  ```

### Testing
- *arecord -Dac108 -f S32_LE -r 16000 -d 5 -c 4 a.wav
- *aplay a.wav*, playback the just recorded sound

## 2. Install MyCroft from source code
Reference:
1. https://github.com/MycroftAI/enclosure-picroft/blob/stretch/image_recipe.md
1. https://github.com/MycroftAI/enclosure-picroft/blob/buster/home/pi/update.sh

(After ssh login)
- add a ramdisk by adding the line to /etc/fstab.
  ```
  tmpfs /ramdisk tmpfs rw,nodev,nosuid,size=20M 0 0
  ```
- *git clone https://github.com/MycroftAI/mycroft-core.git
- *cd mycroft-core
- *dev_setup.sh -y*, waited over 1 hours on my PI3B+
- reboot

Additional:
## 3. Install Skills for the Respeaker LED
Reference:
1. https://github.com/j1nx/respeaker-4mic-hat-skill
(After ssh login)
- mycroft-msm install https://github.com/j1nx/respeaker-4mic-hat-skill.git 

### Testing
(After ssh login)
- *source env/bin/active
- *cd mycore-core
- *./start-mycroft all*, all services will be started
- *./start-mycroft cli*, to show the console for debugging





