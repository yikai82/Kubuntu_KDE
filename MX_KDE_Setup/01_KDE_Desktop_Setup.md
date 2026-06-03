## Kubuntu (or MX_KDE) Desktop Environemnt Setup

Lastest Update: 2026-05-15

### 1. Appearance
- Global Theme: Breeze Dark
    - Application Style: [Breeze] (left as it)
    - Plasma Style:
        - [Breeze]: slightly more colorful
        - Oxygen: more black/white
    - Colors: [Breeze Dark] (left as it)
    - Windows Decorations: Breeze (or Oxygen) > Shadows > Medium, 55% > #55ff00.
    - Icon: Breeze Dark (left as it)
    - Splash Screen: MX-Dark

### 2. Workspace
  - General Behavior: Clicking files or folders: [Selects them]
      
### 3. Windows Management
  **Note**:   
  1. If left empty, it means that I left as [default values].
  2. If some options migh be not present in the newer Kubuntu 26/KDE 6

      ⚜ ⚜ ⚜ 

  - Windows Behavior
      - Focus: `Focus follows mouse`
      - Delay: `500 ms`
      - Focus stealing prevetion: `Low`
      
  - Decktop Effects
      - Background Contrast
      - [Screen Edge]
        - Activation delay: `150 ms`
        - Edge barrier: `150 px`
      - [Sliding Popups]
      - [Magic Lamp]
      - Dialog Parent: `Unchecked`/`[Checked]` --> It really depends; for an older system I will change it to uncheck.
      - Dim Inactive: set `[15-25]`
      - [Window Aperture]
      - Fade Desktop or `[Slide]`
      - Window Open/Close Animation: `[Scale]`
      - Screen Locking: After `[5 minute]`
      - Virtual Desktops: `[1 Row]`
    

  - Windows Rules: If you have a previous windows rule, you can import here. 


### 4. Shortcut
  **Note: Meta = Super Key = Win Key/Command Key**
  
  - Konsole: Ctrl+Alt(Opt)+T
  - Spectacle: **Meta+Shift+S**
  - Switch One Desktop Down:  
  - Switch One Desktop Up: 
  - Switch One Desktop to the Lefe: [**Ctrl+Meta+Left**]/[**Ctrl+Alt+Left**]
  - Switch One Desktop to the Right: [**Ctrl+Meta+Right**]/[**Ctrl+Alt+Right**]
  - Toggle Windows - All Desktop: **Meta + Tab**
  - Window One Destop to the Left (Move the window to the Left Desktop): **Meta+Left**
  - Window One Destop to the Right (Move the window to the Right Desktop): Meta+Right
  - Lock Screen: [Meta+L]


### 5. Startup and Shutdown
  - Login Screen (SDDM)

### 6. Notification:
  - Hide after: `[3 seconds]`
  - Uncheck: Keep popup open during progress
  - Check/Uncheck: Notification Badge

### 7. Display and Monitor
- Night Color
    - Day:`[4700K]`
    - NIght: `[3500K - 3700K]`
    - Begin at `[18:00]`
    - End at `[10:00]`


### 8. Input/Output 
  - Mouse & Touchpad > Screen Edges: [Activation delay] = `[125 ms]`
  - Sound > Configure Volumne Controls > Volume change step = `[2%]`


### Kate Color Profile 
  - TBA 
  
  
### KDE Wallet
  - A secure, integrated password management system for the KDE Plasma desktop environment that stores sensitive information like WiFi password, app credentials
  
  - Only need if you want system to manage your password, or you have a previous wallet that you want to bring it over.
  
  
  - Import previous KDE Wallet: 
  
    1. Copy the old wallet files:

        ```bash
        ~/.local/share/kwalletd/kdewallet.kwl
        ~/.local/share/kwalletd/kdewallet.salt
        ```

    2. Copy them into the same location on your new system

    3. Unlock it with the old password
    
    4. Restart KWallet: Log out/log back in or run:  

        ```bash
        kwalletd6 &
        ```  

  - If the above solution does not work, try the follow steps:
    
    - Enable KWallet + PAM integration: Some distros might only have KWalletManager and KWallet backend, not PAM. To install it, run:
      
      ```bash
      apt search kwallet | grep pam # check if have pam
      cat /etc/pam.d/sddm # looking for pam_kwallet.so
      sudo apt install kwalletmanager pam-kwallet
      
      sudo apt install libpam-kwallet5 # confirm the version is compatiable
      ```
    - Hook PAM with the login manager: 

      ```bash
      sudo nano /etc/pam.d/lightdm

      # Add these lines, Ctrl + X, Save buffer: Y
      auth    optional    pam_kwallet5.so
      session optional    pam_kwallet5.so auto_start
      ```

**Note**: MX-25.1 already has pam integration, only need to copy`kdewallet.kwl` and `kdewallet.salt` over to the default kwalletd location


### Dolphin
  - The Kubuntu's download folder is default grouped by date (i feel not as easy to use):

    - **To Fix:** 

      1) Right-click on the top toolbar.
      2) Select Configure Toolbars....Search for "`Show in Groups`" in the left column.
      3) Move it to the right column and click Apply.
      4) Click the button on your toolbar to toggle it off.
      5) Ensure "Remember display style for each folder" is checked.

