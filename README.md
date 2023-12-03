# Building Linux for STM32MP157f-dk2 and Preparing an SD Card



## Introduction

The STM32MP157F-dk2 development kit is a powerful platform for embedded systems development. By harnessing the power of Linux, you can unlock a world of possibilities for your embedded applications. This guide will walk you through the process of building Linux for STM32MP157f-dk2 and preparing an SD card for seamless operation.

[Image of Poky Repository Screenshot]

## Prerequisites

Before you begin, ensure you have the following:

* A PC running Linux or macOS
* A USB Type-C cable to connect your STM32MP157F-dk2 board
* An SD card reader and an SD card with at least 8GB of storage
* A GitHub account
* Basic familiarity with Linux commands

## Step 1: Setting Up the Development Environment

### Clone the Poky Repository

1. Open a terminal window and navigate to the desired location for your project directory.
2. Clone the poky repository from the Yocto Project using the following command:

git clone https://git.yoctoproject.org/git/poky

### Navigate to the Poky Directory

3. Navigate to the cloned poky directory:

cd poky

### Switch to the Kirkstone-4.0.5 Branch

4. Switch to the kirkstone-4.0.5 branch using the following command:

git checkout kirkstone-4.0.5

### Clone the Meta-st-stm32mp Repository

5. Clone the meta-st-stm32mp repository from STMicroelectronics using the following command:

git clone https://github.com/STMicroelectronics/meta-st-stm32mp.git


### Switch to the Kirkstone Branch

6. Navigate to the meta-st-stm32mp directory:

cd meta-st-stm32mp


7. Switch to the kirkstone branch using the following command:

git checkout kirkstone


### Navigate Back to the Poky Directory

8. Navigate back to the poky directory:

cd ..


### Clone the Meta-openembedded Repository

9. Clone the meta-openembedded repository using the following command:

git clone https://github.com/openembedded/meta-openembedded.git


### Switch to the Kirkstone Branch

10. Navigate to the meta-openembedded directory:

cd meta-openembedded


11. Switch to the kirkstone branch using the following command:

git checkout kirkstone


### Return to the Poky Directory

12. Return to the poky directory:

cd ..


### Initialize the Build Environment

13. Initialize the build environment using the following command:

source poky/oe-init-build-env build


## Step 2: Building the Linux Image

### Verify the Layer Configuration

1. Verify the current layer configuration using the following command:

bitbake-layers show-layers

### Identify the Appropriate Machine Configuration File

2. Identify the appropriate machine configuration file for your STM32MP157F-dk2 board:

ls ../poky/meta-st-stm32mp/conf/machine/

### Modify the local.conf File

3. Open the local.conf file for editing:

vi conf/local.conf

4. Locate the `MACHINE` variable and set it to `stm32mp1`:

MACHINE ?= "stm32mp1"
5. Save and exit the local.conf file.

### Add Additional Layers

6. Add the following layers in the correct order:

bitbake-layers add-layer ../poky/meta-openembedded/meta-oe/
bitbake-layers add-layer ../poky/meta-openembedded/meta-python/
bitbake-layers add-layer ../poky/meta-openembedded/meta-networking/
bitbake-layers add-layer ../poky/meta-openembedded/meta-multimedia/
bitbake-layers add-layer ../poky/meta-st-stm32mp/

### Verify the Updated Layer Configuration

7. Verify the updated layer configuration:

bitbake-layers show-layers

### Configure your Git credentials:
8. Configure your Git credentials:

git config --global user.email "xxxxxx@xxxx.com"
git config --global user.name "xxxxxyyyyyy"

### Build the core-image-minimal image
9. Build the core-image-minimal image:
    
bitbake core-image-minimal

### Preparing the SD Card
1. Prepare the SD card using the flashlayout tool:
   
./create_sdcard_from_flashlayout.sh ../flashlayout_core-image-minimal/trusted/FlashLayout_sdcard_stm32mp157f-dk2-trusted.tsv

### Unmount and Flash the SD card with the generated image
1. umount
   
sudo umount /dev/sdb

3. Flash the SD card with the generated image:
   
sudo dd if=../FlashLayout_sdcard_stm32mp157f-dk2-trusted.raw of=/dev/sdb bs=8M conv=fdatasync
