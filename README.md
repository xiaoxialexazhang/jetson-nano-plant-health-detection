# Eat that lettuce or not? Jetson Nano can help! 
This is an image classification project built on Jetson Nano that aims to help people identify what disease their lettuce might have by looking at the lettuce leaves. 

It will classify a lettuce leaf as healthy, bacterially infected, having downy mildew, having powdery mildew, having septoria blight, mixed with shepherd purse weed, virally infected, or having wilt and leaf blight. 

Narrowing down the leaf condition to the above 8 classes makes it easier for people to search up online about whether their lettuce is safe to consume or not: can they eat the whole thing or which parts do they have to compost? This helps us reduce food waste and food poisoning incidents. 

## Requirede Hardwares
Jetson Nano 2GB/4GB

Camera (USB webcam/Raspberry Pi camera)

Power supply (https://forums.developer.nvidia.com/t/power-supply-considerations-for-jetson-nano-developer-kit/71637)

Keyboard and mouse

Monitor and HDMI cable

Ethernet cable

Memory card (suggest 64GB)

PC/laptop (to flash the SD card)

## Setting Up and First Boot
1. First, set up your Jetson by following the instructions here: [Setting up Jetson with JetPack](https://github.com/dusty-nv/jetson-inference/blob/master/docs/jetpack-setup-2.md). You may want to connect ethernet cable and your camera during first boot as well.
   
2. After your successful set up and first boot, you may want to increase swap memory to 4GB by opening up the terminal and write:
   ```
   # Check your swap (should see 4071 if 4GB swap)
   free -m
   
   # Disable ZRAM
   sudo systemctl disable nvzramconfig

   # Create 4GB swap file
   sudo fallocate -l 4G /mnt/4GB.swap
   sudo chmod 600 /mnt/4GB.swap
   sudo mkswap /mnt/4GB.swap

   # Append the following line to /etc/fstab
   sudo su
   echo "/mnt/4GB.swap swap swap defaults 0 0" >> /etc/fstab
   exit

   # REBOOT!
   ```

## Running The Project
1. [Run the pre-built docker container]( https://github.com/dusty-nv/jetson-inference/blob/master/docs/aux-docker.md), it might take a while especially if you run it for the first time. 
   ```
   $ git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
   $ cd jetson-inference
   $ docker/run.sh
   ```
2. After the jetson-inference docker container is downloaded, open chromium web browser on your jetson desktop. Then, go to [Kaggle Lettuce Diseases Dataset](https://www.kaggle.com/datasets/ashishjstar/lettuce-diseases) and download it. 

3. 
