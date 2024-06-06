# Eat that lettuce or not? Jetson Nano can help! 
This is an image classification project built on Jetson Nano that aims to help people identify what disease their lettuce might have by looking at the lettuce leaves. 

It will classify a lettuce leaf as healthy, bacterially infected, having downy mildew, having powdery mildew, having septoria blight, mixed with shepherd purse weed, virally infected, or having wilt and leaf blight. 

Narrowing down the leaf condition to the above 8 classes makes it easier for people to search up online about whether their lettuce is safe to consume or not: can they eat the whole thing or which parts do they have to compost? This helps us reduce food waste and food poisoning incidents. 

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
6. actually prolly this one cuz the previous one too big https://www.kaggle.com/datasets/ashishjstar/lettuce-diseases 
7. 
