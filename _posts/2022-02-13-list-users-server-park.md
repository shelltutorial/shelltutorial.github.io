---
title: List All Users Of Your *nix Server Park With A Script
description: List All Users Of Your *nix Server Park With A Script
header: List All Users Of Your *nix Server Park With A Script
---

If, for any reason like inventory, you need gathering information from your server park that includes a list of usernames maybe your problem finished since you arrived here. Check out the script below and see if it helps you. It was tested for Linux and AIX.

```console
#!/bin/bash

# Read list of servers to login

for i in `cat server-name.txt`

# Starts the loop to every iten in tke list

do

# Try to connect and send message status log file output.log

echo "Attempting server $i" | tee -a output.log

# Connection and commands are done with a ssh command stored inside a variable named get_users
# The ssh key used must be stored in the user whose enviroment variable it is beeing called
# StrictHostKeyChecking avoid ssh checking. BatchMode it is automation mode.

get_users=`ssh -o StrictHostKeyChecking=no -o BatchMode=yes $USER@$i cat /etc/passwd >> username-$i.log`

# Information it is treated and saved in a file new-servername.log

sed '/nologin/d;/false/d' username-$i.log | cut -d: -f1 | sort >> new-$i.log

# Check if file is empty and if it is empty generates a log file FAIL-get_users.log

if [ ! -s username-$i.log ]
then
	echo "FAIL get user from" $i >> ./FAIL/FAIL-get_users.log
else

# Gathered information is stored and organized by columns

for j in $(cat new-$i.log)
        do
        echo $i $j >> columns.log
        done
fi

echo "############################################################################" >> columns.log

# Clears and organize all the files generated

mv new-$i.log USERNAME/

rm -f username-$i.log

done

# After loop all the itens the script comes to an end
```
