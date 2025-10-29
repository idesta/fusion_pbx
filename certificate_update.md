
1. copy the updated certificate files fro your local machine to the pbx server
    - cert1.pem
    - chain1.pem
    - fullchain1.pem
    - privkey1.pem



    scp /home/zergaw/Downloads/zergaw.com/privkey1.pem root@10.0.101.108:/etc/freeswitch/tls
    scp /home/zergaw/Downloads/zergaw.com/fullchain1.pem root@10.0.101.108:/etc/freeswitch/tls
    scp /home/zergaw/Downloads/zergaw.com/chain1.pem  root@10.0.101.108:/etc/freeswitch/tls
    scp /home/zergaw/Downloads/zergaw.com/cert1.pem root@10.0.101.108:/etc/freeswitch/tls


2. Backup the old certificate files inside your server


    - mkdir -p certbackup

    - mv /etc/freeswitch/tls/cert.pem /root/certbackup/
    - mv /etc/freeswitch/tls/chain.pem /root/certbackup/
    - mv /etc/freeswitch/tls/fullchain.pem /root/certbackup/
    - mv /etc/freeswitch/tls/privkey.pem /root/certbackup/

3. Put them on the same file (all.pem)

    - cat fullchain.pem > /etc/freeswitch/tls/all.pem
    - cat privkey.pem >> /etc/freeswitch/tls/all.pem

4. Remove all cert files

    - rm -f agent.pem
    - rm -f tls.pem
    - rm -f wss.pem
    - rm -f dtls-srtp.pem
    - rm -f dtls-srtp.pem.old


5. ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/agent.pem
   ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/tls.pem
   ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/wss.pem
   ln -s /etc/freeswitch/tls/all.pem /etc/freeswitch/tls/dtls-srtp.pem

6. Perform FreeSWITCH Cache/XML/ACL/SIP Reloads

    - fs_cli -x "reloadxml"
    - fs_cli -x "reloadacl"
    - fs_cli -x "sofia profile external restart" # Restart specific SIP profiles.
    - fs_cli -x "sofia profile internal restart" # Adjust profile names as per your setup.

7. Update Nginx Configuration Files (using your sed commands):

    - sed "s@ssl_certificate[ \t]/etc/ssl/certs/nginx.crt;@ssl_certificate /etc/freeswitch/tls/fullchain.pem;@g" -i /etc/nginx/sites-available/fusionpbx
    - sed "s@ssl_certificate_key[ \t]/etc/ssl/private/nginx.key;@ssl_certificate_key /etc/freeswitch/tls/privkey.pem;@g" -i /etc/nginx/sites-available/fusionpbx

8. Quickly inspect the fusionpbx config file to ensure the changes were made correctly:

    - cat /etc/nginx/sites-available/fusionpbx | grep -E "ssl_certificate|ssl_certificate_key"

            - You should see the new paths (/etc/freeswitch/tls/fullchain.pem; and /etc/freeswitch/tls/privkey.pem;)

9. Test Nginx Configuration:

    - /usr/sbin/nginx -t

            - You should see output similar to:

                    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
                    nginx: configuration file /etc/nginx/nginx.conf test is successful

10. Reload Nginx Service

    - /usr/sbin/nginx -s reload

11. Verify the SSL Update

    - check with new incognito window
    - check with other browsers
    - delete caches
