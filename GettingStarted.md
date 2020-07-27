# Required Parts List
* Any portable FM radio with Headphone jack - $15 - [[Amazon Link]](https://www.amazon.com/Personal-Portable-VR-robot-Rechargeable-Earphone/dp/B07YJBTSWY/ref=sr_1_10?dchild=1&keywords=small+fm+radio&qid=1595532866&s=electronics&sr=1-10)
* Aux cable to phone or computer - $6 - [[Amazon Link]](https://www.amazon.com/AmazonBasics-Stereo-Audio-Cable-Meters/dp/B00NO73MUQ/ref=sr_1_5?dchild=1&keywords=aux+cable&qid=1595532898&s=electronics&sr=1-5)
* Raspberry Pi Kit - $80 - [[Amazon Link]](https://www.amazon.com/CanaKit-Raspberry-Starter-Premium-Black/dp/B07BCC8PK7/ref=sr_1_4?dchild=1&keywords=raspberry+pi+3b%2B+kit&qid=1595531028&sr=8-4)
* Jumper cable (only a single cable needed) - $6 - [[Amazon Link]](https://www.amazon.com/EDGELEC-Breadboard-Optional-Assorted-Multicolored/dp/B07GD2BWPY/ref=sr_1_5?dchild=1&keywords=arduino+wire&qid=1595531219&sr=8-5)
*  Software Defined Radio (SDR) - [[External Link]](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/)

### Not required, but nice to have!
* BaoFeng UV-5R Radio (also acts as FM radio)- $35 - [[Amazon Link]](https://www.amazon.com/dp/B08D9HSQ2C/ref=twister_B01GW7YJTC?_encoding=UTF8&psc=1)

---

# Let's get started!
Download Raspberry Pi OS (Raspbian) - Desktop with Recommended Software
* Note: Do not use NOOBS for this project

Use this links instead:
* https://downloads.raspberrypi.org/raspios_full_armhf_latest

## Write the OS to the Micro SD Card
Write image to SD card using one of the following:
* My personal choice - [Rufus](https://rufus.ie/)
* Raspberry Pi recommended - [rpi-imager](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

## Enable SSH
After the image is fully done writing to the MicroSD card we will enable SSH in headless mode. This means that we will not require a mouse and keyboard to use the Raspberry Pi. To do this we will need to perform the following steps:
* Eject the MicroSD card from your computer
* Insert the MicroSD back into the computer
* Navigate to the now mounted "Boot" partition
* Create an empty text file in that directory "ssh.txt"
* Next rename the file and remove the .txt so the file name is simply "ssh" without any file extension.
* You may need to turn file extensions on if you're performing this action in windows.
* Googling "display file extensions [your windows edition]" should take you to where you need to go.
* Additionally you can also hook up a mouse, keyboard, and monitor to your Raspberry Pi and enable SSH (this would be an alternative method to enabling SSH)

SSH is simply a secure method to access your Raspberry Pi over a network connection which does not require a mouse, keyboard, or monitor.

## Booting the Raspberry Pi
Once you've successfully written your image and created the SSH file, you should now be ready to boot up the Raspberry Pi
* Eject the MicroSD card from your computer
* Ensure the Pi is unplugged from power
* Insert the MicroSD card into the Raspberry Pi 
* Plug in a network cable
* Connect your Raspberry Pi to power

## Finding your Pi on your local network
Your Raspberry Pi should now boot up and automatically obtain an IP address on your local network. You will need to find this IP address in order to connect to your Raspberry Pi.
To find your Pi's IP address, wait a few minutes for your Pi to be fully powered up and connected to your network.

Open a command prompt in windows and type the following command:

``` ipconfig```

Look through the displayed text and locate "DNS Suffix", for my network the "DNS Suffix" is "localdomain". Yours will most likely be different.

Next execute the following command in the same command prompt:

```ping raspberrypi.[your DNS Suffix] ```

Example:
```
C:\Users\Eric>ping raspberrypi.localdomain

Pinging raspberrypi.localdomain [192.168.11.49] with 32 bytes of data:
Reply from 192.168.11.49: bytes=32 time=82ms TTL=64
Reply from 192.168.11.49: bytes=32 time=2ms TTL=64
```
You should hopefully see a ping response with an IP address. Write down this IP address as we will use it to connect to our Raspberry Pi.

## Install an SSH Client
Next install the tool [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
Putty is an SSH Client for Windows which will allow you to connect to your Raspberry Pi.

Once installed, open Putty, and under ```"Host Name (or IP address)"``` enter the IP address we pinged previously. You will now be prompted to login. 
Use the following default credentials:
* Default user - ```pi```
* Default pass - ```raspberry```

With a successful login you'll now have full control of your Raspberry Pi from your normal computer. Congratulations this is the hardest part for most!

## Secure & Personalize

Run the command: ```sudo raspi-config```

* Change your password (1 Change User Password)
* *Optional* - Connect to WiFi (2 Network Options) -> N2 Wireless LAN 
* *Optional* - Configure Time Zone (4 Localisation Options) -> I2 Change Time Zone
* *Optional* - Use full space of Micro SD card (7 Advanced Options) -> A1 Expand Filesystem

Now ```<Finish>``` and select ```<Yes>``` to reboot. 

Putty will lose the connection and you will have to reconnect once the Pi has rebooted. 

# Installing Software

First we are going to make sure everything is nice and updated.

Run the following command once logged into the Raspberry Pi: ```sudo apt-get update && sudo apt-get upgrade -y``` *This may take a few minutes*

We are next going to be following the following modified installation instructions found here: [[Credit alanbjohnston]](https://github.com/alanbjohnston/CubeSatSim/wiki/CubeSat-Simulator-Lite)
* *Optional* - Clean up your home directory: ```rm -rf Bookshelf/ Documents/ Music/ Public/ Videos/ Downloads/ Pictures/ Templates/``` - **[WARNING: This will delete all of the mentioned folders!]**
* Install the following packages: ```sudo apt-get install -y git screen libsndfile1-dev vim nmap minimodem cmake rtl-sdr```
* Clone all the software to your Pi (copy & paste the entire command as one line):
```cd && git clone https://github.com/alanbjohnston/CubeSatSim.git && cd CubeSatSim && git clone https://github.com/ChristopheJacquet/PiFmRds.git && cd PiFmRds/src && make && cd ../../```
Now run this:
`cd && git clone https://github.com/alanbjohnston/CubeSatSim.git && cd CubeSatSim`
You. Are. Done.

# Adding an Antenna
You can next add an antenna to your Raspberry Pi to increase the transmit distance. Without the antenna, it is still possible to recieve the radio signal a few feet away. I would recommend not attaching an antenna to start with. 

Attaching a jumper wire to GPIO 4 (if using the following [reference](https://www.raspberrypi.org/documentation/usage/gpio/)) should increase your transmit range. Please abide by local laws and check before transmitting to make sure your frequency is clear.

# Moving Around
If you're new to linux there are a few things you'll need to move around to different directories.

* `cd` stands for "change directory", typing the command `cd` will take you to your home directory on the Pi
* `cd CubeSatSim` will take you into the CubeSatSim directory
* `ls -lah` will provide a list of files and folders within the directory you are in
* You can also use the `TAB` key to autocomplete!

# Transmitting
Now try a sample transmit from the CubeSatSim directory:
* Make sure you're in the correct directory
  * `cd ~/CubeSatSim/`
* Now try and transmit!
  * `sudo ./PiFmRds/src/pi_fm_rds -audio wav/cw.wav -freq 107.5`
  * The `-audio wav/cw.wav` portion of the command below will play the `cw.wav` audio file within the `wav/` directory.
  * The `-freq 107.5` portion of the command will play the `cw.wav` audio file on 107.5 FM (make sure you pick an empty FM radio slice)

You can listen to this audio on a standard FM radio (Baofeng radios work nicely as well as your car radio)

This is a sample cw file, you can also try playing the other examples provided by the AMSAT team:
* `-audio wav/afsk1.wav`
* `-audio wav/afsk2.wav`
* `-audio wav/afsk3.wav`
* `-audio wav/afsk4.wav`
* `-audio wav/afsk5.wav`
* `-audio wav/afsk6.wav`

# Further experimentation
Once you can hear the sounds produced by your Raspberry Pi on a standard FM radio, the next step is decoding the actual messages. 

## Decoding CW telemetry sample
* Shutdown the raspberry pi with the following command:
  * `sudo shutdown -h now`
* Plug in the software defined radio (SDR)
* Power the Pi back on and reconnect to it via SSH

### Listening
* Navigate to the mutimon-ng directory (more info on Multimon-ng can be found [[here]](https://github.com/EliasOenal/multimon-ng):
  * `cd ~/miltimon-ng/build/`
* Issue the following command:
  * `rtl_fm -f 107.5M -s 48000 | multimon-ng -t raw -a MORSE_CW /dev/stdin`
  * This command will instruct the Raspberry Pi to use your software defined radio to listen on `107.5`FM with a `-s` sample rate of 4800.
  * The pipe `|` will direct the output of the SDR to `multimon-ng`
  * Multimon-ng is instructed to accept raw input `-t raw` and to decode morse cw `-a MORSE_CW` (other decoding options are available)
  * Lastly `/dev/stdin` instructs multimon-ng to accept input from the pipe `|`
  * Multimon-ng can also accept and decode a WAV file that you may have recorded.
### Transmitting
* Open another instance of Putty, and log in to the Raspberry Pi again (don't kill your first "listening" connection)
  * [Linux users] If you've used linux before, feel free to run `screen` or `tmux`
* Navigate to your CubSatSim directory
  * `cd ~/CubeSatSim/`
* Run the sample transmission just as before
  * `sudo ./PiFmRds/src/pi_fm_rds -audio wav/cw.wav -freq 107.5`

You should now see data begin to populate on your listening screen.
Here is a clip of the decoded telemetry sample: ` E A 000 000 000 000 E A 000 000 000 000 E U 000 000 000 000 E`

## Encoding
https://github.com/sunny256/cwwav
sudo make install

# Extra Credit

If you have time, you can experiment with AFSK, start by decoding the samples provided by the AMSAT team!
(Note: multi
Next try encoding your own!


