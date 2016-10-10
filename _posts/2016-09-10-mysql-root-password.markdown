---
layout: post
title: "ACCESS DENIED: Reset MySQL root user password"
date: 2016-09-10
categories:
  - Databases
description: How to reset local MySQL (5.7) root user password on Ubuntu (16.04)
image: https://source.unsplash.com/Dt9kdskj6ek/2000x1200
image-sm: https://source.unsplash.com/Dt9kdskj6ek/500x300
---

This is an aggregation of various Stack Overflow posts, user tutorials, and official docs that helped me figure out how to reset my local MySQL root password. 

Thanks to an early engineering assignment, my initial database setup got **really** screwed up. Being the middle of a semester, I didn't have the time to fix it. Now, a couple of updates later, I had to face the beast and get it working. For future reference for myself and for anyone else stuck, this is a brief post on the steps I took that worked for my system. All the tutorials I found during my fix-it session were either incomplete or not *quite* right.

### Stats

OS: Ubuntu 16.04.1 LTS

MySQL: 5.7.15

### Sit-rep

You've installed, and re-installed, and tried to repair an old MySQL installation. Entering

`mysql -u root -p`

followed by your password, returns an

`Access denied for user 'root'@'localhost' ` 

error, even though you're sure it's the right login. You've also already tried the official documentation on [How to reset the root password](http://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html), which failed. Maybe you even tried to repackage the deb file with 

`sudo dpkg-reconfigure mysql-server-5.7`

which failed. Have no fear - where there's a terminal, there's a way.

<hr/>

### Step 1

Shut down Mysql. Try

`sudo service mysql stop`

or 

`sudo /etc/init.d/mysql stop`

or looking up the PID and killing it. Make sure it's stopped via:

`service mysql status`

<hr/>

### Step 2

Start MySQL in safe mode without a password:

`mysqld_safe --skip-grant-tables &`

Caution: this is insecure! I had no data in any of my tables, which meant I wasn't worried about malicious reads. The 'skip' option enables anyone to connect without a password with full privileges. If you have *any* concerns about your tables, you should also diable any remote access with:

`--skip-networking`

<hr/>

### Step 3

In a new terminal, connect to MySQL server with the `mysql` client. No password is neccessary. Execute the following steps:

`use mysql;` <br/>
`UPDATE user SET authentication_string=PASSWORD("securepassword") where User='root';` <br/>
`UPDATE user SET plugin="mysql_native_password";` <br/>
`FLUSH PRIVILEGES;` <br/>
`quit;`

Brief explanation: The second line is obviously where you set your password. One difference between MySQL 5.6 or older, and the latest 5.7+ is the column name switch from `PASSWORD` to `AUTHENTICATION_STRING.` Older tutorials obviously use the former.

If you miss the third 'set plugin' statement, you'll successfully update your password but still won't be able to connect to your server. My system, for example, had the plugin value set to 'auth socket.' Even with the right login details, my server threw errors about my missing socket, and I needed to shut down, restart in safe mode, and switch both values again.

Finally, the 'flush privileges' command reloads the server's in-memory copy of the grant tables. Modifying the user table with UPDATE doesn't load the changes into the tables immediately, unlike the higher-level GRANT or SET commands.

<hr/>

### Step 4

Stop your safe mysqld, and start the mysql server normally. You should be able to connect with your new password via

`mysql -u root -p`

Enjoy your new database access!

### Links

MySQL Reference Manual: [How to reset the root password](http://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html)

Stack Overflow: [MySQL User DB does not have password columns](http://stackoverflow.com/questions/30692812/mysql-user-db-does-not-have-password-columns-installing-mysql-on-osx)

Stack Overflow: [How to stop mysqld](http://stackoverflow.com/questions/11091414/how-to-stop-mysqld)

Stack Overflow: [MySQL Fails on: mysql "ERROR 1524 (HY000)"](http://stackoverflow.com/questions/37879448/mysql-fails-on-mysql-error-1524-hy000-plugin-auth-socket-is-not-loaded)

[Stop Using Flush Privileges](http://dbahire.com/stop-using-flush-privileges/)
