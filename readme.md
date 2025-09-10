## Amp Pro And Amp Pro Mini Hat Quick Start:

Infineon MA12070P 2x80 Peak Output Amp Hat for all series Raspberry Pi boards.

If you are very familiar with Raspberry Pi and audio hat setting. You could follow below quick start steps，otherwise read the user manual firstly.

### 1. Hardware Connection.

Facing the board with the Black DC-Jack and the green 6-pin connector, the hardware interfaces are arranged from left to right as follows. 

If you still cannot understand the wiring order from the above description, please refer to the silkscreen on the bottom of the board.

#### 1 Power Pins

Amp Pro and Amp Pro Mini:

| Pins                   | Description                             |
| ---------------------- | --------------------------------------- |
| DC Jack                | 9-24V, 5.5mm barrell connector          |
| Green Connector  PIN-1 | Alternative Power supply Positive/+     |
| Green Connector  PIN-2 | Alternative Power supply Negative/-/GND |

Note: 

(1) The DC jack and the green connector (PIN1 and PIN2) provide two mutually exclusive power supply options — only one can be used at a time. Supplying power through both simultaneously may damage the board and the Raspberry Pi, due to potential voltage differences between the two sources.

(2) Do not perform hot plugging of the power supply. Connect the wires or DC plug properly before powering on. At the moment of power-up, poor contact at the power connector may cause a large inrush current and sparks, which could potentially damage the AMP HAT and the Raspberry Pi.

(3) Choose a reliable power adapter. Although some adapters are labeled as 24V, their surge voltage at power-on may reach as high as 30V or more. If you cannot find a good 24V adapter, you may consider using a 19V or 20V/6A adapter, which should be sufficient for most scenarios.

(4) Do not supply 5V power to the Raspberry Pi via TYPE-C, as the AMP PRO HAT will provide power to the Raspberry Pi. Additional power supply may damage the Raspberry Pi.



#### 2 Speaker Pins

Amp Pro:

| Pins                   | Description              |
| ---------------------- | ------------------------ |
| Green Connector PIN-3  | Right Speaker Negative/- |
| Green Connector  PIN-4 | Right Speaker Positive/+ |
| Green Connector PIN-5  | Left Speaker Positive/+  |
| Green Connector  PIN-6 | Left Speaker Negative/-  |

Amp Pro Mini:

| Pins                   | Description              |
| ---------------------- | ------------------------ |
| Green Connector PIN-3  | Right Speaker Positive/+ |
| Green Connector  PIN-4 | Right Speaker Negative/- |
| Green Connector PIN-5  | Left Speaker Positive/+  |
| Green Connector  PIN-6 | Left Speaker Negative/-  |



The following are the typical application parameters for speakers from the official Infineon datasheet. Infineon recommends using speakers with a 4–8 Ω load in typical applications. Therefore, you may consider selecting speakers within this range to achieve the best performance.



（1）2x30W continuous output power (RL = 8Ω at 22V, PMP4, 10% THD+N level, without heatsink)

（2）2×80W peak output power (26V PVDD, RL = 4Ω, 10% THD+N level)



Note:

(1) The total peak power of the board is 2x80W, with each single channel supporting up to 80W.

(2) Generally, it is recommended that the power input for the AMP HAT board be 1.1 to 1.3 times greater than the combined power of the two speakers you choose. The power supply's maximum output power can be calculated by multiplying its voltage and current. For example, a 20V/5A power supply can provide a maximum output power of 100W.

(3) Please use a clean and high-quality power supply for the AMP PRO HAT. Based on feedback from early users and other peers using the same chip, this chip is relatively more fragile compared to our previous AMP HAT. Power supplies with large surges or high ripple can easily damage the chip.

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



#### 7.About V2 Version

After the release of AMP PRO V1, we realized that some customers experienced issues, such as hot-plugging the power supply, hot-plugging the boards, or using a power supply with significant voltage spikes, which caused the Infineon MA12070P chip to burn out. During our standard aging and factory tests, we did not fully account for these operations, which led to these problems.

To address these issues, we decided to design the V2 version. In the transition from V1 to V2, we conducted 7 to 8 test versions and went through rigorous aging tests. We tried various approaches, including adding ESD protection, designing soft-start circuits, and replacing the power supply chip. However, the chip’s sensitivity to the power supply exceeded our expectations, and at one point, we even considered abandoning the product. However, we ultimately decided to persist, spending nearly a year before finally releasing the V2 version.

During the R&D testing phase of the V2 version, we conducted tens of thousands of surge power-on tests with various 24V/6A power supplies on the market and performed 3 months of continuous playback aging tests. We also added multiple power cycling and playback tests during factory testing to ensure the stability of the mass product.

I do not want to make excuses for the failure of the V1 version, we indeed made a mistake in this regard. I sincerely apologize to all customers who were affected by the V1 issues. The success and good reputation of our previous audio products led me to overlook potential issues in our product testing. This experience has driven significant improvements in our R&D and testing processes.

Finally, two points about the V2 version:

 (1) The AMP PRO MINI also uses the same power protection design as the AMP PRO V2 and has gone through the same tests;

 (2) Although no issues were found during testing, I still do not recommend customers perform hot-plugging, as there may be certain scenarios we have not encountered or fully considered. If you encounter any problems, please feel free to email us; your feedback is extremely valuable to us.   





​                                                                                                                                                                                            Author : Calvin （calvin@inno-maker.com）

​                                                                                                                                                                                           Support: support@inno-maker.com
