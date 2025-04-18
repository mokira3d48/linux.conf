## Disable touchscreen
If you have not already taken the advice from above, try what I did a few years ago?

G'day @S3l3n3 and welcome to linux.org :)

First of all check the output of

for the touchscreen entry

Then, in your file `usr/share/X11/xorg.conf.d/40-libinput.conf`.


```sh
sudo nano usr/share/X11/xorg.conf.d/40-libinput.conf
```

You may have a section that looks a bit like this

```conf
Section "InputClass"
        Identifier "libinput touchscreen catchall"
        MatchIsTouchscreen "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```

What you should do rather than just change on to off is alter it
to look like this

```conf
#Section "InputClass"
#        Identifier "libinput touchscreen catchall"
#        MatchIsTouchscreen "on"
#        MatchDevicePath "/dev/input/event*"
#        Driver "libinput"
#EndSection
```

Then save the file, reboot and see how you go.
Check the xinput output and you should see the previous reference removed.
If you try this and it works, I'll tell you a funny story about how I first used this.
