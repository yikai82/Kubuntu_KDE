## Cheat sheet for conda (miniforge) installatin with environment setup

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
System: Acer Nitro AN515-45  


---
### Requirements
1. `Miniforge3-Linux-x86_64.sh` or `Miniconda3-latest-Linux-x86_64.sh`
    - [Link to Miniforge](https://github.com/conda-forge/miniforge)
    - [Link to Miniconda](https://www.anaconda.com/docs/getting-started/miniconda/install/linux-install) 

2. enviroment.yml  => to setup the environments

3. Total time for completing these steps: **10-12 minutes**


### Steps

1. Fix the potential library issue: 5-7 minutes
    ```bash
    sudo apt update
    sudo apt-get install libpq-dev gcc
    ```

2. Download miniforge3 or miniconda: 5-10 minutes
    ```bash
    # miniforge
    curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
    
    # minoconda
    curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    ```

3. Install miniforge 3: 3-5 minutes
    ```bash
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
4. Create an conda environment: 3-5 minutes

    ```bash
    # check update for conda first 
    conda update -n base -c conda-forge conda  # 1-2 minutes
    
    # one-step installation
    conda env create -f ./00_env_config_latest/ai_deploy_2026-06.yml -vv  
    # -vv for detail verbose, 
    # you dont need to name the enviromet name as conda will just read the yml file
    
    # two-steps installtion 
    conda create -n deploying_ai python=3.11  # 1-2 minutes
    conda activate deploying_ai # activate deploying_ai enviroment

    # update the conda environment
    conda env update -f ./file_name_2026-02-11.yml
    ```
5. Test the environment  

    ```bash
    conda activate your_env` 
    python -c "import torch; print(torch.cuda.get_device_name(0))"  ## check if pytorch is avialable 
    


6. Clone an existiong enviroment as a backup in case something fails

    ```bash
    conda env list  # list all the current environment names
    conda create --name env_name_new --clone env_name_xx # clone an existing conda library

7.  Delete an unwanted environment

    ```bash
    conda remove -n env_name --all # remove all the package to make it clean
    ```





