
# FreePBX

> Hardware recommendation

40 users/ 10 concurrent calls

- 250 GB HDD
- 2 GB

100 users / 30 concurrent calls

- 500 GB HDD
- 4 GB

500 users / 100 concurrent calls

- i5 CPU
- 500 GB SSD
- 8 GB RAM
 
> Key elements for a stable voip

- use network switchs
- network switch between IP PBX and IP phones should be at full duplex mode
- separate voice and data using vlan
- use 802.1q vlan standard
- use qos to prioritze voice traffic
- use 802.1P priority setting (default is 5)
- for remote offices that go over the wan use bandwidth optimized codecs like opus or g729
- your ip pbx server should be on static ip

> First steps after installation

- it is a good idea to activate the freepbx installation since it is free
- configure the external and internal network
  - go to settings > asterix sip setting
  - you can detect network settings automatically
- then assign a static ip and dns for your pbx server on admin > system admin
- give your pbx a hostname by going to admin > system admin.this hostname will show up in network scans and also when you ssh into your pbx the hostname will be shown.
- setup email on system admin > notification
- go to system admin > time zone and set the correct time zone
- go to system admin > storage and put your mail address for failure notification email

> Setting up the phone system

When creating extensions it is a good practice to not use 1XX as the extension number because it could conflict with freepbx feature code

- go to Application > Extention > Add Extentions
  - give it an extention number i.e 1001
  - give it a display name i.e director
  - setup a secret i.e 123456

> Setting up dahdi

- Go to connectivity > dahdi config
- Then enable dahdi
- Then Go to connectivity > trunks
- Click on add trunk
- Then click on add dahdi trunk
- Give your trunk a name
- Enter your phone number on the outbound caller ID
- Then navigate to dial pattern tab
  - And insert X. on the match pattern field to allow any type of number to be called out.
  - Then go to the dadhi setting tab and select the dadhi port that your analog cable is plugged into

- Then submit and apply config

> Outbound Routes

- Goto connectivity > outbound Routes
- Click on Add outboudn Routes
- Give the route a name i.e FXOutbound
- Add a password to that route if needed
- Then Select the trunk you have created before on the Trunk Sequence for Matched Routes field
- Then go to the dial pattern tab and add a pattern on the match pattern field i.e X. to allow any kind of number to be dialed
- Then submit and apply

> Inbound Routes

- Goto connectivity > inbound Routes
- Click on Add Inbound Route
- Give it a description i.e incomingFXO
- On the Set Destination select Extentions and select the extetnion you want to forward the call to
- Then click on submit and apply config

> Voicemail

- Goto the extention you want to setup voicemail to
- Enable the voice mail on voicemail tab
- Give the voice mail password
- Then submit and apply config
  the feature code \*97 is the way to access your voicemail
  and if you dont want the voice mail feature you can set up follow me setting to ring other extetnion if you did not pick up

> Important Notes
- system recording location: `/var/lib/asterisk/sounds/custom/`
- /var/spool/asterisk/monitor: where all recording gets stored
- extension > advanced > extention option > ring time: ring time for extension
- On analog lines to clear the line after external party disconnect the call
  - edit the chan_dahdi.conf file under pbx > asterisk file editor and uncomment(;) the following two lines
    - busydetect=yes
    - busycount=3

> SSL encryption

- /etc/pki/tls/certs/localhost.crt
- /etc/pki/tls/private/localhost.key

- copy your certificate file to a suitable folder on the server i.e /root
  - /root/fullchain.pem
  - /root/privkey.pem
- then replace the following files with the certificate
  - cp fullchain.pem /etc/pki/tls/certs/localhost.crt
  - cp privkey.pem /etc/pki/tls/private/localhost.key
- `service httpd restart`

> Debranding

- scp favicon.ico root@callcenter.zergaw.com:/var/www/html/admin/images
- scp operator-panel.png support.png sys-admin.png user-control.png root@callcenter.zergaw.com:/var/www/html/admin/assets/images
- scp 16x16.png 60x60.png 76x76.png 120x120.png 128x128.png 152x152.png 192x192.png 320x480.png root@callcenter.zergaw.com:/var/www/html/ucp/assets/icons
- scp freepbx_small.png sangoma-horizontal_thumb.png root@callcenter.zergaw.com:/var/www/html/admin/images
- scp tango.png root@callcenter.zergaw.com:/var/www/html/admin/images

- #0f5a59 -> #006cd8
- rgba(94,156,125,0.9) -> rgba(0, 108, 216, 1)
- #d6e4dd -> #f5f5f5
- #3b8c62 -> #006cd8
- #4c9471 -> #006cd8

> Unused modules

- Advanced Recovery
- Call Accounting
- Class of Service
- FreePBX Support
- Online support
- Queue Pause Codes
- Queue Penality Codes
- SangomaConnect
- SmartOffice
- Zulu
- Appointement Reminder
- Broadcast
- CallerID Management
- Conference Pro
- Oracle connector
- Park and announce
- Queue Callback
- Queue Priority
- Virtual Queues
- Voicemail notification
- Web Callback
- Endpoint Manager
- CRM Settings
- CRM API Settings
- Call Recordings
- Pinsets Code reports
- Queue Callback reports
- Queue Report Schedulter
- Queue Report Templates
- Queue Reports
- Queue Wallboard
- Voicemail reports
- isynphony v3 panel
- Outbound call limit
- Appointment reminder
- Broadcast
- CallerID management
- Conference pro
- Oracle connector

> Modules to be enabled for the client admin

- Allowlist
- Blacklist
- Contact Manager
- Feature Codes
