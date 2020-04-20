# DDoSDB-in-a-box
DDoSDB system embedded in a virtual machine 


## First steps:

 1. Download the Virtual Machine [here] 
 2. Run the Virtual Machine using the software Virtual Box
 4. Connect to the IP using your browser: http://IP/
 5. Generate fingerprints using  `Dissector`
 6. List the fingerprints generated on Web Interface

 
 ### Access the Web Interface
---
The VM has all the services running and ready to go. You should be able to connect to the Web server simply using the IP address of the VM in our browser.

**Note**: The Virtual Machine is configured to get IP address from DHCP (bridge mode). You can define the IP address manually, if you prefer. Be sure you can reach it h host system. 

Use the following credentials to access the Web Interface. You can add more users following the instructions.

| user | pass |
|--|--|
|  ddosdb| 071739440782b7c6581241607acca8b7 |

![WEB interface](https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/ddosdb.gif?s=200) 

<img src="https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/ddosdb.gif" width="50%" height="50%">

### Web interface configuration
---

##### Users and Permissions (Django)
 Web interface user adminstration [add new users and grant  permissions]
 - `htttp:/IP/admin`
 
##### Site Configuration

Directory structure:
  - DDoSDB root directory: `/opt/`
  - DDDoSDB website settings (django):  `/opt/ddosdb/website/`
  
  configuration file: `/opt/ddosdb/website/settings_local.py`
  **main** directives: 
```shell
# Which hosts are allowed to access the Web interface
# ALLOWED_HOSTS = ['ddosdb.org', 'localhost', '127.0.0.1']
# This allows all hosts to connect to the Web interface
ALLOWED_HOSTS = ['*']

# Raw path to fingerprint and attack vector data
# pcap and json are stored here
RAW_PATH = "/opt/ddosdb-data/"

# Location where HTML are stored
STATIC_ROOT = '/opt/ddosdb-static/'

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'ddosdb',
        'USER': 'ddosdb',
        'PASSWORD': 'ddosdb',
        'HOST': 'localhost'
    }
}
```

### Running DDoS Dissector
---

Dissector is reponsable for process a `.pcap` file and extract its characteristics. We provided a set of DDoS attacks in ``.pcap``format in the repository. Follow the steps bellow to process your first file.

 1. Download the last version of software
`git clone https://github.com/ddos-clearing-house/ddos_dissector`
 2. Use the provided pcap samples to generate fingerprints and update to the repository
 

![Dissector interface](https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/dissector.gif){:height="50%" width="50%"}



