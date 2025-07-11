# Fusion PBX Documentation

## 1. Step -1 Install Debian 12 server (2vCPU,4GB RAM,30GB SSD)
-  Update and upgrade the server.

## 2. Create FusionPBX account and login
- https://www.fusionpbx.com/login
- Discover the dashboard

## 3. Quick install FusionPBX on Debian 12

- Go to https://www.fusionpbx.com/download

- Access your server via ssh (ssh username@server_IP)

- > cd /usr/src/

- paste the url from the download page then hit Enter

    - wget -O - https://raw.githubusercontent.com/fusionpbx/fusionpbx-install.sh/master/debian/pre-install.sh | sh; 

    - The installation command basically do the below scripts

        ```
        #!/bin/sh

        #upgrade the packages
        apt-get update && apt-get upgrade -y

        #install packages
        apt-get install -y git lsb-release

        #get the install script
        cd /usr/src && git clone https://github.com/fusionpbx/fusionpbx-install.sh.git

        #change the working directory
        cd /usr/src/fusionpbx-install.sh/ubuntu

        ```

- Then check the .install.sh file existed inside the downloaded file path.
    ```
    - **root@fusionpbx:/usr/src#** cd fusionpbx-install.sh/debian/

    - **root@fusionpbx:/usr/src/fusionpbx-install.sh/ubuntu#** ls

            install.sh      pre-install.sh      resources

    - $ ./install.sh
    
    ```
## 4. Login to the PBX Dashboard & Change the user credentials

- At the end of the installation script; **the username (admin), password and access url (https://server_ip)** will be provided. 
    - So Login with those credentials
    - Change those login credentials in the Home -> Account setting of the PBX Dashboard.
    - Save the configuration and Login again.

## 5. Create Users
- Go to 
- Agent Users
- Additional Superadmins

## 6. Create Extension

## 7. fs_cli, sngrep

## 8. Phome Option

    ```
        https://github.com/amitiyer/Fusionpbx-WEBRTC
        
    ```
-  