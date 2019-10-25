---
layout: post
title:  "Basic Unix Commands"
date:   2017-07-26 10:12:26 +0200
category: unix
tags: [unix]
---

<h1>Unix</h1>

<h2>Files backup</h2>
`tar czpf site_files.tar.gz httpdocs --exclude='ajax-uploader' --exclude='anyotherdirectory'`
<br /><br />

<h2>Files backup, split into more files with max size</h2>
`tar cvzf - DIR | split --bytes=200MB - sda1.backup.tar.gz.` <br />
`tar cvzf - FILE | split --bytes=200MB - sda1.backup.tar.gz.`
<br /><br />


<h2>Join splitted files to one file</h2>
`cat vid* > test.tar.gz`<br />
<br /><br />

<h2>Unzip tar gzip</h2>
`gunzip magento-1.4.1.0.tar.gz` <br />
`tar xf magento-1.4.1.0.tar`
<br /><br />


<h2>Search for text in all files under dir</h2>
`grep -r "something.co.uk"  /etc/nginx/conf.d` <br />
<br /><br />


<h2>Tar gzip</h2>
`tar -czf dbname.tar.gz dbdump.sql` <br />
<br /><br />

<h2>Get free space on server</h2>
`df -h` <br />
<br /><br />

<h2>Get size of directory</h2>
`du -sh DIRECTORY` <br />
<br /><br />

<h2>Change user</h2>
`su - USERNAME` <br />
<br /><br />

<h2>Change folder permissions (for user lrg and folder var)</h2>
`chown lrg:lrg var -R` <br />
<br /><br />

<h2>List folder</h2>
`ls -l` <br />
<br /><br />

<h2>Find string in file (simple)</h2>
`grep simple system.log | less` <br />
<br /><br />


<h2>Show processes list</h2>
`ps axf | less` <br />
<br /><br />


<h2>Check if Magento reindex process is running</h2>
`ps aux | grep php | grep indexer` <br />
<br /><br />


<h2>Kill process</h2>
`kill NNNN` <br />
<br /><br />


<h2>Move all files one dir up</h2>
`mv myfolder/* .` <br />
<br /><br />


<h2>List of unsend mails - mail queue</h2>
`mailq` <br />
<br /><br />


<h2>Display email with concrete header</h2>
`postcat -q E6A0227EDB` <br />
<br /><br />

<h2>Kill all chrome</h2>
`ps aux | grep chrome | awk ' { print $2 } ' | xargs kill -9` <br />
<br /><br />

<h2>Find all files bigger than 250mb</h2>
`find . -size +250M -ls` <br />
<br /><br />

<h2>To find out all files that have been modified on 2013-02-07 (07/Feb/2013)</h2>
`find /path/to/dir -type f -name "*" -newermt 2013-02-07 ! -newermt 2013-02-08` <br />
<br /><br />

<br /><br />
<h1>DB</h1>

<h2>Mysql backup</h2>
`mysqldump -u USERNAME -pPASSWORD DB_NAME > FILENAME.sql`
<br /><br />


<h2>Db dump</h2>
`mysqldump -u username -ppassword dbname > dbdump.sql` <br />
<br /><br />

<h2>List running mysql queries (showing the longest running at the bottom of the list)</h2>
`mysql -e 'show full processlist;' | grep -v Sleep | sort -n -k 6` <br />
<br /><br />


<h2>Mysql kill query</h2>
`mysql> kill query QUERYID;` <br />
<br /><br />


<h2>Connect to db</h2>
`mysql -h localhost -u username -ppasword` <br />
<br /><br />

<h2>Import db - if in sql file is query for creating db</h2>
`mysql -u username -ppassword < dbdump.sql` <br />
<br /><br />

<h2>Import db - if in sql file isnt query for creating db</h2>
`mysql -u username -ppassword dbname < dbdump.sql` <br />
<br /><br />

<h2>Mysql show databases</h2>
`show databases;` <br />
<br /><br />

<h2>Copy db</h2>
`mysqldump mydbname | mysql mydbcopy`<br />
<br /><br />

<h2>Change mysql password for user</h2>
`set password fop 'db-user'@'db' = PASSWORD('db-password');`<br />
 `flush privileges;` <br />
<br /><br />
