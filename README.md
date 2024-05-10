# Introduction

Vivado has a lot of dependencies that it seems to handle with the installer on windows that is simply doesn't on linux. Admittedly, this is somewhat self evident given the linux installation is anywhere from 20 to 30GBs less than the windows installation.

  

In this immediate example I will be showing steps for Vivado 2021.2 pulled from this reddit post [link](https://www.reddit.com/r/Xilinx/comments/s7lcgq/help_vivado_ml_20212_stuck_on_generating/). I have personally struggled for hours or even days with Vivado and have ended up back at this post multiple times so I thought it best to put it somewhere I'd actually remember.

  

# Vivado 2021.2 on Ubuntu 24.04 LTS

1. Download the [tar package](https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_Unified_2021.2_1021_0703.tar.gz) from the [Vivado download page](https://www.xilinx.com/support/download.html). It should be huge in comparison to the Self Extracting Web Installer.
2. Update cache.
```bash
sudo apt-get update && sudo apt-get upgrade -y
```

3. Install Java. I'd recommend using the default ubuntu options:

```bash
sudo apt-get install default-jre default-jdk
```

4. Install Python.
```bash
sudo apt-get install python3 python3-pip
```

5. Easy Dependencies. (libgtk2.0-0t64 will most likely be substituted for libgtk2.0-0)
```bash
sudo apt-get install -y libstdc++6
sudo apt-get install -y libgtk2.0-0
sudo apt-get install -y dpkg-dev
```

6. Hard Dependencies. I have found the only reliable way to get these on modern OS's is by downloading them directly from Ubuntu's package center. As no versions exist past Ubuntu 22 we will be using the jammy 22 LTS versions of the packages. [libtinfo5](https://packages.ubuntu.com/jammy/amd64/libtinfo5/download) & [libncurses5](https://packages.ubuntu.com/jammy/amd64/libncurses5/download)
```bash
wget http://security.ubuntu.com/ubuntu/pool/universe/n/ncurses/libtinfo5_6.3-2ubuntu0.1_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/universe/n/ncurses/libncurses5_6.3-2ubuntu0.1_amd64.deb
sudo dpkg -i libtinfo5_6.3-2ubuntu0.1_amd64.deb
sudo dpkg -i libncurses5_6.3-2ubuntu0.1_amd64.deb 
```
7. Restart your system.
```bash
sudo reboot
```
8. Untar the Vivado Package.
```bash
tar -xzvf Xilinx_Unified_2021.2_1021_0703.tar.gz
```
9. Find xsetup in the untarred folder. Replace the USER in this command with your username.
```bash
cd Xilinx_Unified_2021.2_1021_0703/
sudo ./xsetup --agree 3rdPartyEULA,XilinxEULA --batch Install --product "Vivado" --edition "Vivado ML Enterprise" --location "/home/USER"
```
If this fails, you will get this really annoying thing where vivado has already created a Program entry group which I still have not figured out how to remove. This will mean you'll have to redo the installation and give it a different name for the Program entry group. To do this just run:
```bash
sudo ./xsetup
```
The gui installer should open. 

10. Run settings64.sh
```bash
cd ~
cd Vivado/2021.2/
./settings64.sh
```
11. Install Cable Drivers
```bash
cd data/xicom/cable_drivers/lin64/install_scripts/install_drivers
sudo ./install_drivers
```


# Board Files
This is always a little different depending on the manufacturer of your board. I found this link years ago that explained it relatively well [link](https://tutorials.logictronix.com/our-resources/linux-for-fpga-design/how-to-install-vivado-on-ubuntu/). For Digilent eval/learning boards, this is one way:
1. Download the board package from the [git repo](https://github.com/Digilent/vivado-boards.git)
2. From the repo folder vivado-boards/new/board_files copy the board files.
3. Paste the board files in Vivado/2021.2/data/boards/
4. From vivado_boards/utility, copy the file Vivado_init.tcl
5. Paste the file to the Vivado installation. 
