## Waydroid install on armbian bullseye server release
Tested with https://github.com/MX10-AC2N/amlogic-s9xxx-armbian/releases/tag/kernel_5.15.48 over https://github.com/ophub/amlogic-s9xxx-armbian/releases/download/Armbian_Aml_bullseye_server_06.20.1351/Armbian_22.08.0_Aml_s922x_bullseye_5.15.48_server_2022.06.20.img.gz

Flashed on USB support.

```
sudo su
apt update
apt install gnome-session-wayland
apt install gnome-terminal
apt install gdm3                  
#apt remove lightdm xfce4
nano /etc/gdm3/custom.conf                  #ensure WaylandEnable=false is commented
nano /usr/lib/udev/rules.d/61-gdm.rules     #comment all lines
sed -i "s/# sleep-inactive-ac-type='suspend'/sleep-inactive-ac-type='nothing'/" /etc/gdm3/greeter.dconf-defaults
sed -i "s/# sleep-inactive-ac-timeout=1200/sleep-inactive-ac-timeout=0/" /etc/gdm3/greeter.dconf-defaults
sed -i "s/# sleep-inactive-battery-timeout=1200/sleep-inactive-battery-timeout=0/" /etc/gdm3/greeter.dconf-defaults
systemctl restart gdm3
echo $XDG_SESSION_TYPE                      #it still shows tty
reboot
echo $XDG_SESSION_TYPE                      #now it shows wayland
```

## Check DISTRO release
``` 
lsb_release -c
```

## Waydroid install 
```
export DISTRO="bullseye" && \               # Adjust DISTRO release
sudo curl -# --proto '=https' --tlsv1.2 -Sf https://repo.waydro.id/waydroid.gpg --output /usr/share/keyrings/waydroid.gpg && \
echo "deb [signed-by=/usr/share/keyrings/waydroid.gpg] https://repo.waydro.id/ $DISTRO main" > ~/waydroid.list && \
sudo mv ~/waydroid.list /etc/apt/sources.list.d/waydroid.list && \
sudo apt update
```

## Launch Waydroid-container
```
waydroid init
systemctl start waydroid-container
waydroid container start
waydroid session start
```

