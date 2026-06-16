# Kubuntn 26.04 Application Setup

#### Lastest Update: **`2026-06-03`**

---
> [!NOTE]  
> This guide is mainly for installing the following package on an **Acer Nitro 5 AN515-45** (or a similar model) running **Kubuntu 26.04**.
>
>

<i><p align="left"><b>Disclaimer</b>: I have made every effort to ensure the accuracy of this document, but errors may still be present, and the system may break with a wrong code  
Feel free to leave any comments/thoughts. Thank you!<p></i>  

<br>  

---
## System

<div align="left">
  <div style="margin: 2px 0;">
    <img src="image/Linux.png" alt="Linux" width="50" style="vertical-align: middle; margin-right: 6px;">
    <span style="vertical-align: middle;">Ubuntu 26.04 LTS</span>
  </div>
  <div style="margin: 2px 0;">
    Codename: <img src="image/Resolute.png" alt="Resolute" width="70" style="vertical-align: middle; margin-right: 6px;">
    <span style="vertical-align: middle;"></span>
  </div>
</div>  

Kernel Version: **7.0.0-22-generic (64-bit)**  
KDE Plasma Version: 6.6.4  
KDE Frameworks Version: 6.24.0  
Qt Version: 6.10.2  
Graphics Platform: Wayland  
Processors: 16 × AMD Ryzen 7 5800H with Radeon Graphics    
Memory: 16 GiB of RAM (15.0 GiB usable)  
System: Acer Nitro AN515-45 

 <!-- sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>   -->

---
## Content

