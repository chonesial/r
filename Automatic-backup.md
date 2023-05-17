# For Automatic Back up of Postgres Database in defined Time over s3 bucket 
#### Requirements 
- Postgres
- Text Editor
- Access keys from AWS


## Steps to Follow 

1.### Changing the Postgresql Configuration files 

Go to Postgres.conf 

``` 

vi /etc/postgresql/14/main/postgres.conf

```

#### Change the listening option to "*" which means listen to all 
```

listen_addresses = '*'         # what IP address(es) to listen on;

```

Now , Open pg_hba.conf

```
vi pg_hba.conf
```
here change the connection settings for all the local user in order to use password
change peer to md5 

```

# "local" is for Unix domain socket connections only
local   all             all                                     md5

```
#### Now After the configuration , we can move forward for creating User along with a password and a database 

2. ### AWS Configuration 

   Install AWS ClI

```

apt instal awscli

```

-----> Go to your AWS Dashboard and create access keys and secret file <-----

here , on terminal write AWS configure 

```
aws configure 

```

set your credentials and preferences 


```
root@ip-192-168-111-213:/etc/postgresql/14/main# aws configure
AWS Access Key ID [****************63VE]:
AWS Secret Access Key [****************DRCB]:
Default region name [ap-south-1]:
Default output format [text]:
```

#### Now AWS is configured

3.### Set up S3 Bucket and a Backup directory inside 

in this case my s3 Bucket name is "teraatestsun"
and 
my directory is "backuppost"

4.### Now create a shell script that will 
          - First,  login inside Postgres as user 
          - Secondly, Create a Database Dump file inside defined directory 
          - Thirdly, Upload the Database Dump file to the S3 Bucket 
          - Fourthly , Delete Temporary files 
Shell Script Goes like 

```

USER="backupuser"
PASSWORD="password"  # Set your desired password here

HOST="localhost"
DB_NAME="car"

BACKUPROOT="/tmp/backups"
S3_DIRECTORY="backuppost"
S3_BUCKET="s3://teraatestsun"
TIMESTAMP=$(date +"%d-%b-%Y-%H-%M-%S")

# Dump the database
start=$(date +%s)
PGPASSWORD="$PASSWORD" pg_dump -U $USER -Fc $DB_NAME > $BACKUPROOT/$DB_NAME-$TIMESTAMP.SQL
end=$(date +%s)
backup_runtime=$((end-start))

# Upload the database to S3
start=$(date +%s)
aws s3 sync $BACKUPROOT/ $S3_BUCKET/$S3_DIRECTORY/$TIMESTAMP/
end=$(date +%s)
upload_runtime=$((end-start))

# Cleanup temporary files
rm -rf $BACKUPROOT/*


```


5.### In order , to Automatically Upload the database backup and regulate the time interval we can use cron job scheduling 

```

crontab -e

```

and in we can regulate as per our requirement , 
As i have used in every five minutes 

```

*/5 * * * *  /bin/bash /home/ubuntu/backup.sh >> /home/ubuntu/logfile.log 2>&1

```


The output on s3 bucket will be the timestamps at which the Database Dump was Created 

# Output on s3 bucket 
```

Name          Type          Last modified         Size          Storage class



17-May-2023-19-05-01/
Folder
-
-
-
17-May-2023-19-00-01/
Folder
-
-
-



17-May-2023-18-55-01/
Folder
-
-
-



17-May-2023-18-50-01/
Folder
-
-
-



17-May-2023-18-45-01/
Folder
-
-
-



17-May-2023-18-32-36/
Folder
-
-
-



17-May-2023-18-30-38/
Folder
-
````
