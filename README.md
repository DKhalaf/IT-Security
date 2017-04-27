# IT-Security

## Week 16 - Workpack A [Set up graphite on apache2 using Ubuntu Server v. 14.04.5]
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-graphite-on-an-ubuntu-14-04-server

# Install Ubuntu Server 14.04.05 in VMware
* https://www.ubuntu.com/download/alternative-downloads

# --------------------------------------------------------------------
# --------------------------------------------------------------------
# --------------------------------------------------------------------

# Install Graphite
1) `sudo apt-get update`
2) `sudo apt-get install graphite-web graphite-carbon`

### Configure a Database for Django
# Install PostgreSQL Components
3) `sudo apt-get install postgresql libpq-dev python-psycopg2`

### Create a Database user and a Database

4) `sudo -u postgres psql`

# Create a user
5) `CREATE USER graphite WITH PASSWORD 'password';`
# Create a database
6) `CREATE DATABASE graphite WITH OWNER graphite;`
# Exit out of PostgreSQL session
7) `\q`

### Open Graphite web app configuration file:
8) `sudo nano /etc/graphite/local_settings.py`

# Inside the file, edit following:
9) * SECRET_KEY = 'a_salty_string'
10) * TIME_ZONE = 'Europe/Copenhagen'
11) * USE_REMOTE_USER_AUTHENTICATION = True

# In the same file, type in your database information:
![databases](https://cloud.githubusercontent.com/assets/23449056/25485387/0db0b204-2b5e-11e7-87a3-ac050c493ed3.PNG)
# Sync the Database
13) `sudo graphite-manage syncdb`

# Enable Carbon service
14) `sudo nano /etc/default/graphite-carbon`

# Inside the file, edit: This will start the service on boot
15) * CARBON_CACHE_ENABLED=true

# Open the configuration file
16) `sudo nano /etc/carbon/carbon.conf`

# Edit the file:
* ENABLE_LOGROTATION = True

### Configure Storage Schemas
17) `sudo nano /etc/carbon/storage-schemas.conf`

# Add following before the "[default_1min_for_1day]" section
* [test]
* pattern = ^test\.
* retentions = 10s:10m,1m:1h,10m:1d

# Copy the file into another directory
18) `sudo cp /usr/share/doc/graphite-carbon/examples/storage-aggregation.conf.example /etc/carbon/storage-aggregation.conf`

# Start Carbon
19) `sudo service carbon-cache start`

### Install and Configure Apache
20) `sudo apt-get install apache2 libapache2-mod-wsgi`
# Disable the default virtual host since it will conflict with our new file
21) `sudo a2dissite 000-default`
# Copy the Graphite Apache virtual host file into the available sites directory
22) `sudo cp /usr/share/graphite-web/apache2-graphite.conf /etc/apache2/sites-available`
# Enable the virtual host
23) `sudo a2ensite apache2-graphite`
# Reload the service to implement the changes
24) `sudo service apache2 reload`


### Verification: Go to your ubuntu client in VMWare, and type:
* http://ServerIP
![graph](https://cloud.githubusercontent.com/assets/23449056/25485381/08cc03ce-2b5e-11e7-887f-a3ace23e0009.PNG)


# --------------------------------------------------------------------
# --------------------------------------------------------------------
# --------------------------------------------------------------------
# You can send some echo request from the servers terminal to view changes in your graphs.
# You can also specify on the graphs, a timeline for monitoring data

