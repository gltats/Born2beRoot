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
⚠️ NOTE:
```
- In the Virtual Machine, there is no mouse, so its necessary to use keyboard (You wil use mostly tab, space, enter 🔼 🔽).
- To increase your Virtual Machine size, press command + c at the same time and then use your mouse to drag the screen to the size you wish.
- Use the arrow keys on your keyboard 🔼 🔽 and press Enter on Install.
```
⚠️ ADVICE:  ```Whenever you are told to create a password, use the same password as everything.```
- 1 Press enter on English - English or your language of preference.
- 2 Press enter the country your installing this Virtual Machine.
- 3 Press enter your keyboard of preference.
- 4 Create a Host Name as your login, with 42 at the end - write down your Host Name, as you will need this later on. <br><br>
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

⚠️ ADVICE: ```Before we move onto starting your Virtual Machine, make sure you remember your Host, Username and Password/s.```

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

## Part 4: Password
- First type ```sudo apt-get install libpam-pwquality``` to install Password Quality Checking Library
- Then type ``sudo nano /etc/pam.d/common-password`` (You can also use vim)
- Find this line. ```password		requisite```
- Add this to the end of that line `minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root`
- The line should now look like this ``- password  requisite     pam_pwquality.so  retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root``
- Save and Exit
- Next type in your Virtual Machine ```sudo vim /etc/login.defs```
- Find this part ``PASS_MAX_DAYS 9999 PASS_MIN_DAYS 0 PASS_WARN_AGE 7``
- Change that part to ``PASS_MAX_DAYS 30`` and ``PASS_MIN_DAYS 2`` keep ``PASS_WARN_AGE 7`` as the same
- Lastly type ``sudo reboot`` to reboot the change affects


## Part 6: Creating a Group
- First type ```sudo groupadd user42 ```to create a group
- Then type ```sudo groupadd evaluating``` to create an evaluating group
- Lastly type ``getent group`` to check if the group has been created
- Type ``cut -d: -f1 /etc/passwd`` to check all local users
- Type ```sudo adduser new_username``` to create a username - write down your new_username, as you will need this later on.
- Type ```sudo usermod -aG user42 your_username``` to add the user to this group, do the same with same evaluating group.
- Type ``getent group user42`` to check if the user is the group, do the same with same evaluating group.
- Type ``groups`` to see which groups the user account belongs to
- Lastly ``type chage -l your_new_username`` to check if the password rules are working in users

## Part 7: Creating sudo.log
- First type ``cd ~/../``
- Then type ``cd var/log``
- Then type ``mkdir sudo ``(if it already exists, then continue to the next step).
- Then type ``cd sudo && touch sudo.log``
- Then type ``cd ~/../``

Part 7.1 - Configuring Sudoers Group
- First type ```sudo nano /etc/sudoers``` to go the sudoers file
- Now edit your sudoers file to look like the following by adding in all of the defaults:
  ``` - Defaults	env_reset
   - Defaults	mail_badpass
   - Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/bin:/sbin:/bin"
   - Defaults	badpass_message="Password is wrong, please try again!"
   - Defaults	passwd_tries=3
   - Defaults	logfile="/var/log/sudo.log"
   - Defaults	log_input, log_output
   - Defaults	requiretty```
   - 
## Part 8: BASH and Crontab
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
- Type sudo visudo to open your sudoers file
- Add in this line ```your_username ALL=(ALL) NOPASSWD: /usr/local/bin/monitoring.sh``` under where its written ```%sudo ALL=(ALL:ALL) ALL``` at ```Allow members of group sudo to execute any command```
- Then exit and save your sudoers file
- Now type ```sudo reboot``` in your Virtual Machine to reboot sudo
- Type ```bash monitoring.sh``` to execute your script.
- Then type ``apt-get install -y net-tools`` to install the netstat tools
Crontab
- Then type ``cd /usr/local/bin/``
Then type touch ``monitoring.sh``
Lastly type ``chmod 777 monitoring.sh``
- Type ```sudo crontab -u root -e``` to open the crontab and add the rule
- Lastly at the end of the crontab, type the following ```*/10 * * * * /usr/local/bin/monitoring.sh``` this means that every 10 mins, this script will show.



