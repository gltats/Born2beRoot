# Born2beRoot
42 project that aims to introduce to the world of virtualization

## Part 1: Setting up a VM

## Part 2: SSH and UWF

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

## PD -> Please don't copy and paste, try to understand the whole process

<hr />
<img alt="html5" src="https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white" style="max-width: 100%;">



