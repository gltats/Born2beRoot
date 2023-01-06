# Born2beRoot
42 project that aims to introduce to the world of virtualization

## Part 1: Setting up a VM with partitions
Part 1.1 - First steps
- Open the VM and click on New
- Set Memory Size as 1024 MB and click continue.
- Click Create a Virtual Hard Disk Now and then click Create to create the Hard Disk.
- Click VDI (VirtualBox Disk Image) and then click Continue to select VDI.
- Click Dyamically Allocated and then click Continue to only use space on your Hard Disk.
- Set Size as 12.00 GB and then click Continue this should be enough for this project.
- Click Settings and then click Storage to view your Virtual Machine Storage.
- Click on Optical Drive (Optical Drive - far right blue small box).
- Click on Choose a disk file... (2nd option in the drop down).
- Then click on the Virtual Machine file (.iso).
- Click on your Virtual Machine and then click 'ok to confirm you Virtual Machine Storage.
- Click Start (The Green Arrow ➡️) to start your Virtual Machine.

Part 1.2 - Accessing Your Virtual Machine

```
⚠️ NOTE:
- In the Virtual Machine, there is no mouse, so its necessary to use keyboard (You wil use mostly tab, space, enter 🔼 🔽).
- To increase your Virtual Machine size, press command + c at the same time and then use your mouse to drag the screen to the size you wish.
- Use the arrow keys on your keyboard 🔼 🔽 and press Enter on Install.
```


- 1 Press enter on English - English or your language of preference.
- 2 Press enter the country your installing this Virtual Machine.
- 3 Press enter your keyboard of preference.
- 4 Create a Host Name as your login, with 42 at the end - write down your Host Name, as you will need this later on. <br><br>
     ⚠️ NOTE: Whenever you are told to create a password, use the same password as everything.
- 5 Leave Domain blank, press enter on Continue.
- 6 Create a Password for the Host Name and retype it.
- 7 Create a User Name without 42 at the end - write down your Host Name, as you will need this later on.
- 8 Create a Password for the User Name (you might as well use the same password as your Host Password).
- 9 Press enter on your Timezone.
- 10 Press enter on Guided - use entire disk and set up encrypted LVM (Second to last option from the list).
- 11 Press enter on Select Disk to Partition.
- 12 Press enter on Select Separate /home, /var, and /tmp paritions (Last option from the list).
- 13 Select Yes and press Enter to write the changes to disks and configure LVM.
- 14 Press Enter to cancel Erasing.
- 15 Create a Encryption passphrase.
- 16 Retype the Encryption passphrase you just created.
- 17 Type in ```max``` and press enter on Continue to assign the amount of volume group to use for guided partitioning.
- 18 Press enter on Finish partitioning and write changes to disk.
- 19 Press enter on Yes for Partition Disks.
- 20 Press enter on No for Configure the package manager.
- 21 Press enter in the country that your in.
- 22 Press enter on deb.debian.org.
- 23 Leave proxy information blank and press enter on continue.
- 24 Press enter on no for Configuring popularity-contest.
- 25 Deselect SSH server and standard system utilities by pressing the Space key and then Continue, (tab and enter).
- 26 Press enter on Yes to Install the GRUB boot loader on a hard disk.
- 27 Press enter on /dev/sda
- 28 Press enter on continue to finish the installation.

⚠️ ADVICE: Before we move onto starting your Virtual Machine, make sure you remember your Host, Username and Password/s.

Part 1.3 - Starting Your Virtual Machine
- Press enter on Debian GNU/Linux
- Enter your encryption password you had created before
- Login in as the your_username you had created before
- Type ```lsblk``` in your Virtual Machine to see the partition

## Part 2: Installing necesary tools
Part 2.1 - Installing Sudo
- First type ```su -``` to login in as the root user.
- Then type ```apt-get update -y```
- Then type ```apt-get upgrade -y```
- Then type ```apt install sudo```
- Then type ```usermod -aG sudo your_username``` to add user in the sudo group (To check if user is in sudo group, type getent group sudo)
- Type ```sudo visudo`` to open sudoers file
- Lastly find ```# User privilege specification```, type ```your_username  	ALL=(ALL) ALL```

