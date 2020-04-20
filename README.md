# DDoSDB-in-a-box
DDoSDB system embedded in a virtual machine 


## First steps:

 1. Download the Virtual Machine [here] 
 2. Run the Virtual Machine using the Software Virtual Box
 3. Figure out the IP address of the Virtual Machine 
 4. Connect to the IP using your browser: http://IP/
 
Note: The Virtual Machine is configured to get IP address from DHCP (bridge mode). You can define the IP address manually, if you prefer. Be sure you can reach it h host system. 

### Access the Web Interface

| user | pass |
|--|--|
|  ddosdb| ddosdb |
<kbd>
![WEB interface](https://github.com/ddos-clearing-house/dddosdb-in-a-box/blob/master/imgs/webinterface.png)
</kbd>
 

 - Web interface user adminstration [add new users and grant  permissions]
 - `htttp:/IP/admin`

## Directory structure:

  - DDoSDB root directory: `/opt/`
  - DDDoSDB website settings (django):  `/opt/ddosdb/website/`
  
  configuration file: `/opt/ddosdb/website/settings_local.py`
  main directives: 
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
