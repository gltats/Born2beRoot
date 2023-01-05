# Born2beRoot
42 project that aims to introduce to the world of virtualization


## Rules:
The use of VirtualBox is mandatory.
This project consists of having you set up the first server by following specific rules.
Operating system either the latest stable version of Debian
SELinux must be running at startup and its configuration has to be adapted for the project’s needs.
AppArmor for Debian must be running at startup too.
Must have at least 2 encrypted partitions using LVM. 

A SSH service will be running on port 4242 only. For security reasons, it must not be possible to connect using SSH as root.
The use of SSH will be tested during the defense by setting up a new account. You must therefore understand how it works.
You have to configure your operating system with the UFW firewall and thus leave only port 4242 open.
Your firewall must be active when you launch your virtual machine.

- The hostname of your virtual machine must be your login ending with 42 (e.g.,wil42). You will have to modify this hostname during your evaluation.
- You have to implement a strong password policy.
- You have to install and configure sudo following strict rules.
- In addition to the root user, a user with your login as username has to be present.
- This user has to belong to the user42 and sudo groups.
During the defense, you will have to create a new user and assign it to a group.

To set up a strong password policy, you have to comply with the following requirements:
- Your password has to expire every 30 days.
- The minimum number of days allowed before the modification of a password will
be set to 2.
- The user has to receive a warning message 7 days before their password expires.
- Your password must be at least 10 characters long. It must contain an uppercase letter, a lowercase letter, and a number. Also, it must not contain more than 3 consecutive identical characters.
- The password must not include the name of the user.
- Of course, your root password has to comply with this policy.
- The following rule does not apply to the root password: The password must have at least 7 characters that are not part of the former password.

To set up a strong configuration for your sudo group, you have to comply with the following requirements:
- uthentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
- A custom message of your choice has to be displayed if an error due to a wrong
password occurs when using sudo.
- Each action using sudo has to be archived, both inputs and outputs. The log file
has to be saved in the /var/log/sudo/ folder.
- The TTY mode has to be enabled for security reasons.
- For security reasons too, the paths that can be used by sudo must be restricted.


Finally, you have to create a simple script called monitoring.sh. It must be developed in bash.
At server startup, the script will display some information (listed below) on all terminals every 10 minutes (take a look at wall).
Your script must always be able to display the following information:
- The architecture of your operating system and its kernel version.
- The number of physical processors.
- The number of virtual processors.
- The current available RAM on your server and its utilization rate as a percentage.
- The current available memory on your server and its utilization rate as a percentage.
- The current utilization rate of your processors as a percentage.
- The date and time of the last reboot.
- Whether LVM is active or not.
- The number of active connections.
- The number of users using the server.
- The IPv4 address of your server and its MAC (Media Access Control) address.
- The number of commands executed with the sudo program.
