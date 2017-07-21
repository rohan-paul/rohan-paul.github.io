---
layout: post
title: How to install MongoDB on Ubuntu 15.04 and 15.10
comments: true
author: Rohan Paul
categories: MongoDB_in_Ubuntu
---
<img src="/images/fulls/mongodb.jpg" class="fit image">

In this article I shall go through the steps for installing MongoDB ( a open source, very popular NoSQL database, widely used in web-applications) in Ubuntu 15.04 and also some common issues.


First to note that Mongoddb's official [Ubuntu installation page](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/) says that currently it only provides packages for 64-bit long-term Ubuntu releases. That is 12.04 LTS (Precise Pangolin) and 14.04 LTS (Trusty Tahr). There is currently no build available for Ubuntu 15.04. Check [this page for all builds](https://www.mongodb.org/downloads#development)

Hence, when installing on Ubuntu 15.04 (Vivid) following the exact process and commands of the above [MongoDB's official page](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/) will not work. Hence the below steps that worked in my Ubuntu 15.04


##First to completely remove an existing Mongodb from your machine if you have that in already

If you have already followed Mongoddb's official [Ubuntu installation page](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/) and installed it, and then post installation its not working in your machine; e.g. the commands ``sudo service mongod start`` OR ``sudo mongod`` is not starting the service - then first remove this existing version completely before installing a new one. Run the below commands in sequence.


``sudo apt-get purge mongodb-org``

``sudo apt-get autoremove``

And now delete the old mongodb.list that was created.

``sudo rm /etc/apt/sources.list.d/mongodb.list``


##Installing a fresh MongoDB  into Ubuntu 15.04 from debian wheezy repository

**Step-A**

First Issue the following command to import the MongoDB public GPG Key:

``sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10``


Now as advised in this [jira issue page](https://jira.mongodb.org/browse/SERVER-17742) create the /etc/apt/sources.list.d/mongodb-org-3.0.list list-file to get the package from debian wheezy repository

``echo "deb http://repo.mongodb.org/apt/debian wheezy/mongodb-org/3.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list``


Reload the local package database

``sudo apt-get update``


And now install the latest stable version of mongodb ( If you want to install a specific version see the [official page](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/#install-the-latest-stable-version-of-mongodb).

``sudo apt-get install -y mongodb-org``


Now you can check the installation version by running ``mongod --version`` which at the time of this writing (3-Sep-2015) returned in my Terminal ``"db version v3.0.6"`` (i.e. the latest stable version)


**Step-B**

However, after all the steps above and mongodb installed in our machine, now if you try to start the mongodb service with ``sudo service mongod start`` it will fail. You may get various reasons for failure in the Terminal. For example I first got

 ``"start: Unable to connect to Upstart: Failed to connect to socket /com/ubuntu/upstart: Connection refused”``

 Then I also got

 ``Failed to start mongod.service: Unit mongod.service failed to load: No such file or directory.``

 You probably will be able to manually start it with ``sudo mongod -f /etc/mongod.conf`` - but in this way MongoDB will remain connected as long as that terminal is opened.


**So here is how we are going to resolve and startup mongodb normally**


+ First we are going to change MongoDB's default data store files from **/var/lib/mongodb** to **/data/db**

+ So, first create a folder /data/db in your machine. Run ``sudo mkdir -p /data/db``  

+ Now open the main mongo configuration file with ``sudo gedit /etc/mongod.conf`` and change the “dbpath” line as below

+ Replace **dbpath=/var/lib/mongodb**   TO   **dbpath=/data/db** and then save the file.

+ Then delete the old default /var/lib/mongodb

+ But the directory you created doesn't have the correct permissions and ownership right after creation -- it needs to be writable by the user who runs the MongoDB process. Hence we must make all the directories/files owned by mongod user

+ Run ``sudo chown -R mongodb:mongodb /data/db``


**And now finally you can start mongo with ``sudo service mongod start``**

And chcek that the service is running with ``sudo systemctl status mongod``  - It should show a message similar to below

``Loaded: loaded (/etc/init.d/mongod)``

``Active: active (running) since Thu 2015-09-03 04:57:49 IST; 7s ago``

Another way to verify that the mongod process has started successfully is by checking the contents of the log file at /var/log/mongodb/mongod.log for a line reading

``[initandlisten] waiting for connections on port <port>``

where **``port``** is the port configured in /etc/mongod.conf, 27017 by default. So for example, in my machine this line was the below one.

``2015-09-02T08:53:15.156+0530 I NETWORK  [initandlisten] waiting for connections on port 27017``

And to stop or restart MongoDB run ``sudo service mongod stop`` and ``sudo service mongod restart`` respectively.


### Other small issue that you might face (as it came up in my case)

**Issue-A>** After running ``sudo service mongod start`` it failed giving the below erroor.

``couldn't unlink socket file /tmp/mongodb-27017.sockerrno:1 Operation not permitted skipping``

**Solution** - Just delete the file .sock file with
``sudo rm /tmp/mongodb-27017.sock``

This was probably because, I started things up before and didn't shutdown correctly.

## Update in Feb-2016: Installing Mongodb in Ubuntu 15.10

**Step-A**

From [Ubuntu's official package page](http://www.ubuntuupdates.org/package/core/wily/universe/base/mongodb) install the mongodb version 2.6.10 (at this point, this is the latest MongoDB version for which Ubuntu 15.10 (wily) has an official Mongodb package) through Software Center.

Post installation, check the version number with ``mongod --version``

**Step-B**

Create a systemd startup file in -  /lib/systemd/system/mongodb.service with the following content -

```
[Unit]
Description=High-performance, schema-free document-oriented database
Documentation=man:mongod(1)
After=network.target

[Service]
Type=forking
User=mongodb
Group=mongodb
RuntimeDirectory=mongod
PIDFile=/var/run/mongod/mongod.pid
ExecStart=/usr/bin/mongod -f /etc/mongod.conf --pidfilepath /var/run/mongod/mongod.pid --fork
TimeoutStopSec=5
KillMode=mixed

[Install]
WantedBy=multi-user.target
```

Now enable this mongodb.service file and start it with the following command

``sudo systemctl enable mongodb.service``


**Step-C**

Create a folder /data/db in your machine; i.e. run ``sudo mkdir -p /data/db``

And then take ownership of the /data/db directory by running - ``sudo chown -R mongodb /data/db``

Now open the mongodb configuration file with ``sudo gedit /etc/mongod.conf`` and change the “dbpath” line as below

Replace dbpath: /var/lib/mongodb TO dbpath: /data/db and then save the file.

**Step-D**

Now running ``sudo service mongodb start`` should start mongodb normally.

To check if mongodb started by open the log file at /var/log/mongodb/mongodb.log. The log file's last line should show something like :-

“[initandlisten] waiting for connections on port 27017”

**Other possible issues in installing Mongodb in Ubuntu 15.10**

**1.** After doing all the above, if you still have some problem in starting mongodb with ``sudo service mongodb start`` try to understand some details on the source of the problem by running ``sudo systemctl status mongod.service`` in the terminal. If this gives some error with respect to starting the database then check if the file /data/db/mongod.lock exists. If it does, remove it and execute ``mongod --repair`` and then again run ``sudo service mongodb start``




Thanks for taking time to read this post and if you enjoyed, follow me on **[Twitter](https://twitter.com/paulr_rohan)** for future updates.
