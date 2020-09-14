 <p align="center"><img width=30.5% src="https://github.com/ddos-clearing-house/ddos_dissector/blob/3.0/media/header.png?raw=true"></p>

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![Python](https://img.shields.io/badge/python-v3.6+-blue.svg)
![GitHub issues](https://img.shields.io/github/issues/ddos-clearing-house/dddosdb-in-a-box)(https://github.com/ddos-clearing-house/dddosdb-in-a-box/issues)
![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/ddos-clearing-house/dddosdb-in-a-box)
 
 ## Overview

 DDoSDB system embedded in a virtual machine. As depicted, the system has 3 components: dissector, database, and converter.




<img src="https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/box-p2p-zoom.png">

## First steps:

 1. Download the Virtual Machine <a href="https://surfdrive.surf.nl/files/index.php/s/e0U0VoMqiiH1ae1/download?path=%2F&files=ddosdb-in-a-box%20new.ova">
 <img src="https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/download.png" width="15%" height="15%"  border=1></a>
 2. Run the Virtual Machine using the software Virtual Box
 4. Connect to the IP using your browser: http://IP/
 5. Generate fingerprints using  `Dissector`
 6. List the fingerprints generated on Web Interface

[![](https://mermaid.ink/img/eyJjb2RlIjoiZ3JhcGggTFJcbiAgQltEaXNzZWN0b3JdIC0tPnx1cGxvYWR8IENbRERvU0RCXVxuICBzdWJncmFwaCBERG9TREJcbiAgQyAtLT58IHwgRFtXZWIgSW50ZXJmYWNlXVxuICBDIC0tPnwgfCBFW0RhdGFiYXNlXSAgXG4gIGVuZFxuXG5cdFx0IiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggTFJcbiAgQltEaXNzZWN0b3JdIC0tPnx1cGxvYWR8IENbRERvU0RCXVxuICBzdWJncmFwaCBERG9TREJcbiAgQyAtLT58IHwgRFtXZWIgSW50ZXJmYWNlXVxuICBDIC0tPnwgfCBFW0RhdGFiYXNlXSAgXG4gIGVuZFxuXG5cdFx0IiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)

### Virtual Machine Credentials
---

| user | pass |
|--|--|
|  ddosdb| ddosdbddosdb|
|  root| givemeroot!|


 ### Access the Web Interface
---
The VM has all the services running and ready to go. You should be able to connect to the Web server simply using the IP address of the VM in your browser.

**Note**: The Virtual Machine is configured to get IP address from DHCP (bridge mode). You can define the IP address manually, if you prefer. Be sure you can reach it from the host system. 

Use the following credentials to access the Web Interface. You can add more users following the instructions.

| user | pass | type |
|--|--|--|
|  ddosdb| 071739440782b7c6581241607acca8b7 | admin user (for adding other users) | 
| upload | uploadupload | user with upload rights (for dissector) |

<img src="https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/ddosdb.gif" width="60%" height="60%"  border=1>



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
```bash
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
 
 <img src="https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/dissector.gif" width="60%" height="60%" border="1px">

```shell
python3 ddos_dissector_cli.py --input ./../pcap_samples/sample2.pcap --log /tmp/log.txt
```
### Configuration Files for Dissector

- filename: /home/ddosdb/ddos_dissector/src/settings.py
```shell
POOL_SIZE = 4

# IP address used to submit the fingerprints
DDOSDB_URL = "http://10.0.0.10/"

# Username for DDoSDB for uploading the attack vector and fingerprint
USERNAME = "ddosdb"
# Password for DDoSDB for uploading the attack vector and fingerprint
PASSWORD = "071739440782b7c6581241607acca8b7"
```
### Converter

The converter translates the processed fingerprints to mitigation rules. The system provides one converter `convert_iptables` that converts fingerprints to firewall IPtables. 

The following animation shows the process.


<img src="https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/converter.gif" width="60%" height="60%"  border=1>




