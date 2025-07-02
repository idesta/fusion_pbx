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

