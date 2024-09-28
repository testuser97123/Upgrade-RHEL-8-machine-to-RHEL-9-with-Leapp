## How to in-place upgrade an offline / disconnected RHEL 8 machine to RHEL 9 with Leapp? 

## Environment

 -   Red Hat Enterprise Linux 9.x
 -   Red Hat Enterprise Linux 8.x
 -   leapp

## Resolution

IMPORTANT: This process will NOT work with RHUI repositories (AWS, Azure, GCP)


# In RHEL 8 Machine :

#### Copy all data from RHEL 9.4 iso to local machine directory.
    
    $ mount /dev/cdrom /opt 
    $ cp -rfv /opt  /mnt 

#### Create yum repository. 

    $ vim /etc/yum.repos.d/server.repo 
    [BaseOS]
    enabled = true
    gpgcheck = false 
    baseurl = file:///mnt/BaseOS
    name = System Base Repository 
    [AppStream]
    enabled = true
    gpgcheck = false 
    baseurl = file:///mnt/AppStream
    name = System AppStream Repository

IMPORTANT: Do not use mount point for ISO mounting.


#### Install the Leapp Utility

Leapp is the most interesting part of this process as it is going to automate the whole process of upgradation. To install the Leapp utility, use the given command:

    $ sudo dnf install leapp-upgrade -y 

#### Remove VersionLock Plugin 

    $ sudo dnf versionlock clear 
    $ sudo dnf clean all 

### Disable AllowZoneDrifting 

This will cause major issues during the upgrade process and gives you an error such as given below:

    $ sudo vim /etc/firewalld/firewalld.conf 
Go to end of the line and you will find the option of AllowZoneDrifting.
    AllowZoneDrifting=no

### Disable Root remote Access.

    $ sudo vim /etc/ssh/sshd_config
    PermitRootLogin no 

### Reboot Server 

### Perform the Pre-upgrade Phase
Upgrading the system is big deal and this is the best way to check whether there are any issues related to packages for the upcoming upgrade session. The below command will check for package availability and check for system issues (if any).

    $ sudo leapp preupgrade --no-rhsm --enableRepo BaseOS --enableRepo AppStream 

### Upgrading from RHEL 8 to RHEL 9
    
    $ sudo leapp upgrade --no-rhsm --enableRepo BaseOS --enableRepo AppStream

### Once the process of downloading and installing new packages is done, reboot your system.

    $ reboot 

### Choose RHEL-Upgrade-initramfs
Once you reboot, choose the third option labeled “RHEL-Upgrade-initramfs”.

### Verify RHEL 9 Upgrade

    $ sudo cat /etc/redhat-release 

