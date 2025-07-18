---
layout: default
---

<!-- Images were scaled to 1920 x 1080 pixels at 150 pixels per inch. Exported as a JPEG with quality set to 40 (out of 100). -->

# Get the essentials
To get started you'll need some hardware:

- a [Raspberry Pi](https://www.raspberrypi.com/)
- speakers or headphones
- an Ethernet cable, unless you have WiFi
- a Secure Digital (SD) card
- a SD card reader
- a power supply

![A photo of the essential hardware listed in the text](/franpi/assets/images/hardware/essential-hardware-including-raspberry-pi-and-speakers.jpeg)

# Check your Rasperry Pi model
Check which model of Raspberry Pi you have. The model number is printed on the circuit board.

![A photo of a Raspberry Pi showing the model printed on the circuit board](/franpi/assets/images/hardware/model-number-printed-on-the-raspberry-pi.jpeg)

Different models of the Raspberry Pi support different audio features. For example, the photo shows a Raspberry Pi 3 Model B+. This model supports WiFi and Bluetooth. It also features an Ethernet port, a 3.5 mm audio jack socket and several Universal Serial Bus (USB) ports. The WiFi or the Ethernet port can be used to receive internet radio stations. Bluetooth can be used to connect to a Bluetooth speaker or Bluetooth headphones. The 3.5 mm audio jack can be connected to speakers. One of the USB ports can be connected to USB speakers or a USB digital-to-analogue converter (DAC).

Older Raspberry Pi models do not have WiFi or Bluetooth support, so you will need an Ethernet cable to use these. Very new models, such as the Raspberry Pi 5, do not have a 3.5 mm audio jack socket so will not connect to some speakers.

# Prepare the SD card
In this step you'll install the Raspberry Pi Operating System (OS) on the SD card.

First, ensure that there is no data on your card that you want to keep. All data on the card will be erased during this step!

Use a computer, any computer, to download the [Raspberry Pi Imager](https://www.raspberrypi.com/software/). Install the Raspberry Pi Imager and then launch it.

The Raspberry Pi Imager will ask you to choose a device, an operating system and storage. Ensure you have an internet connection as it will download the operating system from Raspberry Pi.

![A screenshot of Raspberry Pi imager, step one](/franpi/assets/images/screenshots/raspberry-pi-imager-step-1.png)

The Raspberry Pi Imager needs to know which model of Raspberry Pi you have. Click `CHOOSE DEVICE` and select your Raspberry Pi model from the list.

Now choose an operating system for your Raspberry Pi. To use a Raspberry Pi as a radio the *Lite* version of Raspberry Pi OS is sufficient, though other versions of Raspberry Pi OS will also work. Click `CHOOSE OS`, then `Raspberry Pi OS (other)`, then `Raspberry Pi OS Lite (64-bit)`.

> [!TIP]
> Eject any external USB devices from your computer before proceeding. This reduces the risk that you accidently write to the wrong device.

Tell the Raspberry Pi Imager which drive to write to. Click `CHOOSE STORAGE` and list of devices will appear. Now put your SD card into the SD card reader and plug it into your computer. A new device will appear in the list, select it. Make certain you have selected the right device as all data on it will be erased! If in doubt, eject any other USB devices that appear in the list to ensure you don't accidently overwrite them. Once you're certain, click `NEXT`.

The Raspberry Pi Imager will ask `Would you like to apply OS customisation settings?`. This is an opportunity to do some basic setup of the Raspberry Pi before switching it on for the first time, so click `EDIT SETTINGS`. The `OS Customisation` window appears.

Choose a hostname for your Raspberry Pi. This will be the name you use to connect to your radio. Check `Set hostname` and enter your chosen hostname.

Set up a username and password. Click `Set username and password`, then enter a username and password.

If you want to connect your Raspberry Pi to your WiFi, chcek `Configure wireless LAN` and enter your network's service set identifier (SSID). This the name of your network, exactly as it would appear on a phone or a computer. Enter your WiFi password. If you are connecting with an Ethernet cable you do not need to configure the wireless.

Click `Set locale settings` and choose your time zone. Leave the `Keyboard layout` blank.

![A screenshot of Raspberry Pi imager, step two](/franpi/assets/images/screenshots/raspberry-pi-imager-step-2.png)

Click on the `Services` tab. This is where you'll set up Secure Shell (SSH) to allow you to set up your Raspberry Pi from your computer. If all goes to plan, you won't ever need to connect a screen or keyboard to your Raspberry Pi.

Click `Enable SSH`. You now have two choices for how you will log in to your Raspberry Pi over the network. You can authenticate with the password you entered on the `General` tab. Alternatively you can use a public-key. You can chooose either option. Using a public-key is more complex to set up but means you will not need to type your password every time you log in to your Raspberry Pi.

To set up public-key authenticaion, click `Allow public-key authentication only`. If you don't already have a public-key you can generate one on your computer.

Open PowerShell if you are using Microsoft Windows, or Terminal if you are running Linux. Type:

	ssh-keygen -t ED25519 -f "id_franpi" -C "Fran"
	
replacing `id_franpi` with the filename you would like to use for your key. It's a good idea to use a filename that starts with `id_`. Replace the key's comment, `Fran`, with a name that identifies you. `ED25519` is the type of the key. `ssh-keygen` will ask you to enter a passphrase

	Generating public/private ED25519 key pair.
	Enter passphrase (empty for no passphrase):
	
If you enter a passphrase here it will be used to encrypt your key to keep it secure. Alternatively you can leave the passphrase empty for less security but more convenience. `ssh-keygen` will ask you to repeat your passphrase

```
Enter same passphrase again:
Your identification has been saved in id_franpi
Your public key has been saved in id_franpi.pub
The key fingerprint is:
SHA256:bm7xHRuT1wQWqc53BxmnUNdqE6xdIRaR8+WRnivg7+c Fran
The key's randomart image is:
+--[ED25519 256]--+
|             B*+=|
|            ooO+=|
|             *+@+|
|           .o O+o|
|        S .o.o =.|
|       ..  .B.o.+|
|        oo ..B...|
|       o. . o. . |
|       ..   ..oE |
+----[SHA256]-----+
```

Two new files will have been created. Have a look at them by typing

```powershell
ls id_*
```

to show the directory listing

```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        2025-07-05     19:31            387 id_franpi
-a----        2025-07-05     19:31             87 id_franpi.pub
```

`id_franpi` is a private key file. This key stays on your computer. Keep it private!

`id_franpi.pub` is a public key file. Have a look at its contents by typing

```powershell
cat id_franpi.pub
```

The contents of the file are your public key

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILoSkCkHyOVJgwtqqllukNBo88kdtcBTc69v0fk5G8Q3 Fran
```

You can also find out the public key directly from the private key (`id_franpi`) by typing

```powershell
ssh-keygen -yf id_franpi
```

which will show

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILoSkCkHyOVJgwtqqllukNBo88kdtcBTc69v0fk5G8Q3 Fran
```

Copy and paste your public key into the text box (`Set authorized_keys...`) in the Raspberry Pi Imager.

![A screenshot of Raspberry Pi imager, step three](/franpi/assets/images/screenshots/raspberry-pi-imager-step-3.png)

Click the `Options` tab if you'd like to change what happens once the write is complete.

Click `SAVE`, then click `YES` to apply your OS customization settings.

A warning will appear to tell you that all existing data on your device will be erased. If you are certain you chose the correct device, your SD card, click `YES`.

![A screenshot of Raspberry Pi imager, step four](/franpi/assets/images/screenshots/raspberry-pi-imager-step-4.png)

The Raspberry Pi imager will write the operating system and your customization to the SD card. It will then verify that all the data was written correctly. If you chose Raspberry Pi OS *Lite* this process will take a few minutes. Once complete, you will see the message `You can now remove the SD card from the reader`. Click `CONTINUE`, close the Raspberry Pi Imager and remove your SD card.

You are now ready to boot your Raspberry Pi!

# Boot
Plug in your Raspberry Pi and let it boot. A yellow light on the Raspberry Pi will flash during the boot. The first time that you boot a Raspberry Pi it always takes longer than you expected. Once the yellow light has stopped flashing the Raspberry Pi has finished booting.

# Connect
## If works!
On your computer, open PowerShell or Terminal and type

	ssh -i id_franpi fran@franpi
	
where `id_franpi` is the private key file you created earlier, `fran` is the username you set up and `franpi` is the hostname you set.

You'll get a warning about the host key fingerprint. Type `yes`.

## It doesn't work!
Sometimes it doesn't work right away. So if something goes wrong, here's some things you can try.

First, try pinging it

	ping franpi

Have a look for the Raspberry Pi in your router.

Try mapping your network with `nmap`, the network mapper.

	nmap -sn 192.168.0.0/24
	
replacing `192.168.0.0` with the address of your network. The last digit should be a zero. So if your computer's address is `192.168.0.65` then your network address is `192.168.0.0`.

If you have a mobile phone, an app like *Fing* can scan your network.

# Check for power supply problems
Connect to your Raspberry Pi by typing

	ssh -i id_franpi fran@franpi

Then type

	dmesg
	
and have a look for anything that suggests a problem with the power supply. Sometimes messages will apear that look like this

	[   14.301684] hwmon hwmon1: Undervoltage detected!
	[   18.333563] hwmon hwmon1: Voltage normalised

This indicates that, about 14 s after booting, the power supply was not providing sufficient voltage.

Power supply problems are generally not serious but can cause unreliable Blueooth or WiFi.

# Complete Raspberry Pi setup
First update the package lists. This gets it ready to install software in the next step. Type

	sudo apt update

# Enable auto login
The Raspberry Pi needs to have auto login enabled. This is mainly for Bluetooth devices which will always be disconnected when there is no user logged in.

Be sure not to use the Raspberry Pi for anything security-critical. As it will be logged in all the time anyone can connect a screen and keyboard and start using it.

Start the Raspberry Pi Software Configuration Tool by typing

	sudo raspi-config

![A screenshot of the Raspberry Pi Software Configuration Tool](/franpi/assets/images/screenshots/raspberry-pi-software-configuration-tool.png)

Open `1 System Options`, then `S6 Auto Login`. When asked `Would you like to automatically log in to the console?`, answer `Yes`. The message `Console autologin is enabled` will appear. Select `OK`, then `Finish` to close the tool.

# Install audio software
Now install some audio software.

PipeWire is a framework for handling audio streams on the Raspberry Pi. It has a utility called WirePlumber that makes it easier to work with PipeWire streams.

The Music Player Daemon plays radio streams. It has a utility called Music Player Commander that makes it easier to control.

Install these three with

	sudo apt update
	sudo apt install pipewire wireplumber mpd mpc

# Play a test sound
Now start PipeWire

	systemctl --user start pipewire
	
Connect the speakers.

Audio streams are identified by a number. To find out the number of the speakers run

	wpctl status
	
The output shows various audio devices.

```
PipeWire 'pipewire-0' [1.2.7, fran@franpi, cookie:812395030]
 └─ Clients:
        33. WirePlumber                         [1.2.7, fran@franpi, pid:3676]
        34. WirePlumber [export]                [1.2.7, fran@franpi, pid:3676]
        68. wpctl                               [1.2.7, fran@franpi, pid:3813]

Audio
 ├─ Devices:
 │      53. Built-in Audio                      [alsa]
 │      54. Built-in Audio                      [alsa]
 │  
 ├─ Sinks:
 │  *   63. Built-in Audio Stereo               [vol: 0.40]
 │  
 ├─ Sink endpoints:
 │  
 ├─ Sources:
 │  
 ├─ Source endpoints:
 │  
 └─ Streams:
```

Below this are `Video` and `Settings` but these aren't important just now.

In PipeWire jargon, a place where audio is output from your Raspberry Pi is a *sink*. The output of `wpctl status` shows that device 63 is a sink, described as the `Built-in Audio Stereo`. This is the 3.5 mm audio jack socket on the Raspberry Pi. It also shows that the volume is set to `0.40`, which means 40 %.

Play a sound by typing

	pw-play /usr/share/sounds/alsa/Noise.wav
	
If it's too quiet, turn up the volume

	wptcl set-volume 63 100%
	pw-play /usr/share/sounds/alsa/Noise.wav	
	
replaing `63` with the number of your sink.

You might wonder why there are two devices called `Built-in Audio`. One is the 3.5mm jack, the other is the HDMI. To find out more, use `wpctl inspect 53`, replacing `53` with the device identifier.

# Configure Music Player Daemon
[Music Player Daemon](https://www.musicpd.org/) is a program that runs in the background and continuously plays music. It can easily be used to play internet radio streams.

It needs a bit of setup to get it running. Open the configuration file in the *GNU nano* editor

	sudo nano /etc/mpd.conf

There are two changes to make to the Music Player Daemon's default configuration.

First, set the user which runs Music Player Daemon to the user you created earlier (`fran` in the example).

In the `General music daemon options`, find the line that says

	user     "mpd"
	
and change it to

    user     "fran"
	
where `fran` is the username you set up earlier.

Second, tell Music Player Daemon to output audio to PipeWire. Scroll down to find the audio output section, then add these lines

	audio_output {
        type    "pipewire"
        name    "PipeWire Sound Server"
	}
	
Now change the owner of the Music Player Daemon's files to `fran` (or whatever username you chose).

	sudo chown -R fran:fran /var/lib/mpd

Now stop Music Player Daemon from running as a system service and start it as a user service.

	sudo systemctl stop mpd
	sudo systemctl disable mpd
	systemctl --user enable mpd
	systemctl --user start mpd

# Play a radio station
You are now ready to play your first radio station.

	mpc add "http://direct.fipradio.fr/live/fip-midfi.mp3"
	mpc play

You should hear music. Felicitations! You now have a working internet radio player. (You can type `mpc stop` when you are ready to switch it off.)

# Connect a television or monitor
If you have a television or monitor with a High-Definition Multimedia Interface (HDMI) cable and built in sound it should just work when connected.

# Connect USB audio output
Various devices output audio over the Universal Serial Bus (USB) including some types of speakers or amplifers. If you have a USB connection on your sound output there's a good chance it will work.

First, find out how your device is described in PipeWire. Type

	watch wpctl status
	
This will display (among other things) a list of audio devices. Preceeding the command with `watch` causes it to update every 2 seconds so you'll see if a new device appears.

Have a look at the audio section. You can see there are two built-in audio devices (`55` and `56` in the example below) and one built-in-audio stereo sink (`69` in the example below). The `*` next to the sink indicates it is currently the default audio sink.

```
Audio
 ├─ Devices:
 │      55. Built-in Audio                      [alsa]
 │      56. Built-in Audio                      [alsa]
 │
 ├─ Sinks:
 │  *   69. Built-in Audio Stereo               [vol: 1.00]
 │
```

Now connect your USB device. You should see a new device appear in the list of `Devices` and its corresponding sink in the list of `Sinks`.

For example, after connecting a PMA-60 amplifier, this is the output of `wpctl status`.

```
Audio
 ├─ Devices:
 │      55. Built-in Audio                      [alsa]
 │      56. Built-in Audio                      [alsa]
 │      75. PMA-60                              [alsa]
 │
 ├─ Sinks:
 │      69. Built-in Audio Stereo               [vol: 1.00]
 │  *   76. PMA-60 Analog Stereo                [vol: 0.40]
```

PipeWire has detected the new device, set it up and then made it the default audio sink (`*`). Now any sound will be played from this new device. The volume's a bit low, 0.4, so turn it up to maximum

	wpctl set-volume 76 100%

# Connect a Bluetooth speaker or headphones
Make sure that this package is installed

	sudo apt install bluez-alsa-utils

Connecting a Bluetooth audio output device requires a tool called `bluetoothctl`. Connect to your Raspberry Pi and type

	bluetoothctl
	
Don't run `bluetoothctl` with `sudo` as it needs to know your username. You should see

```
Agent registered
[CHG] Controller XX:XX:XX:XX:XX:XX Pairable: yes
[bluetooth]# 
```

with an address shown in place of `XX:XX:XX:XX:XX:XX`.

Now switch on scanning by typing

```
scan on
```

You'll see a lot of output as various devices are found.

```
Discovery started
[CHG] Controller XX:XX:XX:XX:XX:XX Discovering: yes
[NEW] Device XX:XX:XX:XX:XX:XX XX-XX-XX-XX-XX-XX
```

There's no obvious way to figure out which device is which.

Put your device into pairing mode. This may involve pressing a special button in a regular way or pressing a reglar button in a special way. Often there is a flashing light whilst the device is in pairing mode.

Keep an eye on the scan out. You are looking for something that appears to be a new Bluetooth speaker or headphones.

```
[CHG] Device XX:XX:XX:XX:XX:XX RSSI: -57
[CHG] Device XX:XX:XX:XX:XX:XX TxPower: 4
[CHG] Device XX:XX:XX:XX:XX:XX Name: BOOM 3
[CHG] Device XX:XX:XX:XX:XX:XX Alias: BOOM 3
[CHG] Device XX:XX:XX:XX:XX:XX Class: 0x00240418
[CHG] Device XX:XX:XX:XX:XX:XX Icon: audio-headphones
```
There it is! Note the address (`XX:XX:XX:XX:XX:XX`) and then stop the scan.

```
scan off
```

Now it's time to connect.

```
pair XX:XX:XX:XX:XX:XX
```

If it works, you'll see

```
Attempting to pair with XX:XX:XX:XX:XX:XX
[CHG] Device XX:XX:XX:XX:XX:XX Connected: yes
[CHG] Device XX:XX:XX:XX:XX:XX Bonded: yes
[CHG] Device XX:XX:XX:XX:XX:XX ServicesResolved: yes
[CHG] Device XX:XX:XX:XX:XX:XX Paired: yes
Pairing successful
```

and perhaps some other output.

Now trust your new device.

```
trust XX:XX:XX:XX:XX:XX
```

The output will be something like this

```
[CHG] Device XX:XX:XX:XX:XX:XX Trusted: yes
Changing XX:XX:XX:XX:XX:XX trust succeeded
```

If your device is still in pairing mode, switch off pairing mode.

Now try to connect.

```
connect XX:XX:XX:XX:XX:XX
```

If this works, you'll see something like this

```
Attempting to connect to XX:XX:XX:XX:XX:XX
[CHG] Device XX:XX:XX:XX:XX:XX Connected: yes
[NEW] Endpoint /org/bluez/hci0/dev_XX_XX_XX_XX_XX_XX/sep1 
[NEW] Transport /org/bluez/hci0/dev_XX_XX_XX_XX_XX_XX/sep1/fd0 
[CHG] Transport /org/bluez/hci0/dev_XX_XX_XX_XX_XX_XX/sep1/fd0 Delay: 0x0096 (150)
Connection successful
[CHG] Transport /org/bluez/hci0/dev_XX_XX_XX_XX_XX_XX/sep1/fd0 Volume: 0x002f (47)
[CHG] Device XX:XX:XX:XX:XX:XX ServicesResolved: yes
[BOOM 3]# 
```

Note that the command prompt has now changed from `[bluetooth]#` to `[BOOM 3]#`.
