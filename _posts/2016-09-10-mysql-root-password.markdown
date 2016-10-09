---
layout: post
title: "ACCESS DENIED: Reset MySQL root user password"
date: 2016-09-10
categories:
  - Databases
description: How to reset local MySQL (5.7) root user password on Ubuntu (16.04)
image: https://unsplash.it/2000/1200?image=1003
image-sm: https://unsplash.it/500/300?image=1003
---

This is an aggregation of various Stack Overflow posts, user tutorials, and official docs that helped me figure out how to reset my local MySQL root password. 

Thanks to an early engineering assignment, my initial database setup got really screwed up. Being the middle of a semester, I didn't have the time to fix it while still fresh. For future reference for myself and to help anyone else stuck in the same spot, this is a brief post on the steps I took that worked for my system.

### Stats

OS: Ubuntu 16.04.1 LTS

MySQL: 5.7.15

### Sit-rep

You've installed, and re-installed, and tried to repair an old MySQL installation. Entering

`mysql -u root -p`

followed by your password, returns an

`Access denied for user 'root'@'localhost' ` 

error, even though you're sure it's the right login. You've also already tried the official documentation on [How to reset the root password](http://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html), which failed.

### Steps

yadda yadaa

#### one

aelgiuahge

#### two

halguharg


### Links

MySQL Reference Manual: [How to reset the root password](http://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html)

Stack Overflow: [MySQL User DB does not have password columns](http://stackoverflow.com/questions/30692812/mysql-user-db-does-not-have-password-columns-installing-mysql-on-osx)

Stack Overflow: [How to stop mysqld](http://stackoverflow.com/questions/11091414/how-to-stop-mysqld)

Stack Overflow: [MySQL Fails on: mysql "ERROR 1524 (HY000)"](http://stackoverflow.com/questions/37879448/mysql-fails-on-mysql-error-1524-hy000-plugin-auth-socket-is-not-loaded)
