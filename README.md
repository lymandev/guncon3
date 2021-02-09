# Overview
## Guncon 3
 The Guncon 3 was originally released for the PlayStation 3. It is a USB light gun controller that uses IR leds for aiming. The Guncon 3 was used with the games Time Crisis 4, Time Crisis: Razing Storm, and Deadstorm Pirates.

## Kernel Driver
Linux kernel driver for the Namco Guncon 3 light gun. The kernal driver is written in C and was originally developed by [beardypig](https://github.com/beardypig/guncon3).

This module has been tested on:
- Ubuntu 14.04
- Ubuntu 17.04
- Ubuntu 18.04
- Raspbian 10 (buster) Kernal version: 5.4.72-v7+ on the Raspberry Pi 3B+

There are couple of bugs and some work is still required to make it fully stable.

# Raspberry Pi Setup

Install [RetroPie](https://retropie.org.uk/download/) then press `shift+F4` to access the terminal. Then download and install the following:

```
$ sudo apt-get install git bc bison flex libssl-dev
$ sudo wget https://raw.githubusercontent.com/RPi-Distro/rpi-source/master/rpi-source -O /usr/local/bin/rpi-source && sudo chmod +x /usr/local/bin/rpi-source && /usr/local/bin/rpi-source -q --tag-update
```

Now run rpi-source, this is needed to build the kernal driver.

`$ sudo rpi-source`

The run

`$ sudo apt install libncurses5-dev`

## Download, Build, and Install the Guncon3 driver

First lets move to the /pi folder.

`$ cd /home/pi`

Now lets clone the git repo,

`$ git clone https://github.com/lymandev/guncon3.git`

and now move to the /guncon3 folder.

`$ cd /guncon3`

Now we can build and install the driver using `make`.

```
$ make
$ sudo make modules_install
$ sudo depmod -a
```

The driver should now be built and installed you can now enable it with `modprobe`.

`$ sudo modprobe guncon3`

## Testing Your Guncon3

Once the module is installed the Guncon3 should appear as a regular joystick that you should be able to see with `jstest-gtk`, so lets install it.

`$ sudo apt-get install jstest-gtk`

Now lets check that your controller is found and working.

```
$ cd /dev/input/
$ ls -al
```

You should see js0 (depending on how many controllers you have connected it might be js1). Finally we can test it by running:

`$jstest js0`

You should now see your controller registering inputs on the screen.

## RetroArch Config

The last thing we can do is copy the Guncon3 config file for RetroPie.

```
$ cd /home/pi/guncon3
$ cp guncon3.cfg /home/pi/.config/retroarch/autoconfig
```

You can now return to RetroPie and navigate the menu using the guncon!

# Usage

There is additional work required to acutally get the guncon working in games. MAME has also not been setup to use the light gun yet.

## Helpful Links

- [beardypig's original blog post](http://www.beardypig.com/2016/01/06/guncon3/#driver-for-guncon-3)
- [beardypig](https://github.com/beardypig/guncon3)'s repo
- [pcnimdock](https://github.com/pcnimdock/guncon3)'s repo which includes updates and RetroArch config file
- Guide to using [rpi-source](https://github.com/RPi-Distro/rpi-source)
- [jstest-gtk](https://gitlab.com/jstest-gtk/jstest-gtk) repo
