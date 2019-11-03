Open up a terminal window:
```
$ sudo apt update && sudo apt upgrade && sudo rpi-update && sudo reboot
```

After restarting:
```
$ sudo nano /boot/config.txt
```
Scroll down to the very end and insert
 
```
over_voltage=4
arm_freq=2000
```

You can also overclock the GPU if needed: 
```
gpu_freq=600
```

Ctrl+X and Y to save and run:
```
$ sudo reboot
```

After restarting, this will display the current CPU speed in real time:
```
watch -n1 vcgencmd measure_clock arm 
```


