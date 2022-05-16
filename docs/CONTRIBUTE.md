This repository is the deployed version of the main repository of Software Discovery Tool as a submodule and the data repository as a sub submodule.
Any contribution to SDT should be done in the main repository.

# How To Deploy
The whole SDT deployment is maintained by:
- [cicd.yml]() workflow
- [config_build.py]() to create new sections in the distro config file
- [package_build.py]() to update existing distro files to latest versions

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
          sudo chown -R apache:apache /opt/software-discovery-tool
          sudo -u apache /opt/software-discovery-tool/bin/config_build.py
          sudo apachectl restart
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
Bugs are given high priority since this is an active project used by users of Z Systems.

For deployment improvements, use this repository.
For new feature request to SDT, use the main repository.
For new data files request, use the data repository.

Appreciating all the contributions that you do for the Open Mainframe Project.
