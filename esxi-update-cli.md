### Updating ESXI from CLI 

To update ESXi via CLI, follow these steps:

1. Log in to the ESXi host using SSH.
2. Put the host in maintenance mode by running the command: `vim-cmd hostsvc/maintenance_mode_enter`.
3. Check for available updates by running the command: `esxcli software sources profile list --depot=<path-to-depot>`.
    
    ```bash
    esxcli software sources profile list -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
    ```
    
4. Select the desired update profile from the list.
5. Run the command to install the update: `esxcli software profile update --depot=<path-to-depot> --profile=<profile-name>`.
    
    ```bash
    esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-8.0U2-22380479-standard
    ```
    
    Note: Make sure to replace `<path-to-depot>` with the path to the ESXi update depot, and `<profile-name>` with the name of the selected update profile.
    
6. Once the update is installed, reboot the host by running the command: `reboot`.
7. Remember to exit maintenance mode after the host has rebooted by running the command: `vim-cmd hostsvc/maintenance_mode_exit`.
