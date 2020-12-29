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
xinput map-to-output  "pointer:ELAN9008:00 04F3:29B6" eDP-1
xinput map-to-output  "ELAN9008:00 04F3:29B6 Pen (0)" eDP-1

xinput map-to-output  "pointer:ELAN9009:00 04F3:29A1" DP-2
xinput map-to-output  "ELAN9009:00 04F3:29A1 Pen (0)" DP-2
```
However, the terminal gives the error `unable to find device 'ELAN9008:00 04F3:29B6' Pen (0)` if the pen is not used first.

### Unable to control brightness on Screenpad and toggle it on and off
It is not possible to control the Screenpad out of the box.

#### Solution
The solution is gotten from GitHub user *Plippo*. He has written a Windows Management Instrumentation (WMI) driver after reading the article [Writing a WMI driver - an introduction](https://lwn.net/Articles/391230/). If you check his comment [here](https://github.com/s-light/ASUS-ZenBook-Pro-Duo-UX581GV/issues/1) from May 25 2020, he describes the process he used to develop the driver.

His end product is called the [asus-wmi-screenpad](https://github.com/Plippo/asus-wmi-screenpad) and instructions on how to install and use it is written in the README.md file.

He has also written a simple script in his repository [screenpad-tools](https://github.com/Plippo/screenpad-tools) that allows for a more user friendly command line interface. By combining this script with keyboard shortcuts, you can easily change the brightness of the Screenpad and even toggle it on and off.

I have used these shortcuts:
* Brightness down: Win + F4
* Brightness up: Win + F5
* Toggle on/off: Launch7 (the same button used on Windows)

> When placing the line `sudo chmod a+w '/sys/class/leds/asus::screenpad/brightness'` in your `/etc/rc.local` file, remember to make the file executable by running the command `sudo chmod +x /etc/rc.local` if it is not already executable.


### Brightness control on OLED screen
Linux does not work out of the box for computers with OLED screens. The GUI elements are working though.

#### Solution
The solutions is to install the gnome extension [OLED Dimmer](https://extensions.gnome.org/extension/1222/oled-dimmer/). This extension will control both screens with the deafult key bindings. It also works with night light. The drawback is that the two screens cannot be controlled independently. However, with the Screenpad brightness solution it is possible to make the Screenpad darker than the main monitor but not vice versa.

##### Temporary solution to control independently
A temporary fix is to use `xrandr` to manually change the brightness:

```
xrandr --output eDP-1 --brightness 0.25
```
Set the brightness value to whatever you desire for the monitor (eDP-1 or DP-2) of your choice.

## Unresolved issues

### Touch (with fingers) not working after using stylus pen
This happens after using the pen for a while. It also seems like this happens on the screen on which the pen is first used.

### Sound only the first few seconds.
It worked earlier, so I don't know what happened. It has happened once after installing the OLED dimmer.

### Keyboard battery critically low
According to [this question](https://askubuntu.com/questions/1190836/keyboard-battery-low-for-laptops-built-in-device), it seems like the keyboard is actually the touch screens.
This happens one time on each screen after using the pen for the first time that session.

### Setting default monitor display
I would like to not always have to configure the displays.

## Weird things that happen in Windows

### Unresolved issues

#### Netflix looses sound
Booting Windows after having used Ubuntu results in Netflix placing the play symbol over the video which is playing, but with no sound. The quick fix is just to update the page. It happens only once, but after every time Ubuntu has been booted last. It's interesting as this happened on my previous laptop as well.

#### The clock is one hour wrong
This happens when i boot into Windows after using Ubuntu. It corrects itself after some time.

## Good to know

### How to remove Ubuntu from the laptop
See the end of [this video](https://www.youtube.com/watch?v=aKKdiqVHNqw&t=10s&ab_channel=KskRoyal).
