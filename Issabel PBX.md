## IsabellaPBX

> Installation

- Download the iso from issabel.org
- Boot from the installation media
- click on install on the first prompt
- on the installation screen choose your language
- Then click on the installation destination on the installation summary page
- Then click on done while the automatically configure partiting is selected
- The click on the software selection
- choose the software version you want to install (Issabel with asterisk 16) and select all three addons on the right
- Then click on network and hostname
- Click on configure at the bottom of the screen
- On the IPv4 tab choose wheter you want your pbx on the dhcp or static
- The click on start installation
- set root password while the installation is happening
- Once installation is done and rebooted, set a root password for the mariaDB when prompted
- Then enter a password for the admin user to access the web interface

Now installation is completed and you can login to your pbx using the root password set earlier or using the admin password through the admin interface

> post installation procedures

- ssh into your issabel pbx and perform a system upgrade
  - `yum -y update`

> Changing Network and IP parameter

- Navigate to System > Network > Network Parameters
- Click on the device name
- Here you can set ip information of the pbx

> Creating sip phone extension

- go to PBX
- click on add extension
- select generic sip device from the dropdown and click submit
- fill in
  - User Extensio: 1000
  - Display name: Name
  - Secret: password
- then click on submit and apply config11

> Analog card configuration

- Install the pci card
- Navigate to System > Hardware detector
- It will scan and detect the card and shows information on which port is populated with a fxo/fxs module
- we can click on configuration of span and select the software based echo cancellation for the module you want if you have an echo on your pstn line

> Trunks

- By default a dahdi trunk is created when issabel is installed
- Go to PBX > Trunks and select the trunk from the left listing

> Dial Patterns

- X matches any digit from 0-9
- Z mathces any digit from 1-9
- N mathces any digit from 2-9
- [1236-9] matches any digit or letter in the brackets(in this example 1,2,3,6,7,8,9)
- . is a wildcard that matches any number and length

> Outbound Routes

- Navigate to outbound route
- Give your route a name
- Insert a dial pattern as you wish
- On the trunks section select your previously trunk
- Submit changes and apply

> Class of Service

- Navigate to PBX > class of service
- Give your class a name and description and click create
- Give the class the dial rules you want the class to have(refer dial pattern)
- On the Set All To - Allow(this are the feature of the pbx we want to allow them all)
- On the entry All Outbound routes choose Allow Rules
- On the outbound route section, beside your trunk name select Allow Rules
- Submit and apply
- repeat this step for all you class of services(i.e localcall, nationalcall, internationalcall)
- navigate to your trunk settings and on the dailed number manipulation rules put X. on the match patter field
- navigate to the extension you want and assign the approprate class of service
  - select the extension, on the class of service dropdown: select the class of service you want to assign

> Inbound routes

- Navigate to PBX > Inbound route
- Give your route a description
- Here you can add a specific DID number which instructs the pbx when a call comes in from this did number / or if left empty when a call comes from any did number
- Then at the bottom on the set destination section select where you want to route the call to.

> Ring Group

- Navigate to PBX > Ring group
- Give your ring group a number
- Give it a description
- select a ring strategy(i.e ringall)
- check skip if busy checkbox
- Put the extension numbers you want to ring in the extension list or use the extension quick pick to select them
- Select destination if no answer
- submit changes and apply
- Modify or create an inbound route and set the destination to the newly created ring group

> System recording - audio prompts

- Go to PBX > System recording
- Select the recording and click on upload
- Name the recording
- Save

> IVR

- Navigate to PBX > IVR
- Click on add a new IVR
- Give the IVR a name and description
- Select the audio prompt uploaded on the system from the Announcement dropdown
- Enable of disable direct dial to your extension from the Direct dial dropdown based on your preference
- At the bottom on the IVR Entries plan your IVR route as you want

> Voicemail and voicemail to email

- Navigate to the extension and voicemail section
- Enable the feature
- Give it a password to access your voicemails
- Input an email address where the voicemail should be sent to
- Enable email attachement and play cid
- submit and apply
- we can view the voicemail recording either from the extension or by navigating pbx.address/recordings/ and inputing the extension number and password to login

> Announcement

- Navigate to PBX > Announcement
- Click on Add
- Give it a description
- Selecte a recording
- Then select your destination after playback()

> Opening and closing hours

- Frist add a time group by navigating PBX > Time group
- Give your time group a description(i.e standard_working_hours)
- select time to start(i.e 02 29)
- select time to finish(i.e 11 30)
- select week day to start(i.e monday)
- select week day to finish(i.e sunday)
- select month day start(i.e 1)
- select month day finish(i.e 31)
- select month start(i.e january)
- select month finish(i.e december)
- submit and apply
- now go to PBX > Time conditions
- give your time condition a name
- select the time group you want to apply
- and select the destination if time matches(i.e main IVR)
- and select the destination if time does not match(i.e announcement)
- submit
- navigate to your inbound route
- on the set extension section select time condition and select the time condition you created earlier
- submit and apply

> User group and access permission management

