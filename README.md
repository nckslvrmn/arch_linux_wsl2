# Arch Linux on WSL2
These are installation instructions for Arch Linux in WSL2 using the officially built bootstrap image.

## Requirements
1) [Install WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
2) [Update to the latest kernel](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
3) Set WSL2 as the default version `wsl --set-default-version 2` (version 2 is set by default in newer windows builds)

## Preparing the Image
In an existing linux install (another WSL distro, VM, or native), download the `archlinux-bootstrap-<version>-x86_64.tar.gz` bootstrap image from an [arch mirror](https://archlinux.org/download/) closest to you.

***AS ROOT*** via sudo or a root user, run the following commands:
1) Extract the downloaded image: `tar -zxvf archlinux-bootstrap-<version>-x86_64.tar.gz`
2) Navigate into the extracted directory: `cd root.x86_64`
3) Compress the contents of inside the directory: `tar -zcvf arch_bootstrap.tar.gz .`

The reason this must be done as root is to preserve file ownership of bootstrap filesystem when it is imported into WSL. If this is not done, you will likely experience permissions issues when interacting with imported the operating system.

Once the new archive is created, move the file to the machine Arch will be installed on (copy to usb drive, or move out of existing WSL).

## Importing the Image
On the machine Arch will be installed on:
1) Create a directory where the Arch virtual disk file will live, for example C:\Users\{USER}\wsl
2) Import the gzip archive into WSL, replacing the first path with the directory created in the previous, step and the second path with the path to the created archive from the previous section: `wsl --import Arch C:\Users\{USER}\wsl C:\path\to\arch_bootstrap.tar.gz`

## Configuring Arch in WSL2
Arch can now be launched on the machine it was installed on. Some additional configuration will be needed to make this a fully functional arch install:
1) Launch the installation: `wsl -d Arch`
2) Initialize the keyring required to run pacman: `pacman-key --init`
3) Fill the new keyring with Arch's latest set of keys: `pacman-key --populate archlinux`
4) Pacman's mirrorlist is already installed but entirelly commented out. Fetch a new one and overrite the existing one: `curl "https://archlinux.org/mirrorlist/?country=US&protocol=https&ip_version=4&use_mirror_status=on" | cut -c 2- > /etc/pacman.d/mirrorlist`
5) Update the repos and install the latest packages: `pacman -Syu`
6) There will also be a handful of missing "base" packages that are always useful to have and can be installed with: `pacman -S base base-devel`

## Next Steps
From here, you can continue setting up your new Arch Linux based WSL2 installation by following the [Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide#Configure_the_system). It is probably a good idea to set your locale, hostname, and to create yourself a new user to use. Keep in mind that you will not need to worry about your `fstab` or bootloader to use Arch in WSL. Installing an [aur helper](https://github.com/Morganamilo/paru) is also a good idea.

Additionally, if you use [Windows Terminal](https://github.com/microsoft/terminal) you can set Arch to launch with your new user in its home directory by adding `"commandline": "wsl ~ -u <username>"` to the profile in the profile list. Arch must be the default WSL installation for this to work (which can be done with `wsl --set-default Arch`.
