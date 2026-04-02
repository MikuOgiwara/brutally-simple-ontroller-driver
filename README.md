## Brutally Simple Ontroller Driver

This is a segatools io4 module for [ONTROLLER](https://www.dj-dao.com/en/ontroller) written in C with libusb. The name is supposed to scare off Windows users; you probably want to use [Mu3IO.NET](https://github.com/SirusDoma/Mu3IO.NET). If you play with a keyboard on Linux, you also want Mu3IO.NET.

### Why

Because the existing solutions didn't work on Linux in HID mode. Winusb [seems to be unimplemented](https://wiki.winehq.org/Hardware). It doesn't help that Mu3IO.NET is made specifically for .NET AOT which can't be cross-compiled on Linux.

### How it works

The DLL seen by segatools is a thin wrapper built with `winegcc` which redirects to a native Linux library (that uses the system's native libusb).

### Setup

By default, ONTROLLERs seem to be inaccessible in user space. Add a udev rule:

```sh
# echo 'SUBSYSTEMS=="usb", ATTRS{idVendor}=="0e8f", ATTRS{idProduct}=="1216", MODE="0666"' > /etc/udev/rules.d/10-ontroller.rules
# udevadm control --reload-rules
```

Re-plug the ONTROLLER if it was connected.

Add `mu3io_bsod.dll` and `mu3io_bsod.so` to the game directory and point at the DLL in `segatools.ini`:

```ini
[mu3io]
path=mu3io_bsod.dll
```

### Known issues
* WAD LEDs don't work

# Fork 
To fix the WAD LED issues, I have forced the WAD LEDs to a solid pink color.
If you want to change this, open `led.c` and modify the RGB values.
```c
        set_color(6, 255, 20, 147); 
        set_color(7, 255, 20, 147);

        set_color(8, 255, 20, 147);
        set_color(9, 255, 20, 147);
```

After that, recompile using make.

If someone finds how to make the WAD LEDs work with Ongeki, please fork this project and let me know so I can update my Ongeki Linux setup.

You can find my attempts on my blog, along with some possibly useful resources to help make it work.