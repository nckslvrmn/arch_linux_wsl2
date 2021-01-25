# Arch Linux on WSL2
Installation instructions for Arch Linux in WSL2

## Requirements
1) [Install WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
2) [Update to the latest kernel](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi])
3) Set WSL2 as the default version `wsl --set-default-version 2`

## Preparing the Image
In an existing linux install (another WSL distro, VM, or native):
1) Download the `archlinux-bootstrap-<version>-x86_64.tar.gz` bootstrap image from an [arch mirror](https://archlinux.org/download/) closest to you
2) Extract the downloaded image: `tar -zxvf archlinux-bootstrap-<version>-x86_64.tar.gz`
3) Navigate into the extracted directory: `cd root.x86_64`
4) Compress the contents of inside the directory: `tar -zcvf arch_bootstrap.tar.gz .`
5) Move the newly created gzip file to the machine Arch will be installed on (copy to usb drive, or move out of existing WSL)

## Importing the Image
On the machine Arch will be installed on:
1) Create a directory where the Arch virtual disk file will live
2) Import the gzip archive into WSL: `wsl --import Arch <dir_path_from_step_1> arch_bootstrap.tar.gz`

## Configuring Arch in WSL2
Arch can now be launched on the machine it was installed on. Some additional configuration will be needed to make this a fully functional arch install:
1) Launch the installation: `wsl -d Arch`
2) Initialize the keyring required to run pacman: `pacman-key --init`
3) Fill the new keyring with Arch's latest set of keys: `pacman-key --populate archlinux`
4) Pacman's mirrorlist is already installed but entirelly commented out. Fetch a new one and overrite the existing one: `curl "https://archlinux.org/mirrorlist/?country=US&protocol=https&ip_version=4&use_mirror_status=on" | cut -c 2- > /etc/pacman.d/mirrorlist`
