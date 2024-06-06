# Eat that lettuce or not? Jetson Nano can help! 
This is an image classification project built on Jetson Nano that retrains resnet-18 on a lettuce disease dataset and aims to help people identify what's wrong with their lettuce by looking at the lettuce leaves. 

It will classify a lettuce leaf as healthy, bacterially infected, or virally infected and show how confident it is upon this judgement. Narrowing down the leaf condition to the above 3 classes makes it easier for people to search up online about whether their lettuce is safe to consume or not. 

For example, people can now simply search up "bacterially infected lettuce" without wondering which disease their lettuce have. They can easily answer whether if they can eat the whole thing or they have to compost some parts base on their search results. 

This should thus help us reduce food waste and food poisoning incidents. 

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
   After rebooting, you can check your swap again just to ensure. 

## Running The Project
### Preparation
1. [Run the pre-built docker container]( https://github.com/dusty-nv/jetson-inference/blob/master/docs/aux-docker.md), it might take a while especially if you run it for the first time. 
   ```
   $ git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
   $ cd jetson-inference
   $ docker/run.sh
   ```
2. Test your imagenet to ensure that it's working. It might take a while the first time you run it.
   ```
   # First, run docker container and go to build/aarch64/bin directory
   $ cd jetson-inference/build/aarch64/bin

   # 
   $ ./imagenet.py images/orange_0.jpg images/test/output_0.jpg

   # 
   $ ./imagenet.py --network=resnet-18 images/jellyfish.jpg images/test/output_jellyfish.jpg

   # 
   $ ./imagenet.py /dev/video0
   ```
3. After the jetson-inference docker container is downloaded and you have tested your imagenet, open chromium web browser on your jetson desktop. Then, go to [Kaggle Lettuce Diseases Dataset](https://www.kaggle.com/datasets/santoshshaha/lettuce-plant-disease-dataset) and download it.
   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/dc11716d-3142-4794-847d-a15d4c756fea)
   It should contain 2813 images in .jpg and already split into test, train, and val. 

4. Open files on your left panel bar, go to home, jetson-inference, python, training, classification, data and make a new folder called "lettuce".
   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/c2eae7b0-4464-40c9-947b-10d0a2d9cb66)

5. Then, go to downloads, right click on archive.zip and extract it to jetson-inference/python/training/classification/data/lettuce.
   
6. Now, open the lettuce folder in jetson-inference/python/training/classification/data. You should see 3 folders each as test, train, and valid.
   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/d5324380-7a77-4af5-826c-638eed1c4479)
   
7. Rename the "valid" folder to "val". And create a labels.txt file inside the lettuce folder by writing the following to your terminal (make sure you are inside the docker container and inside jetson-inference/python/training/classification/data/lettuce):
   ```
   


run docker container

cd python

cd training

cd classification

python3 train.py --model-dir=models/lettuce --batch-size=4 --epochs=35 data/lettuce

it took around 4 hours for me

python3 onnx_export.py --model-dir=models/lettuce

imagenet --model=models/lettuce/resnet18.onnx --labels=data/lettuce/labels.txt --input_blob=input_0 --output_blob=output_0 /dev/video0


