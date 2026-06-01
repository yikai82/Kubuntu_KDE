# Kubuntu-T2 installation


## Background: 

The **MBP2019 16-inch** (i9 2.3 GHz 8C, 16GB RAM, AMD Radeon 5500M) cost CAD $3500 in Jan 2021. Unfortunately, it went down in 2024 December due to a logic board issue, and Apple quoted me $1500 + TAX for a logic board replacement. A local Apple-certified shop asked for about $1000. 
 
 I also have a 2011 MBP that is notorious for its AMD GPU failure that I fixed by modifying the NVRAM by permanently shutting off the discrete GPU and forcing the iGPU. Since the post mentioned Arch Linux, I guess it is possible to use software to address minor hardware glitch — I decided to give Linux-T2 a try.


Fortunately, I found the [t2linux wiki](https://wiki.t2linux.org/) webpage, which has a very detailed description on how to install Linux on a T2 Mac. Below, I document the steps on how I turned a broken 2019 MBP into a Linux AI/ML or Data Science Learning Machine. Honestly, the i9-9880H is still very powerful today in 2026. 


---
> [!NOTE]  
> If you are comfortable with Linux, feel free to jump to [installation roadmap](https://wiki.t2linux.org/roadmap/) and [Guide: Pre-installation](https://wiki.t2linux.org/guides/preinstall/) and follow the [distribution specific guide](https://wiki.t2linux.org/distributions/overview/). 
> 2. I am a big fan of Kubuntu (Ubuntu + KDE Plasma), so this guide is mainly for Kubuntu.  


> [!WARNING]  
> Once you have successfully installed Linux-T2, **avoid blindly updating everything at once as it may break the system or cause minor issues**. In particular, be cautious with full system upgrades such as `sudo apt upgrade` or `sudo apt full-upgrade`. A safer approach is to update them in batches and only update what is necessary. This is not only specific to Linux-T2, but generally applies to any Linux distribution. 


<i><h4 align="center">Disclaimer: I have made every effort to ensure the accuracy of this document, but errors may still be present, and the system may break.  
Feel free to leave any comments/thoughts. Thank you!</h4><p>  

<br>  

---
### My MacBook Pro 2019 Linux-T2

<div align="left">
  <div style="margin: 2px 0;">
    <img src="image/Linux2.svg" alt="Linux" width="50" style="vertical-align: middle; margin-right: 6px;">
    <span style="vertical-align: middle;">Kubuntu-T2 24.04.2 LTS</span>
  </div>
  <div style="margin: 2px 0;">
    <img src="image/Noble.svg" alt="Noble" width="50" style="vertical-align: middle; margin-right: 6px;">
    <span style="vertical-align: middle;">Codename: Noble</span>
  </div>
</div>  

Release:	24.04  
Kernel Version: **Linux 6.14.0-1-t2-noble**

 <!-- sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>   -->

---
## Installation Steps

### Main Step: 

1. Read the information from the [Installation Roadmap](https://wiki.t2linux.org/roadmap/)

    - Decide to keep macOS or not --> This will decide your [Wi-Fi method](#wi-fi).  
    - Partition the disk for Linux, select modern one partition or the tradition two partitions approach: root (/) and home (/home).
    - Decide what Linux distribution you want to use, Highly recommend selecting a popular one as there will be more community support.
    - Go to the **Guide** section and follow along.


2. **Pre-installation** steps: 

    - [Link to t2linux/Guide/Pre-install](https://wiki.t2linux.org/guides/preinstall/).
    - Download the iso files and follow distribution specific guide.
    - Below are the currently available installer ISOs for download:
    <br>  

    | Distribution     | Resource                 | Link                                                                                                                |
    | ---------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------- |
    | Arch Linux       | T2 Linux Arch ISO        | [Arch Linux T2 Linux ISO releases](https://github.com/t2linux/archiso-t2/releases/latest)    |
    | CachyOS          | Official site            | [CachyOS](https://cachyos.org/)                                                            |
    | EndeavourOS      | T2 Linux EndeavourOS ISO | [EndeavourOS T2 ISO releases](https://github.com/t2linux/EndeavourOS-ISO-t2/releases/latest) |
    | Fedora           | T2 Linux Fedora ISO      | [Fedora T2 ISO releases](https://github.com/t2linux/fedora-iso/releases/latest)              |
    | Gentoo           | Reference page           | Please refer to [HERE](https://wiki.t2linux.org/distributions/gentoo/installation/)        |
    | NixOS            | T2 Linux NixOS ISO       | [NixOS T2 ISO repository](https://github.com/t2linux/nixos-t2-iso)                           |
    | Ubuntu & flavors | T2 Ubuntu releases       | [T2 Ubuntu releases](https://github.com/t2linux/T2-Ubuntu/releases/latest)                   |
    | Linux Mint       | T2 Mint releases         | [T2 Mint releases](https://github.com/t2linux/T2-Mint/releases/latest)                       |
     <br>

3. Basic setup: ***I did not need to do any of these steps*** as this section is mainly for:

    - If you have installed Linux using an official ISO, instead of a T2 ISO.
    - For those who wish to encrypt their disk drives using LUKS or some other similar software.
    - If some functionality related to T2 Macs is broken
    - Installing kernel for T2 support:  


### Wi-Fi 

- Wi-Fi and Bluetooth: Use **Method 3**

    - [Guide: Wi-Fi and Bluetooth](https://wiki.t2linux.org/guides/wifi-bluetooth/) 
    - **Method 3: Create a Linux specific package which can be installed using a package manager**
    - On macOS: Click [here](https://wiki.t2linux.org/tools/firmware.sh) to download the script and run the `firmware.sh` command to obtain the firmware on the macOS terminal.

    ```bash
    bash path/to/script/firmware.sh
    ```

    - When prompted: "Method 1 / Method 2 / Method 3", just pick: Method 3. 

    - After you run the script, it will ask you to choose between 3 methods to move firmware to Linux. Return to the Linux and run the following command in the terminal (Konsole):

    ```bash    
    sudo apt install /path/to/firmware_package.deb
    ```
    - Confirm Wi-Fi is working 


### Final Check

- Check Kubuntu and its Kernel version by running the following command in Konsole/bash:

    ```bash
    # bash 
    {
    echo "=== Kubuntu Version ==="
    lsb_release -a
    echo -e "\n=== Current Kernel ==="
    uname -a
    echo -e "\n=== Installed Kernels ==="
    dpkg --list | grep linux-image
    } > ~/kubuntu-t2-status-$(date +%Y%m%d).txt
    ```

    This will save everything into a timestamped file: kubuntu-t2-status-yyyymmdd.txt

- Testing internet, adjust screen brightness etc.  


## Reference
-  [t2linux wiki](https://wiki.t2linux.org/)
