# Eat that lettuce or not? Jetson Nano can help! 
This is an image classification project built on Jetson Nano that retrains resnet-18 on a lettuce disease dataset and aims to help people identify what's wrong with their lettuce by looking at the lettuce leaves. 

It will classify a lettuce leaf as healthy, bacterially infected, or fungally infected and show how confident it is upon this judgement. Narrowing down the leaf condition to the above 3 classes makes it easier for people to search up online about whether their lettuce is safe to consume or not. 

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
   
2. After your successful set up and first boot, you may want to increase swap memory to 4GB by opening up the terminal and write the following commands:
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
   git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
   cd jetson-inference
   docker/run.sh
   ```
2. Test your imagenet to ensure that it's working. It might take a while the first time you run each command. You can check the output images at jetson-inference/data/images/test. 
   ```
   # First, run docker container and cd to build/aarch64/bin
   docker/run.sh
   cd jetson-inference/build/aarch64/bin

   # Classify the orange pic using the default network googlenet
   ./imagenet.py images/orange_0.jpg images/test/output_0.jpg

   # Classify the jellyfish pic using resnet-18
   ./imagenet.py --network=resnet-18 images/jellyfish.jpg images/test/output_jellyfish.jpg

   # Run the live camera recognition
   ./imagenet.py /dev/video0      # if you are using usb webcam
   ./imagenet.py csi://0          # if you are using raspberry pi camera
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
   nano labels.txt
   ```
   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/665397f9-eb61-4310-b6c1-3bf5c108c5da)

   Edit the labels.txt file. Make sure all labels are in alphabetical order. There should be three labels, each in a line. 

   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/b9e8bad0-59cb-4c8a-9a52-87230a2f8ac3)

   Then, Ctrl+X to exit, type Y to save changes, and press enter to confirm that the changes are saved to labels.txt

   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/6f0b7c31-47e4-4831-b1c8-a47896961ff8)

9. You can type "exit" to exit the docker container. 

Now, we are finally done with prep work! We can now proceed on training our lettuce model.

### Training Your Lettuce Model
1. First, in your terminal, go to jetson-inference and run your docker container. Then, cd to python/training/classification
   ```
   cd jetson-inference
   docker/run.sh
   cd python/training/classification
   ```
2. Now we are about to re-train the resnet-18 model on our lettuce dataset, get excited!

   Depending on which Jetson you are using, you might want to change the default settings for --batch-size (default 8) and --workers (default 2) to save memory as much as possible.

   You could also set --epochs (default 35) to be a smaller number if you just want a very quick train and are eager to check out the lettuce health inspection sooner. 
   ```
   python3 train.py --model-dir=models/lettuce --batch-size=4 --workers=1 --epochs=35 data/lettuce
   ```
4. It should take around 5-6 hours to train. During this time, the heating fins on your Jetson will become very hot. Please be careful! 

   To stop the training anytime, press Ctrl+C. To resume training later, use --resume and --epoch-start flags.

   You can also test out other models to re-train by running the following code and later using the --arch flag.
   ```
   python3 train.py --help
   ```

6. After training is done, export the lettuce model into ONNX format. Make sure you are writing this command in jetson-inference/python/training/classificaiton that's inside the docker. 
   ```
   python3 onnx_export.py --model-dir=models/lettuce
   ```
   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/9757b992-8576-43f0-ab79-10e9fd481581)
   ![image](https://github.com/xiaoxialexazhang/jetson-nano-safe-lettuce/assets/170693946/5a431b12-4a5a-482f-8b59-d21a0978df0f)

   Be mindful of where your model is getting exported to, we are going to need it when it comes to testing!

   Here, in this tutorial, the model is being exported to models/lettuce/resnet18.onnx. If you are using a different model, the onnx file name might be different and should be used instead of resnet18.onnx when you want to test your model. 


### Testing Your Lettuce Model

Now, the lettuce model you just traind is ready for testing! You can test it in two ways (make sure you are still in docker and in jetson-inference/python/training/classification):

  - Using the test folder that's already in our lettuce folder:
    ```
    # Make directories for the output
    mkdir data/lettuce/test_bacterial_output data/lettuce/test_fungal_output data/lettuce/test_healthy_output

    # Process all the test images
    imagenet --model=models/lettuce/resnet18.onnx --labels=data/lettuce/labels.txt --input_blob=input_0 --output_blob=output_0 data/lettuce/test/Bacterial data/lettuce/test_bacterial_output
    imagenet --model=models/lettuce/resnet18.onnx --labels=data/lettuce/labels.txt --input_blob=input_0 --output_blob=output_0 data/lettuce/test/fungal data/lettuce/test_fungal_output
    imagenet --model=models/lettuce/resnet18.onnx --labels=data/lettuce/labels.txt --input_blob=input_0 --output_blob=output_0 data/lettuce/test/healthy data/lettuce/test_healthy_output
    ```
  - Using your camera to process your lettuce live:
    ```
    # csi camera users substitute /dev/video0 with csi://0
    imagenet --model=models/lettuce/resnet18.onnx --labels=data/lettuce/labels.txt --input_blob=input_0 --output_blob=output_0 /dev/video0
    ```

## Video
Here

## Model Accuracy 
