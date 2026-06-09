# Hot Standby System Setup


> [!NOTE]  
> 1. The easiest strategy is to test **`Syncthing`** first on the same local network to ensure it works properly. After that, install **`Tailscale`** to verify the connection when the two systems are on different internet providers.
>
>

> [!WARNING]  
> 
>
> 
> 

## Content

- [System](#system)
- [Hot Standby System Design](#--hot-standby-system-design)
- [Requirement](#requirement)
- [Setup and Configure Syncthing](#setup-and-configure-syncthing)
- [Setup Tailscale](#setup-tailscale)

---
## System 

<div align="left">
  <div style="margin: 2px 0;">
    <img src="image/Linux2.svg" alt="Linux" width="50" style="vertical-align: middle; margin-right: 6px;">
    <span style="vertical-align: middle;">Ubuntu 26.04 LTS</span>
  </div>
  <div style="margin: 2px 0;">
    Codename: <img src="image/Resolute.svg" alt="Resolute" width="70" style="vertical-align: middle; margin-right: 6px;">
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
System:  AN515-45-R4LC


---
## 🛜 💻 Hot Standby System Design 

- Primary laptop: active working machine
    - Acer Nitro 5 | AMD Ryzen 7 5800H | 16G RAM DDR4-3200 |
    - Kubuntu 26.04 LTS | 7.0.0-22-generic (64-bit)  
    - Role: Source of truth
    - **Mode: Send Only**

- Backup laptop:
    - MBP 2011 | Intel i7 2635 QM(Q1'11) | 16G RAM DDR3-1666 | 
    - MX-25_KDE | 
    - Emergency recovery system
    - Not normally edited unless Nitro fails  
    - **Mode: Receive Only**

- Goal: Mirroring all important files so that if the primary system goes down, the backup system can immediately serve as the primary work machine.

- If the primary laptop (Nitro) fails:
  1. Boot the backup laptop.
  2. Verify that the latest synced data exists.
  3. Continue working immediately — no immediate restore work is required. 💻🟢


---
## Requirement
- Syncthing: 
    - v1.29.5 "Gold Grasshopper" (go1.25.0 linux-amd64) debian@debia
    - **Install to both the primary and backup laptop.**
    - Installation:
    ```bash
    sudo apt update
    sudo apt install syncthing

    # confirm installation complete
    syncthing --version
    ```

- Tailscale: 
    - v1.98.4
    - Installation: Downalod from the official [website](https://tailscale.com/download) and follow the instruction to install it. 
    

---
## Setup and Configure Syncthing


1. After installing syncthing on both system, run the following command on both machines to manual start syncthing.

    ```bash
    syncthing
    ```

    It should automatically open a web UI: http://localhost:8384


2. Enable Auto-Start (Option or Later after testing period)

    ```bash
    # Check status:
    systemctl --user status syncthing  # it should says Loaded but inactive
    
    # Run on both machines 
    systemctl --user enable syncthing
    systemctl --user start syncthing
    ```
    📝 **Note**: Manually launch Syncthing after logging in gives real-time terminal logs, which are very useful for testing and troubleshooting. If Syncthing autostart is enabled, you can use the following commands to watch the logs:

      ```bash
      journalctl --user -u syncthing -f
      ```
    - Early design/testing phase: manually launch Syncthing. After a week or two of stable testing, transition to the `systemd` service and use `journalctl` for monitoring.


3. Connect the Two Devices
    - On either machine: 
      - Open Syncthing Web UI > Go to Actions > Show ID > Copy Device ID  
      - On other machine → Add Remote Device
      - Paste ID and name it (e.g., Nitro / Backup)
      - Accept connection on both sides


4. Create Sync Folder: Add `test_folder` to test 
    - On PRIMARY (Nitro):
      - Create test folder: 

      ```bash
      - mkdir /path/to/test_folder/test.txt 
      - echo "test" > /path/to/test_folder/test.txt
      ```

      - Back to Syncthing web UI: Click “Add Folder”, Enter the Folder Path and Folder Label, then click "Save' to exit the window. You should see "test_folder" is added to the Web UI.

      - ⚠️ **NOTE**: Always sync the real storage path.


5. Configure Folder Modes

    ⚠️ ⚠️ **First complete the setup for the test_folder on the host machine (Send Only) and repeat the similar step for the backup machine (Receive Only)**  

      - Click the test folder > Edit to enter the Edit menu and complete the rest of folder setting. 
      - You can refer to the offical Syncthing Documentation
      [Syncthing\Configuration](https://docs.syncthing.net/v1.29.3/intro/getting-started.html#configuring)
      - Accept Folder on the Backup machine and repeat the similar configuration step. 

6. Key Configuration Setting 
    - Primary (Nitro 5)
      - Folder Type: **Send Only**
    - Backup Laptop
      - Folder Type: **Receive Only**

    - #### 📌 Pull Order Comparision  
    <br>


      | Pull Order     | How it prioritizes files        | What you observe in practice                                       | Best use case                                                      |
      | -------------- | ------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
      | Alphabetical   | By file/path name (A–Z)         | Predictable sequence, no intelligence about size or importance     | General everyday syncing where predictability matters              |
      | Random         | No fixed pattern                | Files appear to sync in an unpredictable order                     | Large sync jobs where fairness matters and you want to avoid bias  |
      | Smallest first | Smaller files first             | Quick “progress feel”, small files complete fast, large files wait | Low bandwidth, laptops, hotspot/mobile setups, fast responsiveness |
      | Largest first  | Larger files first              | Big transfers start immediately, small files may wait              | Backup-heavy workflows or when large files are priority            |
      | Oldest first   | Based on modification timestamp | Older changes propagate before newer ones                          | Archival systems or workflows where chronology matters             |


7. Verify Sync Works
    - On Nitro: In terminal 
      ```bash
      echo "update 1" >> ~/SyncTest/file.txt
      ```
    - Check backup:
      - File should update automatically

8. Add the working folders that require continuous mirroring. 


## Setup Tailscale

Tailscale creates a secure, private network (WireGuard-based) that connects devices across different internet providers and networks. This allows the two laptops to communicate even when they're not on the same local network.

1. **Install Tailscale on Both Systems**

   Download and install from the official [Tailscale website](https://tailscale.com/download):
   
   ```bash
   # Ubuntu/Debian installation
   curl -fsSL https://tailscale.com/install.sh | sh
   
   # Verify installation
   tailscale --version
   ```

2. **Authenticate and Connect**

   On both machines, run:

   ```bash
   sudo tailscale up
   ```

   This will display a login URL. Open the URL in your browser and authenticate with your Tailscale account. After authentication, the device will be added to your Tailscale network.

   Verify connection:
   ```bash
   tailscale status
   ```

3. **Note Device IP Addresses**

   From the `tailscale status` output, note the Tailscale IP address (100.x.x.x) for each device:
   - Primary (Nitro): `100.x.x.x`
   - Backup Laptop: `100.y.y.y`

   These IPs will remain stable across network changes.

4. **Verify Tailscale Network Connectivity**

   Test ping between devices using their Tailscale IPs:

   ```bash
   # From Nitro to Backup
   ping 100.y.y.y
   
   # From Backup to Nitro
   ping 100.x.x.x
   ```

   Both should respond successfully.

5. **Enable Tailscale Auto-Start (Optional)**

   To start Tailscale automatically on boot:

   ```bash
   # Check status
   systemctl status tailscaled
   
   # Enable auto-start (already enabled by default in most installations)
   sudo systemctl enable tailscaled
   sudo systemctl start tailscaled
   ```

   📝 **Note**: Tailscale typically auto-starts after installation. To check logs:

   ```bash
   sudo journalctl -u tailscaled -f
   ```

6. **Use Tailscale with Syncthing (Optional Advanced Setup)**

   If you want Syncthing to communicate exclusively through Tailscale:

   - Access Syncthing web UI at: `http://100.x.x.x:8384` (using the Tailscale IP of the remote machine)
   - Configure firewall rules or Syncthing discovery settings to prioritize Tailscale addresses
   - This ensures sync works even when devices are on different internet providers

7. **Test Tailscale Connection Across Networks**

   - Move one laptop to a different network (mobile hotspot, different WiFi, etc.)
   - Verify that devices can still communicate:

   ```bash
   tailscale status
   ping <other_device_tailscale_ip>
   ```

   Both should work seamlessly, confirming the WireGuard tunnel is functioning.

---
## Reference

- [Tailscale Official Documentation](https://tailscale.com/kb/)
- [Tailscale Getting Started](https://tailscale.com/kb/1017/install)
- [Syncthing Official Documentation](https://docs.syncthing.net/)
- [Syncthing Configuration Guide](https://docs.syncthing.net/v1.29.3/intro/getting-started.html#configuring)

---
## Reference