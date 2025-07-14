# installation
- during installation it might stuck on Connecting to downloads.ioncube.com (downloads.ioncube.com)|192.241.136.243|:443...  to download an archive. in this case you can manually download the archive and place it at /usr/src/fusionpbx-install.sh/debian/resources, then edit /usr/src/fusionpbx-install.sh/debian/resources/ioncube.sh and comment out the wget line which tells the script to download the archive

# Gotcha
## SIP Profiles
- If you are having rouble with sip profiles not starting or restarting on status > sip status it is probably there is another freeswitch instance running on the same port you are trying to start the sip profile
- You can see the possible reasons by sshing into your fusion pbx instance and executing `fs_cli` then `log level debug`
- now you can start or restart the sip profile while monitoring the terminal output
- to check if the port is already take execute `ss -ltunp | grep port` where port is the port defined  in the sip-port parameter of the sip profile, for me changing this to 5060 in the external sip profile fixed the invalide External profile issue
- in addition you could try changing the ext-rtp-ip and ext-sip-ip if the problem persists, for me it was 10.0.101.108 for both

## Gateways
```
Gateway: ethiotelecom
Username: +251111133622@ims.ethiotelecom.com
Password: zerintser12CD34#56
From User: +251111133622
From Domain: ims.ethiotelecom.com
Proxy: ims.ethiotelecom.com
Expire Seconds: 3600
Register: True
Retry Seconds: 30
Context: public
profile: external
enabled: true
Register Proxy: 10.208.233.134
```
## ACL
- add your sip providers ip to the allow list of providers under advanced > access control
    - 10.208.233.0/24 allow
## Web phone
- after following the instruction to install browser phone if you are having a problem saying `Failed to locate the Project Root by searching for project_root.php please contact support for assistance` just touch /var/www/fusionpbx/project_root.php and it will be fixed
- https://www.pbxforums.com/threads/webphone-for-fusionpbx.7301/
- if you are getting access denied, it is likely you are using agent type for the user you are trying to login in, to fix this issue you need to figure out what is blocking access in the /var/www/fusionpbx/core/phone/index.php file line13 https://www.pbxforums.com/threads/webphone-for-fusionpbx.7301/post-30118
- once webphone opened and if it is stuck on connecting to websocket click on the red wifi icon click on okay on the popup and accept the certificate

# Installation
- on debian12
```
wget -O - https://raw.githubusercontent.com/fusionpbx/fusionpbx-install.sh/master/debian/pre-install.sh | sh;
cd /usr/src/fusionpbx-install.sh/debian && ./install.sh
```
```
cp ioncube_loaders_lin_x86-64.zip /usr/src/fusionpbx-install.sh/debian/resources
```
```
nano /usr/src/fusionpbx-install.sh/debian/resources/ioncube.sh
```
- once installation finished login using the provided credentials

# Browser phone
```
git clone https://github.com/amitiyer/Fusionpbx-WEBRTC.git
cd Fusionpbx-WEBRTC
cp -R Browser-Phone /var/www/fusionpbx/
cp -R core/phone  /var/www/fusionpbx/core/
nano /etc/freeswitch/tls/all.pem
service freeswitch restart
systemctl restart freeswitch
touch /var/www/fusionpbx/project_root.php
```

# SSL



        #make sure the freeswitch directory exists
        mkdir -p /etc/freeswitch/tls

        #make sure the freeswitch certificate directory is empty
        rm /etc/freeswitch/tls/*

        cat fullchain.pem > /etc/freeswitch/tls/all.pem
        cat privkey.pem >> /etc/freeswitch/tls/all.pem

        #copy the certificates
        cp cert.pem /etc/freeswitch/tls
        cp chain.pem /etc/freeswitch/tls
        cp fullchain.pem /etc/freeswitch/tls
        cp privkey.pem /etc/freeswitch/tls
        
        ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/agent.pem
        ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/tls.pem
        ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/wss.pem
        ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/dtls-srtp.pem
        chown -R www-data:www-data /etc/freeswitch/tls

flush cache, reload xml, reload acl and restart sip profiles

#update nginx config
sed "s@ssl_certificate[ \t]*/etc/ssl/certs/nginx.crt;@ssl_certificate /etc/freeswitch/tls/fullchain.pem;@g" -i /etc/nginx/sites-available/fusionpbx
sed "s@ssl_certificate_key[ \t]*/etc/ssl/private/nginx.key;@ssl_certificate_key /etc/freeswitch/tls/privkey.pem;@g" -i /etc/nginx/sites-available/fusionpbx

#read the config
/usr/sbin/nginx -t && /usr/sbin/nginx -s reload