## Part 6: Evaluation
                                           ##################################
                                           #         Evaluation Q&A         #
                                           ##################################

- What is virtual machine?
--> A Virtual Machine (VM) is a compute resource that uses software instead of a physical computer to run programs and deploy 
apps. Each virtual machine runs its own operating system and functions separately from the other VMs, even when they are all
running on the same host. For example, you can run a virtual MacOS machine on a physical PC. 

- What it's purpose?
--> VMs may be deployed to accommodate different levels of processing power needs, to run software that requires a different
operating system, or to test applications in a safe, sandboxed environment. 

- How does it works?
--> VM working through "Virtualization" technology. Virtualization uses software to simulate virtual hardware that allows 
VMs to run on a single host machine.

- Diff between aptitude and apt?
--> Aptitude is a high-level package manager while APT is lower-level package manager which can be used by other 
higher-level package managers
(read more: https://www.tecmint.com/difference-between-apt-and-aptitude/)

- What is AppArmor?
--> AppArmor ("Application Armor") is a Linux kernel security module that allows the system administrator to restrict programs'
capabilities with per-program profiles.
(read more: https://en.wikipedia.org/wiki/AppArmor)

- What is SSH?
--> SSH, also known as Secure Shell or Secure Socket Shell, is a network protocol that gives users, particularly system 
administrators, a secure way to access a computer over an unsecured network.
(read more: https://searchsecurity.techtarget.com/definition/Secure-Shell)

- What is SELinux?
--> Security-Enhanced Linux (SELinux) is a security architecture for Linux® systems that allows administrators to have more control 
over who can access the system. 

- What is UWF?
--> Is a firewall configuration tool that runs on top of iptables , included by default within Ubuntu distributions. It provides a streamlined interface for configuring common firewall use cases via the command line.

- How your script works?
--> ... README.md / Part 8

/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\
| You have to create a new user here.        |<br>
| $ sudo adduser username                    | <- creating new user (yes (no)) <br>
| $ sudo chage -l username                   | <- Verify password expire info for new user<br>
| $ sudo adduser username sudo               | <br>
| $ sudo adduser username user42             | <- assign new user to sudo and user42 groups<br>
\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\//\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/

                                           ##################################
                                           #         What to check?         #
                                           ##################################

 1) lsblk                              <- check partitions<br>
 2) getent group sudo                  <- sudo group users<br>
 3) getent group user42                <- user42 group users<br>
 4) sudo service ssh status            <- ssh status, yep <br>
 5) sudo ufw status                    <- ufw status <br>
 6) ssh username@ipadress -p 4242      <- connect to VM from your host (physical) machine via SSH <br>
 7) nano /etc/sudoers.d/<filename>     <- yes, sudo config file. You can $ ls /etc/sudoers.d first <br>
 8) nano /etc/login.defs               <- password expire policy <br>
 9) nano /etc/pam.d/common-password    <- password policy <br>
 10) sudo crontab -l                   <- cron schedule <br>
********************************************************************************************************

 1 [$sudo nano /etc/hostname]  -------->  Change hostname                               
 2 [$cd /var/log/sudo/00/00 && ls]----->  Where is sudo logs in /var/log/sudo?          
 3 [$ sudo apt update]                                                                  
 4 [$ ls]                                                                               
 5 [$ cd <nameofnewdirectory> && ls] -->  Now you see that we have a new directory here.
 6 [$ cat log] <- Input log                                                             
 7 [$ cat ttyout] <- Output log                                                         
 8 [$ sudo ufw allow 8080]  ----------->  8)  allow port 8080 in UFW                    
 9 [$ sudo ufw status] ---------------->  9)  check                                     
 10 [$ sudo ufw deny 8080]  ----------->  10) deny (yes yes)                            
******************************************************************************************

Crontab
- How to run script every 30 seconds?<br>
```[$ sudo crontab -e]```         <br>
*/1 * * * * /path/to/monitoring.sh <br>
*/1 * * * * sleep 30s && /path/to/monitoring.sh <br>

- To stop script running on boot
```$ sudo service cron stop ```

- To restart it
```$ sudo service cron restart ```

## PD -> Please don't copy and paste, try to understand the whole process

<hr />
<img alt="html5" src="https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white" style="max-width: 100%;">