- System Monitoring
  - [glances](#glances)
  - [iftop, traceroute, nethogs](#iftop-traceroute-nethogs)
  - [smartmontools](#smartmontools)
  - [radeontop](#redeontop)

- Backup & Recovery
  - [rsync]
  - [Timeshift](#timeshift)
  - [Back In Time](#back-in-time)

- System Utilities, Benchmark
  - [GParted]()
  - [fio]
  - [MX-LIve USB Maker]
  - [rEFInd]
  - [Ventoy]

- Theme and Customization
  - [Kvantum theme]

- Internet & Media 
  - [Google Chrome](#google-chrome)
  - [tailscale] 
  - [Spotify]
  - [yt-dlp]


---
## System Monitoring

### glances 
- For system resource monitoring

  ```bash
  sudo apt upate
  sudo apt install glances

  # to run:
  glances
  ```

- Set glances as Autostart in Kubunt/KDE:
  - Create a script:
  
  ```bash
  touch ~/.config/autostart/start-glances

  ## use script editor like kate or VS code to add the following code 

  # wait 10-15 seconds until dbus and session is ready
  sleep 10

  konsole --profile "breeze"  -e bash -l -c "glances; exec bash"  # make terminal remain opened after exit.
  ```

  - **Option**: glances + logging

  ```bash
  sleep 15

  # capture the session name
  if [ "$XDG_SESSION_TYPE" = "wayland" ]; then
      SESSION_TYPE="wayland"
  else
      SESSION_TYPE="x11"
  fi

  konsole --profile "breeze" -e bash -l -c "glances --export csv --export-csv-file ~/logs/glances_${SESSION_TYPE}_$(date +%F_%H-%M).csv; exec bash"
  ```

- **Alternative**: `htop`


<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>   

---
### iftop, traceroute, nethogs
- For internet monitoring


- Installation: 
  ```bash
  sudo apt install
  sudo apt install iftop traceroute nethogs
  ```

- To run iftop
  ```bash
  sudo iftop -i wlp4s0 -n -N -m 2M

  # Options:
  # -B : show bytes instead of bits
  # -n : skip DNS lookup (faster, cleaner)
  # -N : show port numbers instead of service names
  # -m : -m <limit> sets the maximum scale for the graph display
  # It does NOT limit your network. For example:
  #    -m 2M : 2 Mbps max display range (~0.25 MB/s)
  ```
- To use traceroute
  ```bash
  traceroute google.com
  traceroute chatgpt.com
  ```

- Use `nethogs` to chech what application is talking to internt 
  ```bash
  sudo apt update
  sudo apt install nethogs
  sudo nethogs 
  ```

- 👉 Other commands for interent monitoring

  ```bash
  #  This shows what your computer is talking to:
  ss -tup

  ## Options: 
  # -t : TCP connections. Shows TCP sockets (web traffic, SSH, etc.)
  # -u : UDP connections. Shows UDP sockets (DNS, streaming, some games, etc.)
  # -p : process info. Shows which program owns the connection (PID + name)

  ## to use open-ssl 
  openssl s_client -connect chatgpt.com:443

  # or
  netstat -tup # need to install with sudo apt install net-tools
  ```


---
### smartmontools
- smartmontools is an open-source software suite that provides utilities for monitoring and controlling storage devices using the Self-Monitoring, Analysis and Reporting Technology (S.M.A.R.T.) system built into most modern hard drives and solid-state drives.

- Installation:  

  ```bash
  sudo apt update
  sudo apt install smartmontools
  ```
- How to use:

  ```bash
  lsblk -f # check the device id
  sudo smartctl -a /dev/sda  # check the status of deivce id = sda
  sudo smartctl -a /dev/sda > result.log # output as .log file
  sudo smartctl -t short /dev/sda  # run a short test
  ```

- External USB enclosures may block or partially hide SMART data, leading to missing or incomplete health information. 
  - Option: use the following commands to identify driver and bus information
  ```bash
  lsusb -t
  lsusb
  ``` 


  #### 💡 `lsusb -t` vs `lsusb`

| Aspect                                        | `lsusb`                                                           | `lsusb -t`                                                               |
| --------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Purpose                                       | Lists USB devices connected to the system.                        | Shows the USB device hierarchy (tree view) and connection details.       |
| View                                          | Flat list.                                                        | Parent-child tree structure.                                             |
| Device Identification                         | Shows Bus, Device number, Vendor ID, Product ID, and device name. | Shows port number, device number, class, driver, and link speed.         |
| Physical Topology                             | ❌ Not shown.                                                      | ✔ Shows which port and hub a device is connected through.                |
| Driver Information                            | ❌ Not shown.                                                      | ✔ Shows the kernel driver in use (e.g., `uas`, `usb-storage`, `usbhid`). |
| USB Speed                                     | ❌ Not shown.                                                      | ✔ Shows negotiated speed (e.g., `12M`, `480M`, `5000M`, `10000M`).       |
| Useful for Identifying Hardware               | ✔ Very good.                                                      | Limited. Device names are often omitted.                                 |
| Useful for Troubleshooting Drivers            | ❌ Limited.                                                        | ✔ Excellent.                                                             |
| Useful for Checking USB 2.0 vs 3.0 Connection | ❌ Not directly.                                                   | ✔ Easily visible from the speed column.                                  |
| Useful for Finding Vendor/Product ID          | ✔ Shows IDs such as `0bc2:2038`.                                  | ❌ Does not show Vendor/Product IDs.                                      |
| Typical Use Case                              | "What USB devices are connected?"                                 | "How are they connected and what driver/speed are they using?"           |



  - Try the following:

  ```bash
  sudo sudo smartctl -a /dev/sdb ## the sdb is an external USB enclosure
    
  ## example of the output from an external usb device  
  smartctl 7.5 2025-04-30 r5714 [x86_64-linux-7.0.0-22-generic] (local build)
  Copyright (C) 2002-25, Bruce Allen, Christian Franke, www.smartmontools.org

  Read Device Identity failed: scsi error unsupported field in scsi command

  If this is a USB connected device, look at the various --device=TYPE variants
  A mandatory SMART command failed: exiting. To continue, add one or more '-T permissive' options.

  ## Try the following
  ## /dev/sdX is the assumed usb external device, use lsblk -f to find the actual disk id 
  sudo smartctl --scan
  sudo smartctl -a -d sat -T permissive -H /dev/sdX
  sudo smartctl -a -d sat,auto -T permissive /dev/sdX
  ```


---
### redeontop
- Installation:
  ```bash
  sudo apt update
  sudo apt install radeontop
  ```
- To Run: `sudo radeontop`


<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>   


---
## Backup & Recovery

### rsync 
- 




### Timeshift
- A GUI for Linux system restore by backing up the root (/). Best for	system snapshots and easy rollback after bad updates.Not designed for personal files backup.

  ```bash
  sudo apt update
  sudo apt install timeshift
  ```

  
---
### Back In Time
- A GUI for home/data folder backup run with rsync-based backups. Focused mainly on user data.

- Installation:
```bash
sudo apt install backintime
```

---
### Syncthing

- Refer to the [hot standby workstation](/cheat_sheet/hot_standby_workstation.md)


---
### MX-Live USB Maker
- [Webpage](https://mxlinux.org/blog/live-usb-maker-tool-now-available-as-an-appimage/)
- [Download Link](https://github.com/MX-Linux/lum-qt-appimage/releases/download/24.6/MX_Live_USB_Maker-24.6.glibc2.28-x86_64.AppImage.zip)

- Download the archive file containing the AppImage, extract to a folder (e.g. /home/user/applications)

- To run:
  ```bash
  cd path/to/folder
  chmod +x /MX_Live_USB_Make****.AppImage
  sudo ./MX_Live_USB_Maker-24.6.glibc2.28-x86_64.AppImage
  ```

---
### Ventoy
- Ventoy is a **multiboot** USB tool that lets you boot multiple ISO files directly from a single USB drive without reformatting; simply copy ISO files to the USB and select them from the boot menu.

- [Download page](https://www.ventoy.net/en/download.html)

- To install: 
  ```bash
  ## Extract the tar.gz file
  tar -xzf ventoy-x.x.xx-linux.tar.gz
  cd ventoy-x.x.xx
  ```

- To use: 
  ```bash
  # Plug in your USB drive and check the device name:
  lsblk

  ## Example device: /dev/sdb
  ## ⚠️ Make sure it is the correct USB!

  ## Install Ventoy
  ## Replace sdX with your USB device (example: sdb).
  sudo ./Ventoy2Disk.sh -i /dev/sdX

  ## If reinstalling:
  sudo ./Ventoy2Disk.sh -I /dev/sdX
  ```




---
### Kvantum theme

- A nice collection of different color themes. It's useful for applying theme colors to the window titles of different applications. For example, *I use a brown title bar (**`KvBrown`**) for Kate* so it can be easily spotted when everything else is black (dark mode) these days.

- To install:
  ```bash
  apt search kvantum
  sudo apt install qt-style-kvantum qt-style-kvantum-themes
  ```



---

### Google Chrome
- [Download Link](https://www.google.com/aclk?sa=L&ai=DChsSEwiys-GU1-yUAxW0i8IIHR1EKiUYACICCAEQBBoCamY&ae=2&co=1&ase=2&gclid=Cj0KCQjwof_QBhCgARIsADaMzOeiuQdtpYkzCTtS9ShndFWt7KqF8WXWWdMLaqXZUtjOSP5cdbPiddwaAilOEALw_wcB&cid=CAASZuRoLWB3fVzW9IAEe6Ex0MqAjnq6aZKjhvVrLd3gHs8x2Ghh11WUiDLP3GBOGmfaY2cfFPip8i-ZUii_MKK9mQBlwXtlV2JFmoi9hI1pg-vHIvLxbSYetMyxV4Ot2W2ou2iB-xgscA&cce=2&category=acrcp_v1_71&sig=AOD64_0UoIWLTXrJ9ga1wgi7Mw_LwCb_qQ&q&nis=4&adurl&ved=2ahUKEwiuztuU1-yUAxXKiSsGHfq1GkMQqyQoAHoECA4QDw)  

- Installation:
  ```bash
  sudo dpkg -i google-chrome-stable_current_amd64.deb
  ## update the *.deb to the actual download package name.
  ```


---
### Tailscale
- Tailscale is a zero-config mesh VPN built on WireGuard. It creates encrypted peer-to-peer connections between your own devices — no firewall changes needed. Features include MagicDNS for hostname resolution, subnet routing for remote LAN access, and ACL policies for access control.

- Installation:
```bash
curl -fsSL https://tailscale.com/install.sh | sh

```




---
### Spotify

- Installation:
  ```bash
  flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
  flatpak update
  flatpak install flathub com.spotify.Client

  # option:
  flatpak remotes  # to check remote site
  flatpak list 
  flatpka list | grep spotify # to confirm if installed
  ```

- To Run:
  ```bash
  flatpak run com.spotify.Client
  ```


---
### yt-dlp
  - Create the virtual environment and activate it
  ```bash
  sudo apt update
  sudo apt install python3.14-venv
  python3 -m venv yt-dl-env
  source yt-dl-env/bin/activate
  ```
  - Install yt-dlp
  ```bash
  pip install --upgrade pip # upgrade pip
  pip install yt-dlp # install yt-dlp
  ```