- Navigate to System > Users
- Here we can create a user and associate them with an extension
  - this will allow them to login to the pbx to
    - view their voicemails
    - view their call recordings
    - enable or disable call waiting, forwarding ...
- under System > Users > Groups we can create a group to categorise our users
- under System > Users > Groups permission we can allow and deny specific permissions related to a user group

> Monitoring live calls

- we need to create a user with operator permission using the previous step
- login to the pbx using the operators credentials
- navigate to pbx > operator panel, from here you can view
  - registered and non registerd extensions
  - active calls and other informations
- if we navigate to pbx > flash operator panel, the operator can
  - view active call status
  - initiate calls between extensions

> Address Book

- Agends > Address Book
- Here we can add contacts

> Feature codes

- PBX > Feature code

> Backup and Restore

- System > Backup and Restore
- Click on Perform a Backup
- Select the features you are using or Select All and deselect the one you are not using(i.e deselect email, other )
- Click on process
- Return to Backup lists and click on the backup name to download it
- Can also enable automatic backup by clicking Set automatic backup by choosing daily, weekly or monthly from the dropdown menu

> Call Center Module

- Install the call center module by sshing into your pbx installation
- `yum install issabel-callcenter`
- refresh the web interface

> Creating role and users

- before creating users and groups for the call center we should create the required sip extensions
- then navigate to system > users
- create a new group i.e supervisor, agent
- go to system > group permissions
- click on filter ans select a group i.e supervisor
- then select all checkboxs related to call center
- then click on save selected as accesable
- then create the agent group with agent console permission under call center

> Creating Agents

- go to system > users > create new user
- proivde login name i.e same as extension number
- provide name i.e agent1
- choose agent group
- associate the user with sip extension

- now go to callcenter > agent options > add new agent
- create an agent number i.e same as extension
- provide password

> Configuring agent callback login

- go to callcenter > agent option > calback
- provide the required information

> Creating Break

- go to callcenter > breakes > create new break
- give it name and description i.e personal, tea, lunch, email

> Call service queue

- go to PBX > Queue
- add queue number i.e 655
- give it a name i.e incomming-queue
- add static agents from extension quick pick dropdown
- we need to add "A" before the extension number for issabel call center module compatability
  i.e A1002, 0
  A1001, 0
- choose ring strategy i.e rrmemory
- tick auto fill
- skip busy agents: queue calls only
- recording mode: after answer
- max wait time
- agent timeout
- submit change

> Agent script

- go to callcenter > Ingoing calls > select queue
- select queue from the dropdown
- input agent queue on script section i.e welcome to the company

> Form and data input during livecall

- go to callcenter > form > new form
- give it a name and description
- create a form i.e first and last name
- you can view form under form preview tab

> Issabel PBX security for cloud

- Security > Firewall > Define Ports
- Try to map all standard port to non standard port(i.e ssh)
- Edit on ssh entry
- Click on edit and change the port, save
  - change the ssh port on your pbx server
  - navigate to /etc/ssh/sshd_config
  - uncomment Port 22 and change it to Port 2345
  - save and exit
  - restart the sshd demon, systemctl restart sshd
- Disable all ports that we dont use

> Directories used by asterisk

- /etc/asterisk: all configuration files of asterisk
- /usr/lib/asterisk/modules: all asterisk modules
- /etc/dahdi: dahdi configuration file 'system.conf'
- /var/lib/asterisk/sounds: all audio files used in IVR/announcements
- /var/lib/asterisk/moh: all music on hold files
- /var/lib/asterisk/agi-bin: used for all AGI-Scripts
- /var/spool/asterisk/monitor: default directory for call recording

# Zergaw Call Center

> Trunks

we have a dahdi trunk (Ethiotelecom PSTN Trunk) configuration that accepts both line 1 and line 2 from ethiotelecom as a group

with this trunk

- we can accept two concurrent calls from the outside
- we can make two concurrent calls to the outside

at most only two calls will be established to and from outside

> Inbound Route

we have two types of inbound route

- one that capture all incomming calls
- one that capture incomming calls with specific CID

> All Incoming Calls

An incoming route that captures all incoming calls and checks a time condition to further route the call

> CID specific route(AgentX-0989898989)

An incoming route that capture calls with a specific CID and checks a time condition to further route call

> Time condition

The time condidtion recives a time group as an input and decided to forward incoming call based on the time it arrived

- In our case we have a general time condition called `Normal Office Hour` that takes `Office Hour` time group as an input and
  - if incoming call time matchs the time condition it will forward the call to the `Main IVR`
  - if the time doesnt match the call will be forward to a `Non Office Hour IVR`
- And we also have an Agent specific time condition for the case of routing calls from a CID specific route to a specific Agent queue, it takes the same `Office Hour` time group and
  - if incoming call time matchs the time condition it will forward the call to a queue called `Agentn-Queue`
  - if the time doesnt match, the call will be forwarded to misc extension that forward call to an external cell number of the agent

> Time group

We have a time group caled `Office Hours` with two time group section

- one covers office hours from monday to friday from, from 08:29 to 17:30 full year and
- one covers office hours on saturday, from 08:29 to 13:00 full year

