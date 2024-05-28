# Bootstrapping the Software Discovery Tool Deployment Repository
## Introduction
This guide will walk you through the process of setting up the Software Discovery Tool deployment repository from scratch on an Ubuntu system. We'll use submodules to include the main repository and the data repository within the deployment repository.

## What are Submodules?
Submodules in Git allow you to include other Git repositories as subdirectories of your own repository. In our case, we use submodules to include the main Software Discovery Tool repository and the data repository within the deployment repository.

# Step-by-Step Instructions
1. Install Ubuntu:
Follow the installation instructions for Ubuntu on your virtual machine (VM) [Link](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview)

2. Clone the Deployment Repository:
Use the following command to clone the deployment repository and its submodules:
```sudo git clone --recurse-submodules https://github.com/openmainframeproject/software-discovery-tool-deploy.git /opt/software-discovery-tool-deploy```
This command will clone the deployment repository and automatically initialize and update all submodules.

3. Create Symbolic Link:
Create a symbolic link to the Software Discovery Tool directory:
```sudo ln -s /opt/software-discovery-tool-deploy/software-discovery-tool/ /opt/software-discovery-tool
```
This command creates a symbolic link named `/opt/software-discovery-tool` pointing to the `software-discovery-tool` directory within the deployment repository.

# Directory Structure
After completing the above steps, your directory structure should resemble the following:
```
/opt/
├── software-discovery-tool-deploy/   # Deployment repository
│   ├── software-discovery-tool/      # Main repository (submodule)
│   └── data/                          # Data repository (submodule)
└── software-discovery-tool/          # Symbolic link to the Software Discovery Tool directory
```


# How To Deploy
The whole SDT deployment is maintained by:
- [cicd.yml](https://github.com/rachejazz/software-discovery-tool-deploy/blob/main/.github/workflows/cicd.yml) workflow
- [config_build.py](https://github.com/openmainframeproject/software-discovery-tool/blob/master/bin/config_build.py) to create new sections in the distro config file
- [package_build.py](https://github.com/openmainframeproject/software-discovery-tool/blob/master/bin/package_build.py) to update existing distro files to latest versions

# Contribute to the deployment workflow

## Contribute to deployment script
The deployment uses [appleboy's ssh workflow](https://github.com/appleboy/ssh-action)
The deployment is thus triggered at every new pusb into the main branch.
The script section uses the required server credentials to be able to host the tool.
```yml
script: |
          cd /opt/software-discovery-tool-deploy
          sudo chown -R sdtuser .
          git checkout main
          git pull origin main --recurse-submodules
          sudo cp -u -r software-discovery-tool/ ../software-discovery-tool/
          [cicd.yml](https://github.com/rachejazz/software-discovery-tool-deploy/blob/main/.github/workflows/cicd.yml)
```
It includes 
- Pulling the latest changes
- Copies the contents to the main directory
- Uses config_build.py to check for new data files
- Uses package_build.py to update the existing files with the recent pkg versions
- Restart the server

## Contribute to package_build.py
The binary has separate methods to extract data from Linux Distributions and [PDS]().
Check [SDT-data]() repo to find out the distro list we support.

## Contribute to config_build.py
This file controls the `supported_distro.py` file that resides inside `src/config` directory.
It checks for any new data file inside `distro_files` directory and adds it's mapping to the `supported_distro.py`.
Any existing data file from PDS is updated from upstream.


_NOTE_
Any contribution to the binary should be done in the main repository.

# Report bugs and issues
Feel free to add new issues or features that you would like SDT to support.
Bugs are given high priority since this is an active project used by users of IBM zSystems and LinuxONE .

For deployment improvements, use this repository.
For new feature request to SDT, use the [main repository](https://github.com/openmainframeproject/software-discovery-tool).
For new data files request, use the [data repository](https://github.com/openmainframeproject/software-discovery-tool-data).

Appreciating all the contributions that you do for the Open Mainframe Project.
