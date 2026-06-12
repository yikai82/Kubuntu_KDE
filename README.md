<p align="center">
  <img src="[insert XXX IMAGE URL]" alt="Image" width="120">
</p>

# 
<h1 align="center">My Linux Journey — Kubuntu + KDE</h1>
<h3 align="center">Since 2024</h3>

<p align="center">
  <!-- Enter Text <br> -->
  <a href="#">Acer Nitro 5 + Kubuntu 26.04</a> |
  <a href="#">MBP 2019 + Kubuntu-T2</a> |
  <a href="#">MBP 2011 + MX-23/25_KDE</a> |
  <!-- <a href="#other-references">References</a> -->
</p>

---
> [!NOTE]  
> 1. The guide here is based on my own machines and my own experiece with `Kubuntu-T2` and `Kubuntu 24.04 and 26.04`. Feel free to follow and tweak as much as you need. But, if you are using AI as an assistant during this journey, make sure you try at least two differnt AI to help you along the way or ask me any questions 🙋.
>  
>     - My experience with troubleshooting Linux with AI is 50/50. 
>     - **Highly recommended**: Install **`timeshift`** and save a snapshot of your root (/) system before applying critical or any updates. If something breaks after an update, you can restore your root (/) system to its state before the update was applied.
>

> [!WARMING]
> 1. If you are one of those people like to **keep things (system) up-to-date**, Linux might not be the best system for you. The reason is because Linux is mostly community driven so it might not have the same level of resources like Winodws and macOS to ensure almost nothing will break after applying the update. 
> 
> 2. Be exttremely careful when updating the system and avoid blindly updating everything at once` as it may break the system or cause minor issues. In particular, be cautious with full system upgrades such as `sudo apt upgrade` or `sudo apt full-upgrade`.   
>
> 3. Same rules for the removing package. Always run `apt rdepends --installed <package name>` to check which installed packages need it (package name).
> 
>     ```bash
>     ## example 
>     $ apt rdepends --installed libibus-1.0-5
>     # output from terminal: 
>     libibus-1.0-5
>     Reverse Depends:
>       Depends: plasma-desktop (>= 1.5.1)
>     # plasma-desktop requires libibus-1.0.-5, removing it might cause it break
>     ```
>     - Feel free to read MY ` Linux troubleshooting story with AI Part 1` [here](/Linux_Troubleshooting_Tales_with_AI/01_ibus_removal_story.md) to understand why I emphasize this. 



**Disclaimer**: *I have made every effort to ensure the accuracy of this document, but errors may still be present. Feel free to leave comments and I will address them. Thank you!*

---
## Content  
- [1. Notes and Important Concepts](#key-note-and-important-concept)  
- [2. Title](link)  
- [3. Title](link)  
- [4. Title](link)  
- [5. Title](link)  
- [6. Title](link)  
- [7. Title](link)  
- [8. Title](link)  


<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub> 

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
System: AN515-45-R4LC

<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>  

---  

<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>  

---
## Heading lv1

###  1. heading lv2


---
## Reference








---
## Color Hex Code

KDE Blue (primary): #1D99F3  
KDE Dark Blue: #1B89D0  
KDE Light Blue: #3DAEE9  
Neutral Gray (backgrounds): #232629  
Highlight Green (accent sometimes used): #27AE60  