> IVR

### Main IVR

We have an IVR called `Main IVR` with

- 30 as a timeout(this means if a user doesnt interact with the ivr within 30 second of the IVR announcement it will time)
- with 3 invalid retries
  - this happens if a user input a wrong number that is not on the IVR
  - it will announce invalid retry recording
  - it will append announcement on invalid(this means it will reannounce the IVR announcement)
  - it will route the call to reception queue by default after 3 invalid inputs

The IVR Announcement script

```
Welcome to Zergaw Call center.
To talk to customer support representative, press 1
To talk to sales representative, press 2
To talk to reception, press 0
```

- pressing 1 will route the call to `CS-Queue`
- pressing 2 will route the call to `Sales-Queue`
- pressing 3 will route the call to `Reception-Queue`

### Non Office Hour IVR

We also have an IVR called `Alt IVR` with

- 30 as a timeout(this means if a user doesnt interact with the ivr within 30 second of the IVR announcement it will time)
- with 0 invalid retries
  - this happens if a user input a wrong number that is not on the IVR
  - it will announce invalid retry recording
  - it will append announcement on invalid(this means it will reannounce the IVR announcement)

The IVR Announcement script

```
Thank you for calling zergaw internet and cloud service provider,
Our office is currently closed. Our regular business hours are from Monday to Saturday, 8.30am to 5.30pm eastern standard time. If you'd like to leave a message, please press 1. For more information about our products and services, please visit our website at www.zergaw.com. Thank you for calling.Thank you for calling zergaw internet and cloud service provider,
Our office is currently closed. Our regular business hours are from Monday to Saturday, 8.30am to 5.30pm eastern standard time. If you'd like to leave a message, please press 1. For more information about our products and services, please visit our website at www.zergaw.com. Thank you for calling.
```

- pressing 1 will route the call to general voicemail

> Queues

### CS-Queue

A queue with

- three customer support static agents
- rrmemory ring strategy
- autofill turned on
- skip busy agents set to yes + (ringinuse=no)
- a join announcement
- announce possition set to yes and
- frequency set to 15 sec

```
One of our customer support representatives will be with you shortly
The conversation is being recorded for training and quality purposes.
```

- join empty set to strict
- leave empty set to strict
- and a failover destnation of misc destination to call external cell number

### Sales-Queue

A queue with

- all the parameters will be the same with the CS-Queue except
- a join anouncement of

```
One of our customer support representatives will be with you shortly
The conversation is being recorded for training and quality purposes.
```

### Reception-Queue

A queue with

- all the parameters will be the same with the CS-Queue except
- a join anouncement of

```
A reception will be with you shortly
The conversation is being recorded for training and quality purposes.
```

### Agentn-Queue

The same as the rest except with a ring strategy to ringall

> What is new on the current system

- Agent Console
  The newly integrated agent console that come with issabel allows - to display scripts right from their login, such as Greeting script, FAQs ... - allow us to have a form input from the agent panel for agents to record information of live calls - inaddition it allows the agent to take a break, this makes calls not to be routed to the extension - transfer and hangup calls right from the agent panel

- CID specific call routing
  calls from known customers with specific CID will be routed to an agent that handles their calls always

- An IVR
  this allows users to navigate to their desired destination without needing an assistant, in addition this allows zergaw to route calls to the intended destination right away

- Call Recording

- CDR
  A detail CDR reports such as agent break reports, calls detail report. calls per hour, calls per agent, hold time, login logout report, ingoing call success, agents monitoring, campain monitoring and much more

> SIP TRUNK Configuration

SIP Range : 111133621 - 111133670

Configuration

- Go to PBX > PBX Configuration > Trunks > Add Trunk
  - Trunk Name: Ethiotelecom_trunk_0111133621
  - Peer Details:
  ```
  username=0111133621
  type=friend
  secret=111133621
  qualify=yes
  nat=yes
  keepalive=45
  insecure=invite,port
  host=10.208.233.128
  fromuser=0111133621
  fromdomain=10.208.233.128
  dtmfmode=rfc2833
  disallow=all
  directmedia=no
  context=from-trunk
  canreinvite=no
  allow=ulaw&alaw
  ```
  - Register String: 0111133621:111133621@10.208.233.128
- ref: https://wiki.merionet.ru/asterisk-calculator/?lang=ENG

# Error and Troubleshooting
- Got 423 Interval too brief for service
  - go to pbx configuration > advanced sip settings
    - change Registration timers for all min, max and default to 3600
- After a new pbx setup, calls comming in from trunk are rejected automatically, to resolve this you need to
  - go to advanced sip setting and enable Allow SIP Guests and Allow Anonymous Inbound SIP Calls
- If you are having two ips, one public and on private on the pbx, call comming in from outside are not properly routed, and looking into sngrep you can see the extenstion is sending rtp to the servers public ip and your pbx server is advertizing the wrong ip(the public ip) to your trunk provider to fix this on setups that have both public and private ips where extensions connect through public ip and trunk connection goes through private ip you need to go to advanced sip setting and set ip configurations to public ip instead of static ip
