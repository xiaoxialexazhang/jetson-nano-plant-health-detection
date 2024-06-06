# jetson-nano-plant-health-detection
This is a Hello AI World project on Jetson Nano 4GB that aims to help plant lovers see if their plants have diseases by showing the leaves of their plant thru camera. 

# Required devices
Jetson Nano 2GB/4GB (I'm using a 4GB)
Raspberry Pi Camera/USB Webcam (I'm using a usb webcam)
Power supply
Keyboard and mouse
Monitor and HDMI cable
Ethernet cable
Memory card (I'd suggest more than 32GB)

# Steps: 
1. Follow set up, flash sd card image, bleana echer
2. First boot
3. open terminal, write
```
$ git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
$ cd jetson-inference
$ docker/run.sh
```

5. While it takes 2-3 minutes to load, open Chromium on desktop and go to kaggle.com to download the dataset we are gonna be using https://www.kaggle.com/datasets/abdallahalidev/plantvillage-dataset
6. 
