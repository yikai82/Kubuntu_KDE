# Kubuntu 26.04 Work-Related App Installation


> [!NOTE]  
> This guide is mainly for installing the following package for my own work on an **Acer Nitro 5 AN515-45** (or a similar model) running **Kubuntu 26.04**.




> [!WARNING]  
> 1. **Error when installing miniforge/minocoda**: **`pg_configexecutable not found`** and/or `/tmp/pip-build-env-mdoyexlj/overlay/lib/python3.11/site-packages/setuptools/dist.py:765: SetuptoolsDeprecationWarning: License classifiers are deprecated`.
>
>     - **Possible Reason**: That error is pretty common when building Python packages that depend on PostgreSQL (like psycopg2). It simply means your system is missing the PostgreSQL development tools—specifically pg_config.
> 
>     - **To fix:**
>       ```bash
>       pg_config --version
>       ## If that command isn't found, you definitely need libpq-dev. 
> 
>       # to install: 
>       sudo apt update
>       sudo apt-get install libpq-dev gcc
>       ```
> 


<i><p align="left"><b>Disclaimer</b>: I have made every effort to ensure the accuracy of this document, but errors may still be present, and the system may break with a wrong code  
Feel free to leave any comments/thoughts. Thank you!<p></i>  

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

---
## Content
- [Git Credentail Manager](#1-git-credentail-manager-gcm)
- Miniconda/Miniforge
  - [Miniconda](#21-miniconda)
  - [MIniforge](#-22-option-miniforge)
- [VScode](#3-vscode)
- [LM Studio](#4-lm-studio)
- [GIMP](#)
- Infrastructure
- [Docker Engine](#)
- [Draw.io](#)

---
### 1. Git Credentail Manager (GCM)
  - Link: https://github.com/git-ecosystem/git-credential-manager/releases
  - I have been using [v2.4.1](https://github.com/git-ecosystem/git-credential-manager/releases/tag/v2.4.1) in the past
  - After intallation run `git credential-manager --version` to confirm 

---
### 2. Miniconda or Miniforge3

#### Miniconda vs Miniforge


| | Miniconda | Miniforge |
|-------|-------|-------|
| Maintainer | Anaconda, Inc. | Community (conda-forge) |
| Default channel | `defaults` (Anaconda) | `conda-forge` |
| Commercial use | Restricted (large orgs) | Free |
| Mamba included | No (conda only) | Yes |
| Solver speed | Slower (classic) | Faster (libmamba default) |
| License risk | Yes, for orgs >200 | None |  


👉 Dont want to read? Go to the [cheatsheet](/cheat_sheet/cheat_sheet_miniforge.md)!  

### 2.1 Miniconda   
- Link: https://www.anaconda.com/docs/getting-started/miniconda/install/linux-install

- Installation: 

  ```bash
  curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

  bash ./Miniconda3-latest-Linux-x86_64.sh
  
  ## NOTE: During the installation, you will see the following question from terminal: 
  ## installation finished.
  ## Do you wish to update your shell profile to automatically initialize conda?
  ## This will activate conda on startup and change the command prompt when activated.

  ## >> Enter [yes] here if it is the **first time** you setup conda (can be miniconda or miniforge) so it will add the startup, 
  
  ## >> Enter [no] if you have install different conda or your prefer do it manually. 

  Proceed with initialization? [yes|no] 
  [no] >>> yes
  
  ```

  - Typed [yes] to initialize conda after initallation or initialize manually later.
  - Confirm conda installation by running:

  ```bash
  conda --version # return 26.x.x
  ```

- **Conda initialization manually

  1. Add $PATH by run:
  ```bash
  sudo nano ~/.bashrc
  # add the following command to the bottom
  # Add new $PATH
  export PATH="$HOME/miniconda3/bin:$PATH"
  # Ctrl+X, Yes to Write Buffer
  source ~/.bashrc
  ```
  2. Initialize conda by running:
  ```bash
  conda --version
  conda init bash
  ```

  3. Disable auto activate conda base:
  ```bash
  conda config --set auto_activate_base false
  ```

- To uninstall Miniconda, run:
  ```bash
  ~/miniconda3/uninstall.sh
  ```

- Install specific conda environment: **deploying_ai**
  ```bash 
  ### Creata a specific environment: deploying ai
  
  # delete unwant env
  conda remove -n deploying_ai --all # remove all the package to make it clean

  # create a new conda env
  conda create -n deploying_ai python=3.11
  conda activate deploying_ai # activate deploying_ai enviroment

  # if require update conda
  conda update -n base -c conda-forge conda

  # update the conda environment
  conda env update -f ./file_name_2026-xx-xx.yml # update file name
  # deploying_ai_2026-02-10_new.yml # much lighter
  # deploying_ai_2026-02-11.yml  # much heavy 
  ```

<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>  

---    
### 👉 2.2 Option: Miniforge
- **Major benefit: Better solver speed**  

- Link: https://github.com/conda-forge/miniforge  

- Installation: 

  ```bash
  # download the package
  curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
  
  # install miniforge
  bash Miniforge3-Linux-x86_64.sh

  ## NOTE: During the installation, you will see the following question from terminal: 
  ## installation finished.
  ## Do you wish to update your shell profile to automatically initialize conda?
  ## This will activate conda on startup and change the command prompt when activated.

  ## >> Enter [yes] here if it is the **first time** you setup conda (can be miniconda or miniforge) so it will add the startup, 
  
  ## >> Enter [no] if you have installed different conda or your prefer do it manually. 

  Proceed with initialization? [yes|no] 
  [no] >>> yes

  ## Close and Open the shell
  ## If you'd prefer that conda's base environment not be activated on startup, run the following command when conda is activated:
  
  conda config --set auto_activate_base false
  ## Close and Open the shell again
  ```

- If you have installed different conda previously:

  ```bash
  which conda ## check which conda you are using

  # initiate conda
  cd ~/miniforge3/bin/  # navigate to bin folder under miniforge3 
  conda init bash       # re-point conda to "/home/Yi-Kai/miniforge3/bin/conda" if you have previous install minoconda
  ```

- Install conda python environment for your app:  
   
  - 💡 All roads lead to Rome—there are different ways to do it. The most efficient approach is to create it directly from a YAML file in a single step. Alternatively, you can create the Python environment first and then add the packages you need later.
    
  - Single-Step vs Two-Step:   

  | Method | Pros | Cons |
  | --- | --- | --- |
  | **Create directly from a YAML file** | - Reproducible and consistent across machines <br> - Captures exact dependencies and versions<br>- Faster for onboarding or reuse | - Requires maintaining the YAML file <br> - Can become outdated if changes are made outside the file<br>- Dependency conflicts in the file may be harder to troubleshoot |    
  | **Create the environment first, then install packages manually** | - More flexible during experimentation <br> - Easier to add or remove packages incrementally <br> - Good for learning and troubleshooting dependency issues | - Harder to reproduce later <br> - Team members may end up with slightly different environments <br>


  ```bash
  # check update for conda first 
  conda update -n base -c conda-forge conda  # 1-2 minutes

  # OPTION 1: one-step installation
  conda env create -f ./00_env_config_latest/ai_deploy_2026-06.yml -vv  
  # -vv for detail verbose, 
  # you dont need to name the enviromet name as conda will just read the 'name: xxxx' section in the YML file

  # OPTION 2: two-steps installtion 
  conda create -n your_env_name python=3.12  # <-- change the python version if needed, 1-2 minutes
  conda activate your_env_name # activate deploying_ai enviroment

  # update the conda environment
  conda env update -f ./file_name.yml
  ```
- Remove a conda python environemnt from your system:

  ```bash
  conda remove --name your_environment --all 
  # OR 
  conda remove --n your_environment --all
  ```

- To `clone` a conda environment: good way to create a backup 

  ```bash
  conda create --name new_env_name --clone old_env_name
  ```



- **Uninstall Miniforge**
  - 🛑 ⚠️ Carefully proceed the following step one-by-one. If you have any doubts, confirm with the original [source](https://github.com/conda-forge/miniforge#uninstall).
  
  - First, remove any modifications to your shell rc files that were made by Miniforge:  

  ```bash
  # Use this first command to see what rc files will be updated
  conda init --reverse --dry-run
  # Use this next command to take action on the rc files listed above
  conda init --reverse
  # Temporarily IGNORE the shell message: 
  #       'For changes to take effect, close and re-open your current shell.',
  # and CLOSE THE SHELL ONLY AFTER the 3rd step below is completed.
  ```
  
  - Second, remove the folder and all subfolders where the base environment for Miniforge was installed:  

  ```bash
  CONDA_BASE_ENVIRONMENT="$(conda info --base)"
  echo The next command will delete all files in "${CONDA_BASE_ENVIRONMENT}"
  # Warning, the rm command below is irreversible!
  # check the output of the echo command above
  # To make sure you are deleting the correct directory
  rm -rf "${CONDA_BASE_ENVIRONMENT}"
  ```
  - Thrid, remove any global conda configuration files that are left behind.
  
  ```bash
  echo ${HOME}/.condarc will be removed if it exists
  rm -f "${HOME}/.condarc"

  echo ${HOME}/.conda and underlying files will be removed if they exist.
  rm -fr "${HOME}/.conda"
  ```
  - Last, manual clean up the .bashrc related to the mamba
  ```bash
  nano ~/.bashrc

  ## you delete the following as the miniforge directory as been removed ## >>>>

  # >>> mamba initialize >>>
  # !! Contents within this block are managed by 'mamba shell init' !!
  export MAMBA_EXE='/home/yikai/miniforge3/bin/mamba';
  export MAMBA_ROOT_PREFIX='/home/yikai/miniforge3';
  __mamba_setup="$("$MAMBA_EXE" shell hook --shell bash --root-prefix "$MAMBA_ROOT_PREFIX" 2> /dev/null)"
  if [ $? -eq 0 ]; then
      eval "$__mamba_setup"
  else
      alias mamba="$MAMBA_EXE"  # Fallback on help from mamba activate
  fi
  unset __mamba_setup
  # <<< mamba initialize <<<
  ``` 

<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>  

---  
### 3. VScode 
- Download .deb version from the official [VS Code website](https://code.visualstudio.com/) 
- Installation: `sudo dpkg -i code_x.xxx.x-xxxxxxxxxx_amd64.deb`



---
### 4. LM Studio
- Two version: `deb` vs `appimage`

#### LM Studio: AppImage vs DEB Package Comparison

| Feature | AppImage | DEB Package |
|---|---|---|
| **Installation** | No installation required; download, `chmod +x`, and run | Installed via `sudo dpkg -i` or `apt install` |
| **Root Required** | No | Yes |
| **System Integration** | Limited by default; manual desktop entry needed | Full integration with menus, launchers, and package management |
| **Dependencies** | Bundled with the application; no external deps needed | Uses system libraries; possible conflicts after OS upgrades |
| **Compatibility** | Very high; works on most Linux distros | Primarily Debian/Ubuntu-based systems only |
| **Updates** | Manual re-download and replacement | Via `apt upgrade` if repository is configured |
| **Multiple Versions** | Easy to keep several versions side-by-side | Difficult; package manager typically allows only one version |
| **Disk Usage** | Larger (bundles all libraries) | Smaller (shares system libraries) |
| **Portability** | Can be copied and run on any compatible system | Must be reinstalled on each system |
| **Removal** | Just delete the AppImage file | Clean removal via `apt remove` |
| **Rollback** | Simple; just keep older AppImage files | Requires uninstall + reinstall of a specific version |
| **System Cleanliness** | Leaves few traces outside user config dirs | Installs files into standard system locations |
| **FUSE Dependency** | Requires `libfuse2` (Ubuntu 22.04+); workaround: `--appimage-extract-and-run` | Not required |
| **Sandbox Issues** | Possible in containerized envs; fix: `--no-sandbox` | Rare |
| **Security Updates for Libraries** | Only when the app itself updates | Benefits from system-wide library security updates |
| **Best For** | Portability, testing, version pinning, non-Debian distros, no-root envs | Users wanting traditional package management and full system integration |  
<br>

**Summary**: `AppImage` wins on portability, flexibility, and self-containment; `.deb` wins on system integration and security patching. For testing different LLMs, use `AppImage`; use `.deb` if you want it fully integrated for larger-scale work. 

**Verdict**: Install **`AppImage`**

<br>  

- **Fix FUSE error**: Ubuntu 22.04+ ships with FUSE 3 by default, so libfuse2 must be installed separately: 
  ```bash
  sudo apt update
  sudo apt install libfuse2
  ```


- Download the AappImage file from [here](https://lmstudio.ai/download) as my goal is to test different LLMs.

- To run: 
  ```bash
  cd path/to/the/folder  # where AppImage will be kepted, normally we created an applications for it.

  chmod +x LM-Studio-x.x.xx-x-x64.appimage
  ./LM-Studio-x.x.xx-x-x64.appimage
  ```


- Add to App Menu
    ```bash
    ls -d ~/.local/share/applications  # check if you already have applications folder under .local/share. If no, create one use the command below.

    mkdir -p ~/.local/share/applications  
    
    # create a create a desktop entry to integrate LM Studio with your system's application menu 
    cat > ~/.local/share/applications/lmstudio.desktop << EOF
    [Desktop Entry]
    Name=LM Studio
    Exec=/path/to/LMStudio.AppImage
    Icon=lmstudio
    Type=Application
    Categories=Development;
    EOF
  ```

<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>  

---
### GIMP





---
### Draw.io




#### Draw.io AppImage vs deb

| Feature | AppImage | .deb |
|---|---|---|
| **Installation** | None — just download, `chmod +x`, run | Installed via `dpkg` / `apt` |
| **System integration** | Minimal (no app menu entry by default) | Full — appears in app menu, `/usr/bin/drawio` in PATH |
| **System changes** | None — completely self-contained | Writes to system directories |
| **Updates** | Manual — download new file each time | Can use `apt` tooling |
| **Portability** | High — single file, move it anywhere | Tied to the system |
| **Command-line export** | Works but path is manual | Clean `drawio` command from anywhere |
| **Distro compatibility** | Runs on most Linux distros — Arch, Debian, Fedora, Ubuntu, openSUSE, etc. | Best suited for Debian/Ubuntu-based distros |


















---
### 5. R and RStudio (2025.09.2-418-amd64.deb): 
  ```bash
  sudo apt update
  sudo apt install r-base
  # go to RStudio https://posit.co/download/rstudio-desktop and download the package for your Linux System
  sudo dpkg -i /path/to/packages/rstudio-yyyy.mm.d-xxx-amd64.deb 
  ## you might encouter error like " studio depends on libssl-dev; however: Package libssl-dev is not installed", "rstudio depends on libclang-dev; however: Package libclang-dev is not installed."

  ## fix with: 
  sudo apt --fix-broken install
  ## then re-run: 
  sudo dpkg -i /path/to/packages/rstudio-yyyy.mm.d-xxx-amd64.deb
  ```


<sub>[↥ back to top](#content)&emsp;|&emsp;[Return Main Page 🏠](/README.md) </sub>  

---
###


| Feature | AppImage | .deb |
|---|---|---|
| **Installation** | None — just download, `chmod +x`, run | Installed via `dpkg` / `apt` |
| **System integration** | Minimal (no app menu entry by default) | Full — appears in app menu, `/usr/bin/drawio` in PATH |
| **System changes** | None — completely self-contained | Writes to system directories |
| **Updates** | Manual — download new file each time | Can use `apt` tooling |
| **Portability** | High — single file, move it anywhere | Tied to the system |
| **Command-line export** | Works but path is manual | Clean `drawio` command from anywhere |
| **Distro compatibility** | Runs on most Linux distros — Arch, Debian, Fedora, Ubuntu, openSUSE, etc. | Best suited for Debian/Ubuntu-based distros |















