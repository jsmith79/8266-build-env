# 8266-build-env
Vagrantfile and instructions to setup build env for building haa

# Software to install

Download and install Vagrant for your OS from https://www.vagrantup.com/downloads

Download and install Virtualbox from https://www.virtualbox.org/wiki/Downloads

After installing Virtualbox download the extension pack and install from https://www.virtualbox.org/wiki/Downloads

Latest Windows Installer
https://download.virtualbox.org/virtualbox/6.1.16/VirtualBox-6.1.16-140961-Win.exe

Latest MacOS installer
https://download.virtualbox.org/virtualbox/6.1.16/VirtualBox-6.1.16-140961-OSX.dmg

Latest Extension pack installer
https://download.virtualbox.org/virtualbox/6.1.16/Oracle_VM_VirtualBox_Extension_Pack-6.1.16.vbox-extpack

Visual Code - optional
https://code.visualstudio.com/download
Vagrant stores the key needed to ssh in the following subfolder from your Vagrantfile ".vagrant/machines/default/virtualbox/private_key"



# Setup instructions

Create a new folder somewhere and download "Vagrantfile" and save into that folder.
For example i will a folder named HAA in my home folder.
    
    mkdir ~/HAA

Change directory to the created folder.
    
    cd ~/HAA
    
Download "Vagrantfile" and save into that folder
    
    curl -L https://raw.githubusercontent.com/jsmith79/8266-build-env/main/Vagrantfile -o Vagrantfile

Type "vagrant up" and press enter
    
    vagrant up
    

This will download Ubuntu and the source code from the following repositories.
https://github.com/RavenSystem/esp-homekit-devices

https://github.com/pfalcon/esp-open-sdk

https://github.com/SuperHouse/esp-open-rtos

This will take a while, About 25 min for me.

Once complete you should be able to type "vagrant ssh" and press enter and you will ssh into the created virtual machine.
    
    vagrant ssh

Source files for HAA are located in ~/build/esp-homekit-devices/ 
    
    cd ~/build/esp-homekit-devices/

You should now be able to follow the build instructions at https://github.com/RavenSystem/esp-homekit-devices/wiki/Build-Instructions

Build:
    
    make -j8 -C devices/HAA all
    
Upload firmware to ESP and then run it:
    
    make -j8 -C devices/HAA test
    
Flash firmware:
    
    make -j8 -C devices/HAA flash
    
Show UART output of connected device (Same as capture log):
    
    make -j8 -C devices/HAA monitor
    
Clean and build from scratch:
    
    make -j8 -C devices/HAA rebuild
    
Erase Flash:
    
    make -j8 -C devices/HAA erase_flash

