---
title: DLNA Media Server on Armbian Linux for Single Board Computer
description: Your DLNA Media Server on Armbian Linux for Single Board Computer
header: DLNA Media Server on Armbian Linux for Single Board Computer
---

DNLA stands for Digital Living Network Alliance and is a trade organization that set guidelines for sharing content among network devices. It includes sharing media like music, movie and photo. MiniDLNA is a server software intended to be compatible with DLNA / UPnP-AV clients. MiniDLNA is a simple and lightweight alternative for your home server file, but it does not have a web interface. Its configuration have to be done using the command line interface (CLI).  

You can install MiniDLNA in any Linux operanting system. In this post I have installed MiniDLNA in an old tv box (model rk322x-box), wich is a very simple type of Single Board Computer [SBC], with armbian Linux. Here, in this post, we do not cover details about Armbian installation.
When the installation is over you can use VLC Media Player to access your files like music, video and picture.  

![tv-box-rk322x-box](/assets/images/rk322x-box.png)

Let's start with ssh to the Armbian and download the package:  

~~~ console
# sudo apt install minidlna
~~~

Open the file /etc/minidlna.conf and set up the lines like below:  

~~~ console
$ sudo vi /etc/minidlna.conf
~~~

~~~ console
# If you want to restrict a media_dir to a specific content type, you can
# prepend the directory name with a letter representing the type (A, P or V),
# followed by a comma, as so:
media_dir=A,/home/username/Music
media_dir=P,/home/username/Picture
media_dir=V,/home/username/Video

# Path to the directory that should hold the database and album art cache.
db_dir=/var/cache/minidlna

# Path to the directory that should hold the log file.
log_dir=/var/log/minidlna

# Name that the DLNA server presents to clients.
# Defaults to "hostname: username".
friendly_name=midiaserver
~~~

Next file to be edited is /etc/default/minidlna :  

~~~ console
$ sudo vi /etc/default/minidlna
~~~

~~~ console
# Path to the configuration file
CONFIGFILE="/etc/minidlna.conf"

# User and group the daemon should run as
# only for sysV init, for systemd please override minidlna.service
USER="root"
#GROUP="minidlna"
~~~

Let's set up a kernel parameter:  

~~~ console
# echo fs.inotify.max_user_watches=100000 > /etc/sysctl.d/10-max-user-watches.conf

# sysctl --system
~~~

Restart the service if running:  

~~~ console
$ sudo systemctl restart minidlna
~~~

Logs can be viewed at /var/log/minidlna :  

~~~ console
$ cd /var/log/minidlna

$ tail -f minidlna.log
[2022/03/01 22:02:32] minidlna.c:155: warn: received signal 15, good-bye
[2022/03/01 22:02:32] minidlna.c:1060: warn: Starting MiniDLNA version 1.2.1.
[2022/03/01 22:02:32] minidlna.c:1101: warn: HTTP listening on port 8200
[2022/03/01 22:02:32] playlist.c:135: warn: Parsing playlists...
[2022/03/01 22:02:32] playlist.c:269: warn: Finished parsing playlists.
~~~

Open up your browser and check out the content at http://YOUR-SERVER-IP:8200/  

For client you can choose to use VLC Media Player:

* Open up VLC Media Player.
* Click on "View" > "Playlist" [CTRL + L].
* On the left under "Local Network", click on "Universal Plug’n’Play".
* Your files may appear listed on the right. You can now play from network.



