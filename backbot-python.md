DB - Backup Scripts
==================

A python module written in python3 to take Full Backup/Recovery of particular database to/from remote location (like S3, FTP or Glacier).

Synopsis
---------

To take full backup & recovery on following DB servers
- MySQL server
- Mongodb server
- PostgreSQL server
- Redis server

Requirements
------------
- **OS**
    - Ubuntu 18.04 (Debian)

- **Packages**
    - python3.7
    - pip3
    - python3-virtualenv


Prerequisite
-------

**1. Install Database server clients on your system**

```
# Install mysql, mongodb, postgres & redis clients
$ sudo apt-get install -y \
                        mysql-client        \
                        mongodb-clients     \
                        postgresql-client   \
                        redis-tools

# Installing redis-dump to export redis data
$ sudo apt-get -q install -y npm
$ sudo npm install redis-dump -g
```

"README.md" 123L, 2324C                                                                                       1,1           Top
DB - Backup Scripts
==================


Synopsis
---------

To take full backup & recovery on following DB servers
- MySQL server
- Mongodb server
- PostgreSQL server
- Redis server

Requirements
------------
- **OS**
    - Ubuntu 18.04 (Debian)

- **Packages**
    - python3.7
    - pip3
    - python3-virtualenv


Prerequisite
-------

**1. Install Database server clients on your system**

```
# Install mysql, mongodb, postgres & redis clients
$ sudo apt-get install -y \
                        mysql-client        \
                        mongodb-clients     \
                        postgresql-client   \
                        redis-tools

# Installing redis-dump to export redis data
$ sudo apt-get -q install -y npm
$ sudo npm install redis-dump -g
```

**2. Install python dependencies**
```
# Goto project dir and create a virtual environment
$ cd /path/to/project-repo/
$ python3 -m venv venv

# Activate python virtual env
$ source venv/bin/activate

# Install requirements
$ pip install --upgrade pip setuptools wheel
$ pip install -r requirements.txt
```

Backup
----

Configure your ENV vars
```
cp .env.example .env
```

Edit .env file (for example )
```
# Database configs for incremental backup
APP_NAME=GOBLIN
DB_HOST=192.168.0.100
DB_PORT=27017
DB_USER=BACKUPUSER
DB_PASSWORD=##########
DB_NAME=emp_db
DB_TYPE=mongodb

# S3 configs
S3_KEY=################
S3_SECRET=#########################

```

Now run following command to take backup

```
python3 main.py backup
```

Restore
----
Configure your ENV vars
```
cp .env.example .env
```

Now restore
```
python3 main.py restore
```

----

Backup/Restore using Docker
----

**1. Install Docker**
- https://docs.docker.com/engine/install/ubuntu/

**2. Build docker image**
```
docker build . -t db_backups:v1

```
**3. Update your .env file & Run docker image instance**
```
# for backup
docker run --rm --env-file .env --volume `pwd`/backup-data:/opt/backup --volume `pwd`/config.yml:config.yml db_backups:v1 backup

# for restore
docker run --rm --env-file .env --volume `pwd`/backup-data:/opt/backup --volume `pwd`/config.yml:config.yml db_backups:v1 restore
```
