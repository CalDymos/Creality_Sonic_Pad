# __Firmware Helper__

https://github.com/CrealityOfficial/Creality_Sonic_Pad_Firmware.git

# __Faq  Helper__

https://www.creality.com/pages/faq?spm=..page_2450731.header_1.1&spm_prev=..page_1934481.products_display_1.1

# __Os  Helper__

https://github.com/CrealityTech/sonic_pad_os

# __Tip for use:__

When the configuration file is copied to the USB stick and boot through the Sonic Screen custom model, the mcu connection serial address will be automatically changed to the corresponding USB interface mapping. If you manually overwrite the configuration file with the Fluidd configuration file option, you need to modify the mcu connection serial address format to: serial: /dev/serial/by-id/usb_serial_1 (if the machine is connected to USB port 2, then change it to serial: /dev/serial/by- id/usb_serial_2 and so on).
 
# __User defined firmware compilation guide__

Using Linux subsystem under Windows 10 64-bit
1. Download VirtualBox from www.VirtualBox.org and install VirtualBox
2. Download Ubuntu 18.04.LTS from https://releases.ubuntu.com/18.04/ubuntu-18.04.6-desktop-amd64.iso
3. Create a new virtual machine and install Ubuntu
   - First make sure that the hardware visualization is activated in the bios (VT-x, AMD-V, SVM, or Vanderpool)
   - Open VirtuslBox and create a ***New*** virtual machine
   - Under ***Name and Operating System***
     - Select a destination ***Folder***
     - Enter a ***Name***
     - Set ***Type*** to Linux
     - Set ***Version*** to Ubuntu（64-bit）
   - Under ***Hardware*** set the ***Base Memory*** to to 4096MB
   - Under ***Hard Disk***
     - Select ***Create a virtual Hard Disk Now***
     - Set Hard Disk Size to 25GB
     - Set ***Hard Disk File Type and Variant*** to VDI
     - Uncheck ***Pre-allocate Full Size*** for dynamic allocation.
   - click Finish
   -	Open Settings
     -	Under Settings ***General*** – Tab ***Advanced***
       -	Change ***Shared clipboard*** to Bidirectional
      	- Change ***Drag'n'Drop*** to Bidirectional
     - Under Settings ***System*** - Tab ***Motherboard***
       - Adjust the boot order to ***Optical - Hard Disk - Floppy***
     - Under Settings ***Storage***
       - Select ***Empty*** Item under ***Storage Devices / Controller:IDE***, then select under ***Attributes*** the Downloaded Ubuntu 18.04.6LTS image
         over the Disk Icon and over context menu ***Chose a Disk file***
       - Select ***Controller:SATA*** under ***Storage Devices*** and check ***Use Host I/0 Cache*** under ***Attributes***
     -	Under Settings ***Network*** - Tab ***Advanced***, Make sure the network is connected
     -	Under Settings ***User Interface*** - dropdown menu ***Devices***, make sure ***Insert Guest additions CD image*** is checked
     -	confirm all Settings and close Settings Dialog with ***OK***
     -	Start the virtual machine
     -	Install Ubuntu
4.	System upgrade and install git
   - Open ***Terminal***
   -	run command  ***sudo apt update*** （list all updatable software）
   -	run command  ***sudo apt upgrade***  （upgrade the package）
   -	Under menu ***Devices*** of the VM select  ***Insert Guest additions CD image*** and and perform the installation of VBox_GAs.
   -	run command  ***sudo apt-get install git*** （install git）
   -	restart the system
5.	Download Klipper firmware
   - Open ***Terminal***
   - run command ***cd ~*** (switch the working directory to the home directory)
   - run command ***git clone --recurse-submodules https://github.com/CrealityOfficial/Creality_Sonic_Pad_Project.git***
6.	Configure printer firmware
   -	Open the ***File Manager*** an go to folder ***/Creality_Sonic_Pad_Project/klipper/scripts***, click rigth mouse button in folder,	select ***open in terminal***
   -	run command ***ls*** (confirm that the content in the current directory is consistent with that in the git repository)
   -	run command ***./install-ubuntu-18.04.sh***（install ubuntu）
   -	run command ***cd ..*** (switch to the upper directory)
   -	run command ***ls*** (confirm that the content in the current directory is consistent with that in the git repository)
   -	run command ***make menuconfig*** (configure the printer motherboard)
   -	Select the Micro-controller Architecture, the Processor model, Bootloader Offset, Clock Reference, Communication interface etc. parameters
   -	After the parameter configuration is completed, press "Q" to save，Press "Y" to confirm
   -	run command ***make*** （generate firmware）
7.	Upgrade printer firmware via SD/TF card
   -	Create a new "ubuntu_sharing" on the computer disk
   -	Open VirtualBox--Settings--Shared Folders，Add a shared folder (tick auto mount)，Mount point type "/sharings"
   -	Copy the klipper.bin file just generated to the sharings folder
   -	Copy klipper.bin in the ubuntu_sharing folder of the computer disk to the TF/SD
8.	Upgrade printer firmware via USB  
     (If the motherboard has no Bootloader and does not support SD card upgrades)
     Please use a USB cable to connect the printer to the computer 
     where the virtual machine is located. Please keep the connection 
     during the firmware upgrade process
     -	Refer to the previous descrition to enter the configuration motherboardinterface,Select the Micro-controller Architecture, Processor model,Clock Reference,Communication interface etc. parameters
     -	After the parameter configuration is completed, press "Q" to save，Press "Y" to confirm
     -	run command ***make*** （generate firmware）
     -	Select in menu ***Device*** of the VM, under ***USB*** select the USB connection that is connected to the printer
     -	run command ***ls /dev/tty****  (Confirm that /dev/ttyUSB0 appears in the directory)
     -	run command ***sudo avrdude -cwiring -patmega2560 -P/dev/ttyUSB0 -b115200 -D -Uflash:w:out/klipper.elf.hex:i*** （Transfer the klipper firmware to the printer for upgrade，Note the space before the "-" in the command）
