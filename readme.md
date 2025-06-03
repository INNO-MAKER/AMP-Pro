## Amp Pro Hat Quick Start:

If you are very familiar with Raspberry Pi and audio hat setting. You could follow below quick start steps，otherwise read the user manual firstly.

### 1. Hardware Connection.

Facing the board with the Black DC-Jack and the green 6-pin connector, the hardware interfaces are arranged from left to right as follows. 

If you still cannot understand the wiring order from the above description, please refer to the silkscreen on the bottom of the board.

#### 1 Power Pins

| Pins                   | Description                         |
| ---------------------- | ----------------------------------- |
| DC Jack                | 9-24V, 5.5mm barrell connector      |
| Green Connector  PIN-1 | Alternative Power supply Positive/+ |
| Green Connector  PIN-2 | Alternative Power supply Negative/- |

Note: 

(1) The DC jack and the green connector (PIN1 and PIN2) provide two mutually exclusive power supply options — only one can be used at a time. Supplying power through both simultaneously may damage the board and the Raspberry Pi, due to potential voltage differences between the two sources.

(2) Do not perform hot plugging of the power supply. Connect the wires or DC plug properly before powering on. At the moment of power-up, poor contact at the power connector may cause a large inrush current and sparks, which could potentially damage the AMP HAT and the Raspberry Pi.

#### 2 Speaker Pins

| Pins                   | Description              |
| ---------------------- | ------------------------ |
| Green Connector PIN-3  | Right Speaker Negative/- |
| Green Connector  PIN-4 | Right Speaker Positive/+ |
| Green Connector PIN-5  | Left Speaker Positive/+  |
| Green Connector  PIN-6 | Left Speaker Negative/-  |

Note:

(1) The total power of the board is 80W, with each single channel supporting up to 40W.

(2) Generally, it is recommended that the power input for the AMP HAT board be 1.1 to 1.3 times greater than the combined power of the two speakers you choose. The power supply's maximum output power can be calculated by multiplying its voltage and current. For example, a 20V/5A power supply can provide a maximum output power of 100W.

(3) Please use a clean and high-quality power supply for the AMP PRO HAT. Based on feedback from early users and other peers using the same chip, this chip is relatively more fragile compared to our previous AMP HAT. Power supplies with large surges or high ripple can easily damage the chip. We are currently working on some improvements and aging tests, and we hope to fully resolve this issue in the near future. If this is a concern for you, please return the product and kindly wait for our upcoming version.

(4) Please do not reverse the +/- terminals of the left and right speakers, otherwise you may hear strange sounds due to phase differences.

### 2.Software

#### 1.System Image Download link

[rAudio Image  download](https://github.com/rern/rAudio/releases)

[Volumio image download](http://volumio.org/get-started/)

[MoOde image download](http://www.moodeaudio.org/)

[Raspbian and Raspbian image download](https://www.raspberrypi.com/software/operating-systems/)

[LibreELEC image download](https://libreelec.tv/downloads/raspberry/)

[Max2Play image download](https://www.max2play.com/en/max2play-image/)

[OSMC image download](https://osmc.tv/download/)

#### 2.rAudio Setup

Setting→System→GPIO Devices→Audio-I2S→**MERUS AMP piHat ZW**→reboot.

#### 3.Volumio Setup

Playback Options→Audio Output→I2S DAC→ **On**→DAC Model→**MERUS AMP piHat ZW**→reboot

Or Playback Options→Audio Output→I2S DAC→ **On**→DAC Model→**Innomaker Amp Pro**→reboot

Playback Options→Volume Options→Mixer Type→**hardware**

#### 4.MoOde Setup

Configure→Audio→Output device → **I2S audio device**

Configure→Audio→Named I2S device→**MERUS AMP piHat ZW**

#### 5.Raspberry Pi OS and Raspberry Pi OS Lite Setup

1.Open  the config.txt on terminal via build-in nano tools.

```
sudo nano /boot/firmware/config.txt
```

Legacy version system：

```
sudo nano /boot/config.txt
```

2.Append the following lines to the end of the files.

```
dtoverlay=merus-amp
```

3.Pressing CTRL+ x key to exit and pressing  y’ key to save your changes. Finally, reboot.

```
sudo reboot
```

4.Check the DAC module named 'Merus-Amp' with serial number 3 is available. 

```
aplay -l 
```

or

```
cat /proc/asound/cards
```

5.On Raspbian desktop, Right click on desktop volume icon  set the raspberry pi audio output as ‘Merus-Amp'

6.On Raspbian lite, you could follow as step to change the default audio output.

Create /etc/asound.conf 

```
sudo nano /etc/asound.conf
```

Type in the following content , pressing "CTRL+x" and then pressing "y" to save  your changes. Finally, reboot again.Note that the numebr **3** is the DAC module device number on the system.

```
pcml.!default {
  type hw card 3
}

clt.!default{
 type hw card 3
}
```

7.Type in the commands that are shown below, you can see the alsamixer tool.

```
alsamixer
```

#### 6.LibreELEC Setup                            

1.Modify the config.txt on LibreELEC image card.Append the following lines to the end of the file.

```
dtoverlay=merus-amp
```

2.Settings→System→Audio→Audio output device→**MERUS AMP piHat ZW**→reboot.



​                                                                                                                                                                                            Author : Calvin （calvin@inno-maker.com）

​                                                                                                                                                                                           Support: support@inno-maker.com
