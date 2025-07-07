# Error you may got

## Error no 1. 

### When you try to install with out the root priviledge. 

 wget -O - https://raw.githubusercontent.com/fusionpbx/fusionpbx-install.sh/master/debian/pre-install.sh | sh;
--2025-07-02 08:53:37--  https://raw.githubusercontent.com/fusionpbx/fusionpbx-install.sh/master/debian/pre-install.sh



Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.109.133, 185.199.110.133, 185.199.111.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.109.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 299 [text/plain]
Saving to: 'STDOUT'

-                              100%[==================================================>]     299  --.-KB/s    in 0s      

2025-07-02 08:53:38 (4.77 MB/s) - written to stdout [299/299]

Reading package lists... Done
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
sh: 10: git: not found
sh: 13: cd: can't cd to /usr/src/fusionpbx-install.sh/debian


## Solution
- Try to install by using the **'root' priviledge**
- > sudo su
- then paste the above url and hit enter

### Error no 2

> **ioncube error**

```
--2025-07-04 06:25:56--  

https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.zip

Resolving downloads.ioncube.com (downloads.ioncube.com)... 192.241.
136.243

Connecting to downloads.ioHello everyone, happy Friday ☺  
Please be reminded that Friday is the LAST DAY to complete your home work and checklist on Analytics in AWS and Machine Learning in AWS. Friday should always be used to prepare yourselves for the coming week. Therefore, please start watching the 2 class videos on Monitoring and Auditing in AWS and Disaster Recovery, Migration and NoSQL DB's In AWS. DO NOT just watch the videos, make sure that you are pausing the videos and practicing on the AWS management console on your computer. Please read the lecture note side by side and prepare your questions, if any, for the coming live classes on Saturday and Sunday.


See you.ncube.com (downloads.ioncube.com)|192.241.136.243|:443...

```

## Solution

1. check php -v

    ```
    php -v

    Failed loading /usr/lib/php/20210902/ioncube_loader_lin_8.1.so:
            
    /usr/lib/php/20210902/ioncube_loader_lin_8.1.so: cannot open shared object file: No such file or directory

    PHP 8.1.33 (cli) (built: Jul  3 2025 16:16:18) (NTS)

    Copyright (c) The PHP Group Zend Engine v4.1.33, 

    Copyright (c) Zend Technologies with Zend OPcache v8.1.33,

    Copyright (c), by Zend Technologies

    ```

2. check this github link

    https://gist.github.com/grafikchaos/4f1f21d549a5ecc06c60

3. check your cpu architecture

    > uname -m

4. Download the zip file from safari network/connect other countries VPN. (https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.zip)

5. Copy(scp) the zip file to / (root of the remote server) and move to the destination server

    - scp ioncube_loaders_lin_x86-64.zip username@hostip:~/

    - mv ioncube_loaders_lin_x86-64.zip /usr/src/fusionpbx-install.sh/debian/resources

5. Go to and edit the bash/script file of ioncube.sh

    > /usr/src/fusionpbx-install.sh/ubuntu/resources#
    - when you list files under this directory you will see ioncube.sh file.
    > nano ioncube.sh

    - Edit the bash script file
        - > comment the wget command

            ```
            #get the ioncube load and unzip it
            if [ .$cpu_architecture = .'x86' ]; then
        
            #get the ioncube 64 bit loader

            #wget --no-check-certificate https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.zip

            ```
    - Save the script and check using 'cat' command

6. Go back to /usr/src/fusionpbx-install.sh/debian/

    - > ls

        - Make sure that there is 'install.sh' file

    - > ./install.sh
