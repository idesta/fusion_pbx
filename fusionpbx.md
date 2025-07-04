# Fusion PBX Documentation

## 1. Step -1 Install Ubuntu Server 24.04
-  Update and upgrade the server.

## 2. Create an account and login
- https://www.fusionpbx.com/login
- Discover the dashboard

## 3. Quick install FusionPBX on Ubuntu Server 24.04

- Go to https://www.fusionpbx.com/download

- Access your server via ssh (ssh username@server_IP)

- > cd /usr/src/

- paste the url from the download page then hit Enter

    - wget -O - https://raw.githubusercontent.com/fusionpbx/fusionpbx-install.sh/master/ubuntu/pre-install.sh | sh;

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

        - > ./install.sh
    
    ```
