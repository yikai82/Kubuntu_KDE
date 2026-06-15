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
> 1. The guide here is based on my own machines and my own experience with `Kubuntu 24.04 and 26.04`, `Kubuntu-T2`, and `MX-Linux_KDE`. Feel free to follow alone and tweak as much as you need.  <br>  
> However, if you are using AI as an assistant during this journey, make sure you try at least two different AI chatbots to ensure their responses are similar enough, or you can also ask me here 🙋 or on popular forums like Super User.
>  
>     - My experience with troubleshooting Linux with AI is 50/50. 
> 
>     - **Highly recommended**: 
>       1. Install [**`timeshift`**]() and save a snapshot of your root (/) system before applying any updates. If something breaks after an update, you can restore your root (/) system to its state before the update was applied. It is recommended that you save snapshots on a different partition or event an external hard drive. 
>       2. If you think you are tech-savvy and comfortable enough, use the two-partition strategy: root(/) and home(/home) during the installation step. 
>
> 2. I have tried three different Linux distributions (mostly Kubuntu, Kubuntu-T2 or MX Linux_KDE ) on different machines. One thing I would like to emphasize: 
> 
> <h4 align="center"> your system dictates your choice of Linux distribution — the hardware decides the software. </h4>  
> <br>       
>         
> 3. When troubleshooting system issues by reading solutions on the internet, keep in mind that `a solution that works perfectly on one machine may cause issues on another machine, or even break the system if you are not careful`. Before you try anything, make sure you back up your data and your root (/) partition.
> 


> [!WARNING]
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

## System Requirement 

- MBP 2011  

- MNP 2019

- Acer Nitro 5 


---
<!-- ## Content  
- [1.]()  
- [2. Title](link)  
- [3. Title](link)  
- [4. Title](link)  
- [5. Title](link)  
- [6. Title](link)  
- [7. Title](link)  
- [8. Title](link)   -->

