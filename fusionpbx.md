# Fusion PBX Documentation

## 1. Step -1 Install debian 12 (bookworm)
-  Update and upgrade the server.

## 2. Create an account and login
- https://www.fusionpbx.com/login
- Discover the dashboard

## 3. Insert your username (beyond your 'root' account) to 'sudoers config file in your debian server.

> **Use this steps**
- > su 
    - Enter your root password
- > vi /etc/sudoers
    ```
        User privilege specification
     
        root    ALL=(ALL:ALL) ALL
        zgadmin ALL=(ALL:ALL)   ALL
    ```
- Click [Esc] -> then shift + (;) + write wq! and then enter

- check whether it's saved or not 
    - cat /etc/sudoers
- Exit from your root account privilege and check from the user account (eg. sudo apt update)

## 4. Quick install FusionPBX in debian 12

- Go to https://www.fusionpbx.com/download

- Access your server via ssh (ssh username@server_IP)

- > cd /usr/src/

- paste the url from the download page then hit Enter

    - wget -O - https://raw.githubusercontent.com/fusionpbx/fusionpbx-install.sh/master/debian/pre-install.sh | sh;

- Then check the .install.sh file existed inside the downloaded file path.

    - **root@fusionpbx:/usr/src#** cd fusionpbx-install.sh/debian/

    - **root@fusionpbx:/usr/src/fusionpbx-install.sh/debian#** ls

            install.sh  pre-install.sh  resources
    - > ./install.sh
    

