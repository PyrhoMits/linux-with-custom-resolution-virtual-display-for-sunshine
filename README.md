1. First download EDID from `https://git.linuxtv.org/v4l-utils.git/tree/utils/edid-decode/data`, think of it as similar to dummy HDMI plug.

2. Create directory for EDID 

```
sudo mkdir -p /usr/lib/firmware/edid
```

and

Add the file to folder

```
sudo cp lg-34gn850b-dp /usr/lib/firmware/edid/
```

3. Configure kernel parameter (To test first)

```
sudo vim /boot/limine.conf

# Add this parameter: 

drm.edid_firmware=DP-2:edid/lg-34gn850b-dp video=DP-2:e"

```

DP-2 stands for Display Port 2, you can view in video output with 

```
for p in /sys/class/drm/*/status; do con=${p%/status}; echo -n "${con#*/card?-}: "; cat $p; done
```

And restart to that kernel, if it work add it to 

```
sudo nano /etc/mkinitcpio.conf

# FILES
# This setting is similar to BINARIES above, however, files are added
# as-is and are not parsed in any way.  This is useful for config files.
FILES=(/usr/lib/firmware/edid/lg-34gn850b-dp)
```

Regenerate boot config

```
sudo mkinitcpio -P
```