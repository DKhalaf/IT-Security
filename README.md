# IT-Security

### Week 16 - Workpack A [Set up graphite on apache2 using Ubuntu Server v. 14.04.5]
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-graphite-on-an-ubuntu-14-04-server

# Install Ubuntu Server 14.04.05 in VMware
# Install Graphite
1) `sudo apt-get update`
2) `sudo apt-get install graphite-web graphite-carbon`

## Configure a Database for Django
# Install PostgreSQL Components
3) `sudo apt-get install postgresql libpq-dev python-psycopg2`

# Create a Database user and a Database

4) `sudo -u postgres psql`

* Create a user
5) `CREATE USER graphite WITH PASSWORD 'password';`
* Create a database
6) `CREATE DATABASE graphite WITH OWNER graphite;`
* Exit out of PostgreSQL session
7) `\q`

# Open Graphite web app configuration file:
8) `sudo nano /etc/graphite/local_settings.py`

# Inside the file, edit following:
9) * SECRET_KEY = 'a_salty_string'
10) * TIME_ZONE = 'Europe/Copenhagen'
11) * USE_REMOTE_USER_AUTHENTICATION = True
12) * DATABASES = {
    'default': {
        'NAME': 'graphite',
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'USER': 'graphite',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': ''
    }
}

# Sync the Database
13) `sudo graphite-manage syncdb`

# Configure Carbon
14) `sudo nano /etc/default/graphite-carbon`

# Inside the file, edit:
* CARBON_CACHE_ENABLED=true
