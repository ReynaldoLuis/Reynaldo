# esp8266_tpm2net_ws2812
This is a custom firmware for the esp8266 wifi module that will drive a strand of ws2812 LEDs using the TPM2NET protocol. I was successful at driving 132 LEDs (11x12 matrix) using this firmware and the pixelcontroller (v2.1.0-RC1) software. I expect it to operate more LEDs (hopefully up to 512 LEDs) ... This is based on work by Charles (cnlohr) and Frans (Frans-Willem) 

#How to build a build environment.

#STEP 0: Environment
  I use Linux Mint and already had all the packages I needed installed.
  If you  run into commands that do not exist on your system, it should prompt
  you on how to get it.  If you fail in GCC, you may need libisl-dev,
  libcloog-isl-dev, amongst others.

#STEP 0.1: Alternatives.
The methods outlined in this document are an older method of installing everything.  You can check out
this site here: https://github.com/esp8266/esp8266-wiki/wiki/Toolchain to see if anything jives more
easily with your setup.

#STEP 1: Getting GCC and tools
  Get and build a copy of the xtensa build toolchain here:
  https://github.com/jcmvbkbc/xtensa-toolchain-build
   ... you're building the lx106 version.  You will also need the patched GCC here: https://github.com/jcmvbkbc/gcc-xtensa/commits/call0-4.8.2
  The commands you will need are as follows:
```
cd ~/esp8266
git clone https://github.com/jcmvbkbc/xtensa-toolchain-build.git
cd xtensa-toolchain-build
wget http://ftp.gnu.org/pub/gnu/binutils/binutils-2.24.tar.bz2
git clone --depth=1 https://github.com/jcmvbkbc/gcc-xtensa.git
mv gcc-xtensa gcc-4.9.1
wget http://ftp.gnu.org/gnu/gdb/gdb-7.6.tar.bz2
./prepare.sh lx106
./build-elf.sh lx106
```

#Step 2: ESPTool (for binaries)
You'll need a copy of esptool:
  download esp-0.0.2.zip from http://filez.zoobab.com/esp8266/esptool-0.0.2.zip
  I downloaded it to ~/esp8266/other/.
  You will need to build it, too.
```
cd ~/esp8266
mkdir other
cd other
wget http://filez.zoobab.com/esp8266/esptool-0.0.2.zip
unzip esptool-0.0.2.zip
cd esptool
### Edit Makefile to read on the first line: TARGET_ARCH	= LINUX
make
```
  Verify there are no errors.

#Step 3: esptool.py
You'll need to get a copy of esptool.py.  You may want to change:
```
ESP_ROM_BAUD    = 230400
```
  So you don't have to wait all day.
  You can get it from: https://github.com/themadinventor/esptool/.  Simply git clone.
```
cd ~/esp8266
git clone https://github.com/themadinventor/esptool.git
```
#Step 4: Get the SDK
I expect you get the esp_iot_sdk_v0.9.3. 
  esp_iot_sdk_v0.9.3_14_11_21.zip -> http://bbs.espressif.com/download/file.php?id=72
  esp_iot_sdk_v0.9.3_14_11_21_patch1.zip -> http://bbs.espressif.com/download/file.php?id=73
```
cd ~/esp8266
wget http://bbs.espressif.com/download/file.php?id=72 -O esp_iot_sdk_v0.9.3_14_11_21.zip
wget http://bbs.espressif.com/download/file.php?id=73 -O esp_iot_sdk_v0.9.3_14_11_21_patch1.zip
unzip esp_iot_sdk_v0.9.3_14_11_21.zip 
unzip -f esp_iot_sdk_v0.9.3_14_11_21_patch1.zip 
```

Note the "License" file in this folder, you can move it into esp_iot_sdk_v0.9.3 if you wish.


#Step 5: Configure and build.
Once you got all those four, check out a copy of ws2812esp8266.
```
cd ~/esp8266
git clone https://github.com/cnlohr/ws2812esp8266.git
```

Configure it in your makefile with the following:
```
GCC_FOLDER:=~/esp8266/xtensa-toolchain-build/build-lx106
ESPTOOL_PY:=~/esp8266/esptool/esptool.py
FW_TOOL:=~/esp8266/other/esptool/esptool
SDK:=~/esp8266/esp_iot_sdk_v0.9.3
```

