---
title: "Auto Backup OpenShift Database to Dropbox"
date: 2017-05-06 1:53:06
description: "Flexible way to backup redhat openshift database to the cloud"
---

Due to privilieges restriction ([Openshift](https://www.openshift.com/) free Plan), you'll certainly find difficulties to backup your database using the well known gems, plugins available in Github such as [Backup Gem](https://github.com/backup/backup). Since majoriy of these tools require write/read permissions to create and load configuration files.

<span style="color:#4ECDC4">This tutorial is generic and applies for any kind of Web app (Ruby on Rails, Django, Laravel ...)</span> 

### My Stack
- Rails 4.1.4
- Ruby 2.0
- PostgreSQL 9.2
- Cron 1.4 <span style="color:#E26A6A">(Make sure to add this cartridge to your app)</span>

<br />

### Steps

##### 1 - Create new Dropbox App and get the token

* Create A new Dropbox App Through [Dropbox Developers](https://www.dropbox.com/developers/apps/create)
* Select `Dropbox API` -> `App Folder` then name your app
* Once created, Go to `Generated access token` section and click generate
* Now you got the token !

<br />

##### 2 - Setup Dropbox Shell Uploader
SSH into your Openshift Application then run the following commands.

~~~
# It goes to App data folder then downloads the dropbox uploader shell
cd $OPENSHIFT_DATA_DIR && curl "https://gist.githubusercontent.com/medyo/e0c6e46ca31b995eb46f119af914f2eb/raw/ecf61975054b369b12f22cfe7d46855e30df8855/dropbox_uploader.sh" -o dropbox_uploader.sh
~~~

~~~
# It makes the script executable
chmod +x dropbox_uploader.sh
~~~

~~~
# it creates new file containing:
echo "OAUTH_ACCESS_TOKEN=PUT_YOUR_ACCESS_TOKEN_HERE" > dropbox_uploader
~~~


##### 3 - Run first test
~~~
# Create a test file to upload
echo "Hello World !" > demo.txt

# Upload file to dropbox
./dropbox_uploader.sh upload demo.txt hello.txt
~~~

<span style="color:#4ECDC4">If you receive like this message:</span>
> Uploading "/var/lib/openshift/ID/app-root/data/demo.txt" to "/hello.txt"... DONE

<span style="color:#4ECDC4">Everything is Okay, if not recheck your access token.</span> 

##### 3 - Automate the backup & Upload

In your local enviroment, open your app folder then locate the `.openshift` folder, go to `cron` folder and at that point, select the most suitable scheduling time for your cron task. For my case, I select `daily`, I do backups each day.

<img src="{{ site.baseurl }}assets/images/openshift-cron.png" alt="File Uploaded to Dropbox" width="40%" align="center">


create a new file file `auto_backup.sh` with the following content, so the Openshift cron module can schedule its execution.

Don't forget to push the changes to your app

<script src="https://gist.github.com/medyo/638bc1be401ec9d1d2f35eefdc26be41.js"></script>

:thumbsup:  <span style="color:#4ECDC4">Backup successfully uploaded to Dropbox :)</span> 
<img src="{{ site.baseurl }}assets/images/dropbox-openshift-database.png" alt="File Uploaded to Dropbox" width="70%" align="center">


##### That's it :)


For Mysql Database (Not Tested, source: [https://gist.github.com/nicdoye/4697265](https://gist.github.com/nicdoye/4697265))

~~~
# MYSQL
mysqldump -h $OPENSHIFT_MYSQL_DB_HOST -P ${OPENSHIFT_MYSQL_DB_PORT:-3306} -u ${OPENSHIFT_MYSQL_DB_USERNAME:-'admin'} --password="$OPENSHIFT_MYSQL_DB_PASSWORD" --all-databases  > $FILENAME
~~~

