### Creating Virtual Interface and Assign Multiple IP Addresses,

`[anup@c7-100-apache-server ~]$ cd /etc/sysconfig/network-scripts/`

`[anup@c7-100-apache-server network-scripts]$ ls -ltr`

`[anup@c7-100-apache-server network-scripts]$ sudo nano ifcfg-enp0s8`

    IPADDR=192.168.56.100
    IPADDR1=192.168.56.101
    PREFIX=24
    GATEWAY=192.168.56.1
    DNS1=8.8.8.8
    DNS2=8.8.4.4

`[anup@c7-100-apache-server network-scripts]$ ip addr`

`PS C:\Users\uniqs> ping 192.168.56.100`

`PS C:\Users\uniqs> ping 192.168.56.101`

`PS C:\Users\uniqs> ping 192.168.56.102`


### Create the Directory Structure,

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.ip`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/public_html`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log/`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/public_html`

`[anup@c7-100-apache-server ~]$ sudo mkdir -p /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log/`


### Grant Permissions,

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.ip/`

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/public_html`

`[anup@c7-100-apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/public_html`


### Create Demo Pages for Each Virtual Host,

[anup@c7-100-apache-server ~]$ nano /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/public_html/index.html

    <html>
        <head>
            <title>anuniqsTV</title>
        </head>
        <body>
            <h1>Hey ! from <em>anuniqstv.apache.ip.ek</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>

`[anup@c7-100-apache-server ~]$ nano /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/public_html/index.html`

    <html>
        <head>
            <title>anuniqsTV</title>
        </head>
        <body>
            <h1>Hey ! from <em>anuniqstv.apache.ip.dui</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>


### Create New Virtual Host Files,

`[anup@c7-100-apache-server ~]$ sudo lsof -i -P -n | grep LISTEN`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:80`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:81`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:82`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:83`

`[anup@c7-100-apache-server ~]$ sudo lsof -i:84`

<br>

[anup@c7-100-apache-server ~]$ sudo nano /etc/httpd/sites-available/anuniqstv.apache.ip.conf


    Listen 84
    
    <VirtualHost 192.168.56.101>
        ServerName 192.168.56.101
        ServerAlias 192.168.56.101
        DocumentRoot /var/www/anuniqstv.apache.ip/
        ErrorLog /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log/error.log
        CustomLog /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log/requests.log combined
    </VirtualHost>
    
    <VirtualHost 192.168.56.102>
        ServerName 192.168.56.102
        ServerAlias 192.168.56.102
        DocumentRoot /var/www/anuniqstv.apache.ip/
        ErrorLog /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log/error.log
        CustomLog /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log/requests.log combined
    </VirtualHost>

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-available/`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled`


### Enable the New Virtual Host Files,

`[anup@c7-100-apache-server ~]$ sudo ln -s /etc/httpd/sites-available/anuniqstv.apache.ip.conf /etc/httpd/sites-enabled/anuniqstv.apache.ip.conf`

`[anup@c7-100-apache-server ~]$ ls -ltr /etc/httpd/sites-enabled`


### Firewall,

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --zone=public --add-port=84/tcp --permanent`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --reload`

`[anup@c7-100-apache-server ~]$ sudo firewall-cmd --list-all`


### SELinux,

`[anup@c7-100-apache-server ~]$ sudo semanage port -l | grep http_port_t`

`[anup@c7-100-apache-server ~]$ sudo semanage port -a -t http_port_t -p tcp 84`

 
`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log/`

`[anup@c7-100-apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log(/.*)?"`

`[anup@c7-100-apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log/`

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.ek/log/`


`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log/`

`[anup@c7-100-apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log(/.*)?"`

`[anup@c7-100-apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log/`

`[anup@c7-100-apache-server ~]$ sudo ls -dlZ /var/www/anuniqstv.apache.ip/anuniqstv.apache.ip.dui/log/`


### Check all the available Virtual Host configurations, and restart service,

`[anup@c7-100-apache-server ~]$ httpd -S`

<br>

`[anup@c7-100-apache-server ~]$ apachectl configtest`

`[anup@c7-100-apache-server ~]$ sudo systemctl restart httpd.service`


### Test Your Results, Open your favorite browser :

http://192.168.56.100/

[http://192.168.56.100:80/](http://192.168.56.100/)

http://192.168.56.100:81/

http://192.168.56.100:82/

http://192.168.56.100:83/

http://192.168.56.100:83/anuniqstv.apache.name.ek/public_html/

http://192.168.56.100:83/anuniqstv.apache.name.dui/public_html/

http://192.168.56.101:84/anuniqstv.apache.ip.ek/public_html/

http://192.168.56.102:84/anuniqstv.apache.ip.dui/public_html/


### List all Virtual Hosts available,

`[anup@c7-100-apache-server ~]$ httpd -S`

<br>