Part 2.2 - Installing Git and Vim
- Then type ```apt-get install git -y``` to install Git
- Then type ```git --version``` to check the Git Version
- Then type ```apt install vim``` to install Vim
- Then type ```vim --version``` to check the Git Version

## Part 3: SSH and UWF
Part 3.1 - Installing and Configuring SSH (Secure Shell Host)
- Type ```sudo apt install openssh-server```
- Type ```sudo systemctl status ssh``` to check SSH Server Status
- Type ```sudo nano /etc/ssh/sshd_config```
- Find this line ```#Port22```
- Change the line to ```Port 4242``` without the # (Hash) in front of it
- Exit and save
- Then type ```sudo grep Port /etc/ssh/sshd_config``` to check if the port settings are right
- Lastly type ```sudo service ssh restart``` to restart the SSH Service

Part 3.2 - Installing and Configuring UFW (Uncomplicated Firewall)
First type apt-get install ufw to install UFW
Type ```sudo ufw enable``` to inable UFW
Type ```sudo ufw status numbered``` to check the status of UFW
Type sudo ```ufw allow ssh``` to configure the Rules
Type sudo ```ufw allow 4242``` to configure the Port Rules
Lastly Type ```sudo ufw status numbered``` to check the status of UFW 4242 Port

Part 3.3 Connecting to SSH
To exit your Virtual Machine and use your mouse, press command and click left and your mouse should appear
Go to your Virtual Box Program
Click on your Virtual Machine and select Settings
Click Network then Adapter 1 then Advanced and then click on Port Forwarding
Add or edit the rule 1, and set the ```Host Port``` and ```Guest Port``` to ```4242```(double check this step)
Then head back to your Virtual Machine
Type ```sudo systemctl restart ssh``` to restart your SSH Server
Type ```sudo service sshd status``` to check your SSH Status
Open an iTerm and type the following ```ssh your_username@127.0.0.1 -p 4242```
To quit your SSH iTerm Connection type ```exit```. Its possible and would say easier to continue doing this project on iTerm.

## Part 3: BASH
Create a monitoring.sh at /usr/local/bin

```
 #!/bin/bash
 architecture=$(uname -m)
 kernel=$(uname -r)
 physical_processors=$(grep -c ^processor /proc/cpuinfo)
 virtual_processors=$(nproc --all)
 ram_total=$(awk '/MemTotal/ {print $2}' /proc/meminfo)
 ram_free=$(awk '/MemFree/ {print $2}' /proc/meminfo)
 ram_utilization=$((100 - ram_free * 100 / ram_total))
 memory_total=$(awk '/MemTotal/ {print $2}' /proc/meminfo)
 memory_free=$(awk '/MemFree/ {print $2}' /proc/meminfo)
 memory_utilization=$((100 - memory_free * 100 / memory_total))
 processor_utilization=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')
 last_reboot=$(who -b | awk '{print $3, $4}')
 lvm_status=$(systemctl is-active lvm2-lvmetad.service)
 active_connections=$(ss -neopt state established | wc -l)
 users=$(who | wc -l)
 ipv4_address=$(hostname -I)
 mac_address=$(ip link show | grep link/ether | awk '{print $2}')
 sudo_commands=$(journalctl _COMM=sudo | grep COMMAND | wc -l)
 wall "
               #Operating system architecture: $architecture
               #Kernel version: $kernel
               #Number of physical processors: $physical_processors
               #Number of virtual processors: $virtual_processors
               #RAM utilization: $ram_utilization% ($ram_free/$ram_total)
               #Memory utilization: $memory_utilization% ($memory_free/$memory_total)
               #Processor utilization: $processor_utilization
               #Last reboot: $last_reboot
               #LVM Status: $lvm_status
               #Number of active connection: $active_connections
               #Number of users: $users
               #IPV & MAC adress: IP $ipv4_addres ($mac_address)
              #Commands executed with sudo: $sudo_commands "
```
## Part 4: Evaluation

## PD -> Please don't copy and paste, try to understand the whole process

<hr />
<img alt="html5" src="https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white" style="max-width: 100%;">



