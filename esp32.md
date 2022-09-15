# ESP32

## Step 1

### Install Prerequisites

sudo apt-get install flex bison gperf python3-pip python3-setuptools cmake ninja-build ccache libffi-dev libssl-dev dfu-util

### Permission issues /dev/ttyUSB0

sudo usermod -aG dialout $USER newgrp dialout

## Step 2

mkdir \~/esp cd \~/esp git clone -b v4.2.1 --recursive https://github.com/espressif/esp-idf.git

## Step 3 - Set up the tools

cd \~/esp/esp-idf ./install.sh

## Step 4

. $HOME/esp/esp-idf/export.sh

or add alias get-esp-idf='. $HOME/esp/esp-idf/export.sh' to `.bashrc`

## Step 5

mkdir \~/workspace/esp cd \~/workspace/esp cp -r $IDF\_PATH/examples/get-started/hello\_world .

## Step 6

cd \~/workspace/esp/hello\_world idf.py set-target esp32 idf.py menuconfig

idf.py build idf.py -p /dev/ttyUSB0 flash idf.py -p /dev/ttyUSB0 monitor idf.py -p /dev/ttyUSB0 flash monitor

Exit monitor: Ctrl+]

## Using

```
get-esp-idf
cd ~/workspace/esp
cp -r $IDF_PATH/examples/...project-name .
idf.py set-target esp32
idf.py menuconfig
idf.py build
idf.py -p /dev/ttyUSB0 flash
idf.py -p /dev/ttyUSB0 monitor
```

## Creating a new project

```
get_idf
idf.py create-project project0
```
