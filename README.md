# dddosdb-in-a-box
DDoSDB system embedded in a virtual machine 



## Directory structure:

  - DDoSDB root directory: /opt/
  - DDDoSDB website settings (django): /opt/ddosdb/website/
  
###
###


```shell

# Which hosts are allowed
#ALLOWED_HOSTS = ['ddosdb.org', 'localhost', '127.0.0.1']
ALLOWED_HOSTS = ['*']
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
