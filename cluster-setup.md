# Building your own AWS: a 6-node Raspberry Pi 4 Cluster

---

![alt text](https://lh3.googleusercontent.com/yPUjqIXq_UZVSVd0TqYtAD48XX_EyoZXHvw2aPtZRSa-01zXuddLSmyvJDrfsOZIgOt-IQKKB_vugQmhzwgOn2ZgjZm8fErNbhhMsMDlAre5pwfoemZ0pE4O7-HWEkoisWjiaekOHJiUJ64DRqXAxCrhLcOGJ1SY4X0p68wJ_1wNd2GdsiSn15lcwZSYmEITFWYGR6swrT1tIzCLraeGnfdEGm12KJTrLHpPeH4yDqSAjFj8SUKzkeawUgNrgsTUPx50rTToHQHyVrkQEYggRBuxIHfLMHOMMcQJFKBMhN2E_KeOr4qsMLhBOjLP2rHlbPXrowDJzPkZ20mHTRWOLkMTFLWZ7JsFryLQNmnzEkXhmx1j9RG65E6aNX7pcpJnePm8vFO-5bpv59Kf1tFddGedjpIGF-TZirRD6l7qFJ39CtsPe18tIqXDHtQ0ARwrNZiK7-1PgM6uhjBKAnMXJRnfQEh1FfmLicBeVE3nRQcR9-yKrkCJ2KVUICcB3dAd6hy4F-sp_j0EYxq4ol2iI118GAkDq0CzJL5BxkYh987mnbqXH6qbfnhtMIOsMTJAVNCufTDuorlbiKCZ0azJ2MZGo3l6bPWm5GFJraFQjx7gNzi-KaRjnEdm687-ZSword0T80EtRSrVsrT-F1sAodKZs_jyChTBSgMpYqx1ZO7gkdloRQVNtq2aykkikU6Dha_ojYFNNZft4ww6rUCQ3ALmr2aKElWHDL_eMtcmMZ1xzb1s=w1762-h857-no "Raspberry Pi 4 Cluster")

---
# Summary
1. Overview
2. Hardware
3. Preparing SD Card
4. Initial Pi Setup
5. Done: next ideas
---
### 1. Overview

I built this cluster for many reasons:

* Processing power
  * This setup itself has 24 cores in total (each Pi has 4 cores x 6 units) running at 1.5 Ghz (2 Ghz overclocked)
* Studies
  * Lerning Kubernetes, docker swarm, monitoring and managing multiple nodes,
* Research and Benchmarking
   * Comparison between protocols (udp, tcp, http, mqtt), cpu stock vs overclocked, how many clients we can handle, etc. 

---
### 2. Hardware

The case itself was 3d printed on a Prusa i3 MK3 using PLA filament, I will leave the links for the printable parts and some footage from the process:
* Files to print (.stl)
  * Stack mount: https://www.thingiverse.com/thing:2883047?fbclid=IwAR2jm9g8h_Y0gDxOqPw_cflQelIKZe3qx8vo1xCUZ1UaMJIDThuyJ2HnjDc
  * Wall bracket: https://www.thingiverse.com/thing:3594018?fbclid=IwAR1--Dl6uY-RaW55rIvJzcI6RYXuRX-IVUTYTWWs4xdIhilOPVdKukupomA
  * Stack mount (anker battery): https://www.thingiverse.com/thing:2758155?fbclid=IwAR2l5pLktHg4jz6l8797r8oWRwOJFtJXxyNyz8wqeyBAGBPAq5kllu0fnkY
* Footage
  * Printer: https://photos.app.goo.gl/FRSSnsJZwtBNswXL8
  * Close up: https://photos.app.goo.gl/4cYvbbwHub425opw7

Plus this list of items purchased (link included):

| Qty           | Item          | Price (CDN$) *                                                                                                |
| ------------- |:-------------:| ----------------------------------------------------------------------------------------------------:|
| 2             | [Raspberry Pi 4 - 4 GB](https://www.buyapi.ca/product/raspberry-pi-4-model-b-4gb/ "Raspberry Pi 4 - 4 GB") |  $74.25 |
| 2             | [Raspberry Pi 4 - 2 GB](https://www.buyapi.ca/product/raspberry-pi-4-model-b-2gb/ "Raspberry Pi 4 - 2 GB") |  $59.95 |
| 2             | [Raspberry Pi 4 - 1 GB](https://www.buyapi.ca/product/raspberry-pi-4-model-b-1gb/ "Raspberry Pi 4 - 1 GB") |  $46.95 |
| 2             | [SD Card Gigastone 32 GB (5 pack)](https://www.amazon.ca/Gigastone-32GB-U1-C10-Nintendo/dp/B07N73LB4T/ref=sr_1_3 "SD Card Gigastone 32 GB (5 pack)") |  $32.98 |
| 6             | [Pimoroni Fan SHIM](https://www.amazon.ca/Pimoroni-Fan-SHIM-Raspberry-Expectancy/dp/B07TTTCN8H/ref=sr_1_1 "Pimoroni Fan SHIM") |  $19.99 |
| 1             | [Micro HDMI to HDMI cable](https://www.amazon.ca/UGREEN-Ethernet-Support-Resolution-Cameras/dp/B00B2HORKE/ref=sr_1_5 "Micro HDMI to HDMI UGREEN") |  $11.99 |
| 1             | [Copper Heatsink (8 pack)](https://www.amazon.ca/Gigastone-32GB-U1-C10-Nintendo/dp/B07N73LB4T/ref=sr_1_3 "Copper Heatsink (8 pack)") |  $9.99 |
| 1             | [Arctic MX-4 Thermal Paste](https://www.amazon.ca/MX-4-4G-Thermal-Compound-Coolers-Durability/dp/B07PZSTW52/ref=sr_1_1_sspa "Arctic MX-4 Thermal Paste") |  $12.99 |
| 2             | [USB Type C Cable 1ft (5 pack)](https://www.amazon.ca/Canjoy-Braided-Charging-Compatible-Samsung/dp/B07KFX18KJ/ref=sr_1_5 "USB Type C Cable 1 ft (5 pack)") |  $12.99 |
| 1             | [Sabrent 60 Watt (12 Amp) 10-Port](https://www.amazon.ca/Sabrent-Family-Sized-Charger-Technology-AX-TPCS/dp/B00OJ79UK6/ref=sr_1_6 "Sabrent 60 Watt (12 Amp) 10-Port") |  $36.99 |
| 1             | [NETGEAR S8000 8-Port Gigabit ](https://www.amazon.ca/NETGEAR-GS808E-100NAS-Nighthawk-Streaming-Advanced/dp/B01MU3GE5L/ref=sr_1_13 "NETGEAR S8000 8-Port Gigabit") |  $102.99 |

* The currency is Canadian Dollars.

---
### 2. Preparing SD Card

First, we need to download the OS. I am using Raspbian Buster Lite (terminal only), but there is a Desktop version included, but consuming more resources - which is something that we do not want to waste in a cluster setup.
* Raspbian:
  * https://www.raspberrypi.org/downloads/raspbian/
* Other options:
  * https://www.raspberrypi.org/downloads/

Once we have downloaded the OS image, we have to save it in the SD Card:
* https://www.raspberrypi.org/documentation/installation/installing-images/README.md

Done, now we can go to the next step!

---
### 3. Initial Pi Setup

Once we have our SD card ready, we are now going to say "hello world" to each Pi node: 
* Connect the SD Card to the board
* Connect the ethernet cable to the board to the router
* Connect the USB-C cable to power on

[TODO...]



Fan Shim Python Script:
* Install Python:
  * $ sudo apt install python3-pip
* https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-fan-shim

Custom config
* Turn on at 60°
* Turn off at 50°
* RGB at 255
```
$ cd ~/fanshim-python/examples && sudo ./install-service.sh --on-threshold 60 --off-threshold 50 --delay 2 --brightness 255
```

Watching temp
```
$ watch -n 1 vcgencmd measure_temp
```

---
### 4. Done: next ideas

NGINX
https://github.com/renancaldas/aws-ec2-guide#3-nginx

[TODO...]


