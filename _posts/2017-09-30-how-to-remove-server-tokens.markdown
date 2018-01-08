---
layout: post
title:  "How to remove server tokens"
date:   2018-01-08 07:13:26 +0200
category: Server
tags: [server,linux]
---

<h2>PLESK</h2>

`mkdir -p /usr/local/psa/admin/conf/templates/custom/domain`<br />
`cp /usr/local/psa/admin/conf/templates/default/server.php /usr/local/psa/admin/conf/templates/custom`<br />
`cp /usr/local/psa/admin/conf/templates/default/domain/nginxDomainVirtualHost.php /usr/local/psa/admin/conf/templates/custom/domain`<br />
`cp /usr/local/psa/admin/conf/templates/default/domain/nginxForwarding.php /usr/local/psa/admin/conf/templates/custom/domain`<br />
<br />


Modify `server.php`:<br />
remove `Header add X-Powered-By PleskLin`<br /><br />

Modify `nginxDomainVirtualHost.php`:<br />
remove `add_header X-Powered-By PleskLin;`<br /><br />


Rebuild configuration files:<br />
`/usr/local/psa/admin/sbin/httpdmng --reconfigure-all`<br /><br />


https://support.plesk.com/hc/en-us/articles/115000385274-How-to-remove-modify-the-X-Powered-By-header-
<br /><br />

<h2>NON PLESK</h2>


On Debian, Ubuntu or Linux Mint: <br />
`$ sudo vi /etc/apache2/apache2.conf`<br /><br />

On CentOS, Fedora, RHEL or Arch Linux:<br />
`$ sudo vi /etc/httpd/conf/httpd.conf`<br /><br />

`ServerSignature Off`<br />
`ServerTokens Prod`<br /><br />

The first line 'ServerSignature Off' makes Apache2 web server hide Apache version info on any error pages.

<br /><br />


On Debian, Ubuntu, or Linux Mint:<br />
`$ sudo vi /etc/php5/apache2/php.ini`<br /><br />

On CentOS, Fedora, RHEL or Arch Linux:<br />
`$ sudo vi /etc/php.ini`<br /><br />

`expose_php = Off`<br /><br />

Finally, restart Apache2 web server to reload updated PHP config file.
Now you will no longer see "X-Powered-By" field in HTTP response headers.