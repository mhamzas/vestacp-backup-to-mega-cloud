﻿# VestaCP: uploading backups to the MEGA cloud [![](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme?gtid=UA-77529928-1&mr)](https://ga-spider.appspot.com)

(*v-sync-backups-to-mega - script for syncing backups with Mega cloud*)

This script allows to automate uploading backups to the **MEGA** cloud, which provides **50 GiB** GiB of the cloud space for free (and provides paid plans as well).

The script is a breeze to use. You can easily install it on your own server (with the help of some little ingenuity, and if you are comfortable with the command line and **VestaCP** control panel). Let's go ahead and dive into the world of automated cloud backups!

[![](https://img.shields.io/badge/LICENSE-MIT-blue.svg?style=flat-square)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme?gtid=UA-77529928-1&mr&mgo&go=https://git.io/vrxu8) [![](https://img.shields.io/badge/README-RU-green.svg?style=flat-square)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme?gtid=UA-77529928-1&mr&mgo&go=https://git.io/voGJf) [![](https://img.shields.io/badge/RELEASE-V2.0-blue.svg?style=flat-square)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme?gtid=UA-77529928-1&mr&mgo&go=https://github.com/By-Vasiliy/VestaCP-Sync-Backups-To-Mega/releases/tag/v2.0)

# Dependencies 

(*Pre-installation of the packages required for the script*)

First, you need to install [MegaTools](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme?gtid=UA-77529928-1&mr&mgo&go=https://megatools.megous.com).

The autoinstall script for Linux Ubuntu 14.04: [install-megatools.sh](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme?gtid=UA-77529928-1&mr&mgo&go=https://git.io/vrq6v) (Where can you find it? Look up for the files in the git repository!).

*MegaTools and the script (v-sync-backups-to-mega) can be installed on any Linux OS, but my  instructions are written for Ubuntu 14.04 and have not been tested under other systems. So, if you want to try it yourself and the outcome is successful, you are welcome to contribute to this guide.* 

 **To check, if MegaTools installed successfully:** run the command ***megareg*** under any user, and it the outcome is like this:

```
ERROR: You must specify either --register or --verify option
```

..then everything is ok. while if it's like this:

```
megareg: command not found
```

then **MegaTools** installation failed and you should look for the problem (but normally everything goes well).


# Installation 

(*Script installation and customization*)

Next, place the script (v-sync-backups-to-mega) in the **/usr/local/vesta/bin/** folder and make the following small changes to the script.
 
Find the following line in the script file:

```
CLOUD_BACKUPS_DIR="DEFAULT_DIR"; #Name the backup directory in the MEGA cloud
```
and replace *DEFAULT_DIR* with the folder name of your choice. The script will automatically create a folder in the cloud with the same name (if it was absent). For instance: “vps1”, but something clear is better, and the name should be without spaces). The script will use this folder upload your backups.

Then run the following commands:

```
chmod 770 /usr/local/vesta/bin/v-sync-backups-to-mega
chown root:root /usr/local/vesta/bin/v-sync-backups-to-mega
```
Next, create a file **.megarc** in the **root** home directory (/root/.megarc), containing the following information:

```
[Login]
Username = Your_Mega_Username
Password = Your_Mega_Password
```
**Check the result:** on behalf of **root** run the command ***megals*** (or you can just run the command ***sudo megals*** #Linux Ubuntu/Debian). It should print something like this:

```
/Contacts
/Inbox
/Root
/Trash
```
If the command gives back an error, then the config file **.megarc** was filled with the wrong data, or you ran the command not on the behalf of **root**.

# CRON job 

(*CRON job — scheduled synchronization of the backups*)

Now let's use Vesta control panel to create a cron job with the command:

```
sudo /usr/local/vesta/bin/v-sync-backups-to-mega $1 $2
```
**$1** - The integer number of backup copies for 1 user, stored in the cloud. When this limit is exceeded, the oldest backup is deleted. The default number of copies stored is 21. If you have few users in the panel and the backup size does not exceed 500-800 MiB, this number of copies is quite reasonable. But it's highly recommended to estimate the number of backups you need for every particular case and check the cloud regularly.
 
**$2** – The maximum number of lines in the log file. Once this value is exceeded, the log gets cleared, leaving only a message about the cleanup (by default, this value is equal to 200,000 lines ~ 50 MiB).

# CRON example

![VestaCP script for uploading backups to the MEGA cloud - Cron example](http://i.imgur.com/CBt1lfx.png)

*The image shows an example of CRON job (every day at 7:15 a.m. the backup will synchronize with the cloud, the maximum number of copies - 21, and the maximum number of lines in the log file is 200000 ~ 50 MiB).*

# Log file 

(*The log file helps to always be aware of what's going on with your backups synchronization*)

The log file records to the default folder **/var/log/**, with it's name generated by formula: **navaneeta.log (/var/log/script_name.log)**. Log file by default:

```
/var/log/v-sync-backups-to-mega.log
```

# Support the developer

(*Without Donat - hard*)

***WebMoney***
* Z347790264030
* E175788944874
* X060454244628
* R808133397823
* G149817486110
* U448946356404
* K200344065989

# License

The software is distributed under the [MIT](https://git.io/vrxu8) license.

# Copyright (c) 2016 Vasilyuk Vasiliy