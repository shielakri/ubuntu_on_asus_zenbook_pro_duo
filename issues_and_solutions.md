# Ubuntu 20.04 on Asus Zenbook Pro Duo UX581LV (dual boot)

## Resolved issues

### WiFi not detected
When testing Ubuntu on USB stick, the WiFi was not detected after booting from Windows. It was however detected after booting from Ubuntu.

#### Solution
Disabling Fast Boot in Windows. Follow [this link](https://help.uaudio.com/hc/en-us/articles/213195423-How-To-Disable-Fast-Startup-in-Windows-10).

### Unable to install Ubuntu with Intel RST
Under the installation process, Ubuntu refuses to continue until Intel's Rapid Storage Technology (RST) software is disabled.

#### Solution
The solution is to disable Intel RST. The 'how to' is gotten from [How to install Ubuntu on your Asus Zenbook Pro](https://jeroen.pro/2019/05/how-to-install-ubuntu-on-your-asus-zenbook-pro/?fbclid=IwAR1MFaQufni8Rs1reRJteimvUXhazMChcsoHMV7fikxMzBzOPvVXvrO7J-k).

1. Boot into Windows
2. Open command prompt (run as administrator). Press the wind button, type `cmd` and right click to press `run as administrator`.
3. Type this command and press enter: `bcdedit /set {safeboot minimal}`. This is to boot Windows in safe mode.
4. Shut down.
5. Turn on the computer and press `Esc` button to enter into BIOS setup.
6. On the `Advanced tab`, change the SATA Configuration > SATA Mode Selection from Intel RST to **AHCI**.
7. Go to the `Save & Exit` tab and save changes and exit setup. Windows will then automatically boot into safe mode.
8. Open the command prompt and run as administrator again.
9. Type this command and press enter: `bcdedit /deletevalue {current} safeboot`. This disables safeboot.
10. Restart.
11. Now you can resize the Windows partition to create space for Ubuntu.

### Unable to install Ubuntu with third party software
After some research, this seemed to be a bug from Ubuntu (as of 20.12.2020). The installation crashed when almost finished. Ubuntu was able to dual boot anyway. However,
I deleted the partition and started from scratch to get a clean install.

#### Solution
The solution is to install the third party software after the installation. The solution is from user *Chrollo283* in [this thread](https://www.reddit.com/r/Ubuntu/comments/iyj18z/installer_crashed_for_some_reason_if_anyone_could/). Install Ubuntu without checking the box for third party software. Once installed, perform an upgrade by opening the terminal and typing:

```
sudo apt update && sudo apt upgrade -y
```

Then install the NVidia drivers manually.

Detect the model of your NVidia graphic card:

```
ubuntu-drivers devices
```

Install the recommended driver:

```
sudo ubuntu-drivers autoinstall
```

Commands for installing the driver is from [here](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-20-04-focal-fossa-linux).

### Unable to configure pen
It was impossible to configure the pen using the normal settings under Wacom tablet (no idea why I was able to configure it while booting from USB stick).

#### Solution
Solution is gotten from user *s-light* in thread [Linux on Asus Zenbook Pro Duo](https://www.reddit.com/r/ASUS/comments/denjgl/linux_on_asus_zenbook_pro_duo_ux581gv/).

Open the terminal and paste these commands. The first two lines configure that primary screen, while the last two configure the secondary screen.

```
xinput map-to-output  "pointer:ELAN9008:00 04F3:29B6" eDP-1-1
xinput map-to-output  "ELAN9008:00 04F3:29B6 Pen (0)" eDP-1-1

xinput map-to-output  "pointer:ELAN9009:00 04F3:29A1" DP-1-2
xinput map-to-output  "ELAN9009:00 04F3:29A1 Pen (0)" DP-1-2
```

## Unresolved issues

### Sound only the first few seconds.
It worked earlier, so I don't know what happened.

### Keyboard battery critically low
According to [this question](https://askubuntu.com/questions/1190836/keyboard-battery-low-for-laptops-built-in-device), it seems like the keyboard is actually the touch screens.

### Brightness control
The default brightness keys do not work (only the GUI works).

#### Temporary fix
A temporary fix is to use `xrandr` to manually change the brightness:

```
xrandr --output eDP-1-1 --brightness 0.25
```
Set the brightness value to whatever you desire.

### Setting default monitor display
I would like to not always have to configure the displays.

## Weird things that happen in Windows

### Unresolved issues

#### Netflix looses sound
Booting Windows after having used Ubuntu results in Netflix placing the play symbol over the video which is playing, but with no sound. The quick fix is just to update the page. It happens only once, but after every time Ubuntu has been booted last. It's interesting as this happened on my previous laptop as well.


## Good to know

### How to remove Ubuntu from the laptop
See the end of [this video](https://www.youtube.com/watch?v=aKKdiqVHNqw&t=10s&ab_channel=KskRoyal).
