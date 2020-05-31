# RPi-Headless-Setup
Instructions for headless setup of a Raspberry Pi

# Headless Raspberry Pi Setup
## Headless?
While Raspberry Pi’s can be used with a monitor and keyboard, they don’t have to be. You can configure and access a RPi via another computer. This install is called “headless” because there’s no monitor to provide a GUI on the RPi.

This guide will reference MacOS computers and terminology, but the instructions are virtually identical for Windows PCs.

## Requirements
*Hardware:*
* A computer
* RPi with power supply
* microSD card (8GB minimum - more info here: [SD cards - Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/installation/sd-cards.md))
* microSD card reader
* ethernet cable (optional - obtain one if you won’t be using your RPi on your Wifi network)

*Downloads:*
* Etcher - [balenaEtcher - Home](https://www.balena.io/etcher/)
* IP Scanner - [Angry IP Scanner](https://angryip.org/)
* Raspbian Buster with Desktop - [Raspbian Download](https://www.raspberrypi.org/downloads/raspbian/)

*Other*
* Access to your router’s admin section (optional - can use Angry IP Scanner)

## Instructions
### Preparing the OS
1. Download all the download items above. Install Etcher and Angry IP Scanner.
2. Plug in the microSD card into your computer and ensure your computer detects the microSD card.
3. There’s a permissions issue with Etcher on macOS Catalina (10.15). You’ll need to open up your Terminal app and run Etcher as superuser. Cut and paste this command and enter your computer password when prompted: `sudo /Applications/balenaEtcher.app/Contents/MacOS/balenaEtcher`
4. With Etcher open, click on /Select image/ and select the .zip file of the Raspbian image you downloaded.
5. Click on /Select target/ and select the microSD card from the list. *Double check you have selected the correct drive in this step.*
6. Click on /Flash/. It will take several minutes for this process to complete. Meanwhile you can prepare for the following steps.
7. If you want your RPi to access your Wifi network automatically when booting up, you will need to tell it how. /You can skip to step 19 if you will connect your RPi to your network via an ethernet cable./ Download the `wpa_supplicant.conf` file and open it with TextEdit by right-clicking on the file, selecting /Open With/ > /Other…/ and then finding TextEdit in the window that opens.
8. Edit the `country=` entry with your country’s 2 digit code. You can find the codes in this Wikipedia entry: [Country Codes ISO 3166-1 - Wikipedia](https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)
9. Edit the `ssd=` entry and input your Wifi network name inside the quotes.
10. Edit the `psk=` entry and input your Wifi password inside the quotes.
11. Save your update `wpa_supplicant.conf`  file
12. Etcher will need to have completed it’s loading of the Raspbian OS to the microSD card at this point. Once it’s done, eject the miroSD card and then plug it back in. It should show up as a drive named “boot”
13. Place  `wpa_supplicant.conf` in “boot”. Make sure it’s in the root location of your microSD card and not in any folders.
14. Next, open up the Terminal app.
15. In Terminal, type the following and hit enter: `cd /Volumes/boot`
16. To check if you’re in the right directory, type the following and hit enter: `ls -la` If you see several files and amongst them `wpa_supplicant.conf` then you’re in the right location.
17. Type the following and hit enter `touch ssh` This will create an empty file called “ssh” in “boot”. When your RPi starts up this will tell it to start an ssh server. 
18. Us the `ls -la` command to make sure the ssh file is in “boot”
19. Eject the microSD card

### Starting Up Your RPi and Connecting to It
1. In order to connect to the RPi, it’s IP address on your network needs to be known. Open Angry IP Scanner.
2. To the right of the /Start/ button there is a two column icon with multiple lines, click on it. A window called “Fetchers” will open.
3. Click on “MAC Vendor” in the right column and click on the left arrow to add it to the “Selected fetchers column on the left. Do the same for “MAC Address”. 
4. Close the “Fetchers” window.
5. Insert the microSD card into the RPi.
6. If you are *not* using Wifi with your RPi, connect it to your router with an ethernet cable and power it on. If you *are* using Wifi with your RPi, power it on and wait a minute for it to boot up.
7. Press /Play/ in Angry IP Scanner. Once it’s done scanning you’ll see an entry for the RPi along with its IP address. Take note of this IP address. As an example will say it is 192.168.1.X (your IP address will be different, it could be 10.0.0.X).
8. Open Terminal and type the following and hit enter: `ssh pi@192.168.1.X` 
9. Enter the password `raspberry` and hit enter. 
10. You’ll see a message regarding security and RSA keys. Type “yes” and hit enter to proceed. You’ve now accessed your RPi via ssh!
11. It’s important to change the default password. While accessing your RPi via ssh, type the following and hit enter `passwd` It will ask you for a new password. Don’t forget it!

### Configuring a Static IP Address for the RPi
Routers cycle through IP addresses and unless you instruct your router to use the same IP address with a device, the IP address might change at some point. Instructions on how to do this very from router to router. Do a search for "how to set static IP address [your router model goes here]" and you should find instructions on how to set a static IP address for the RPi.

You can now connect to your RPi and use it via command line from another computer without needing a keyboard or monitor connected to your RPi. 

#### Acknowledgements
Inspiration and content for this guide comes form this Youtube video by Brian Lough: [A Truly Headless Setup for your Pi](https://www.youtube.com/watch?v=gOLnIrqmPQc)
