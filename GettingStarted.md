# Download Raspberry Pi OS
Download Raspberry Pi OS (Raspbian) - Desktop with Recommended Software
* Note: Do not use NOOBS for this project

Use these links instead:
* https://www.raspberrypi.org/downloads/raspberry-pi-os/
* https://downloads.raspberrypi.org/raspios_full_armhf_latest

# Write the OS to the Micro SD Card
Write image to SD card using one of the following:
* My personal choice - https://rufus.ie/
* Raspberry Pi recommended - https://www.raspberrypi.org/documentation/installation/installing-images/README.md

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

# Booting the Raspberry Pi
Once you've successfully written your image and created the SSH file, you should now be ready to boot up the Raspberry Pi
-Eject the MicroSD card from your computer
-Ensure the Pi is unplugged from power
-Insert the MicroSD card into the Raspberry Pi 
-Plug in a network cable
-Connect your Raspberry Pi to power

Your Raspberry Pi should now boot up and automatically obtain an IP address on your local network. You will need to find this IP address in order to connect to your Raspberry Pi.
To find your Pi's IP address, wait a few minutes for your Pi to be fully powered up and connected to your network.
Open a command prompt in windows and type the following command:
ipconfig

Look through the displayed text and locate "DNS Suffix", for my network the "DNS Suffix" is "localdomain". Yours will most likely be different.

Next execute the following command in the same command prompt:
ping raspberrypi.[your DNS Suffix] 

You should hopefully see a ping response with an IP address. Write down this IP address as we will use it to connect to our Raspberry Pi.

Next install the tool Putty: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
Putty is an SSH Client for Windows which will allow you to connect to your Raspberry Pi.

Once installed, open Putty, and under "Host Name (or IP address)" enter the IP address we pinged previously. You will now be prompted to login. Enter the username pi, and the password raspberry

With a successful login you'll now have full control of your Raspberry Pi from your normal computer. Congratulations this is the hardest part for most!

First things first let's change your password:
Enter the following command: passwd
Now create a unique passphrase so that no one else can access your Raspberry Pi.

From there you can run the command "sudo raspi-config" and expand your operating system to use the full Micro SD card, to connect your Pi to WiFi and to set the correct timezone.


We are going to be following the following installation instructions found here:
https://github.com/alanbjohnston/CubeSatSim/wiki/CubeSat-Simulator-Lite

