Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.network "forwarded_port", guest: 22, host: 2222

  config.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 4
      vb.customize ["modifyvm", :id, "--usb", "on"]
      vb.customize ["modifyvm", :id, "--usbehci", "on"]
      vb.customize ["usbfilter", "add", "0",
        "--target", :id,
        "--name", "ESP",
        "--manufacturer", "Silicon Labs",
        "--product", "USB2.0-Serial"]
      vb.customize ["usbfilter", "add", "0",
        "--target", :id,
        "--name", "ESP",
        "--vendorid", "0x1a86",
        "--productid", "0x7523"]
      vb.customize ["usbfilter", "add", "0",
        "--target", :id,
        "--name", "ESP",
        "--vendorid", "0x10c4",
        "--productid", "0xea60"]
  end
  config.vm.provision "shell", inline: "apt-get update"
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo apt upgrade -y
    sudo apt install -y make unrar-free autoconf automake libtool gcc g++ gperf \
    flex bison texinfo gawk ncurses-dev libexpat-dev python-dev python python-serial \
    sed git unzip bash help2man wget bzip2 linux-image-extra-virtual libtool-bin inxi screenfetch

    cd ~/
    mkdir -p build
    cd build
    git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
    cd esp-open-sdk
    make -j8 toolchain esptool libhal STANDALONE=n
    cd ..

    echo 'export PATH=/home/vagrant/build/esp-open-sdk/xtensa-lx106-elf/bin:$PATH' >> ~/.bashrc

    # Set the ESP USB port and make sure the vagrant user can write to it
    echo 'export ESPPORT=/dev/ttyUSB0' >> ~/.bashrc
    sudo usermod -a -G dialout vagrant

    # Set the dout Mode for NodeMCU boards. If you dont have a NodeMCU board comment this line
    echo 'export FLASH_MODE=dout' >> ~/.bashrc

    # Download esp-open-rtos
    git clone --recursive https://github.com/Superhouse/esp-open-rtos.git
    echo 'export SDK_PATH=/home/vagrant/build/esp-open-rtos' >> ~/.bashrc

    sudo modprobe usbserial
    sudo modprobe cp210x

    git clone --recursive https://github.com/RavenSystem/esp-homekit-devices.git
    cd esp-homekit-devices
    git submodule update --init --recursive

    sudo curl -L https://raw.githubusercontent.com/jsmith79/8266-build-env/main/99-motd -o /etc/update-motd.d/99-motd
    sudo chmod +x /etc/update-motd.d/99-motd
    sudo reboot
    
  SHELL
end
