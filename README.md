## SF4-LTS-BASE-SECURE
### Installation
#### Centos8 Stream - PHP 7.4 / Mysql 8
##### Les repos
EPEL :`dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`<br>
REMI :`dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm`<br>
##### Update
`dnf update `<br>
`reboot`<br>
##### PHP 7.4
`dnf module install php:remi-7.4`<br>

NB :<br>
`# dnf install php56`<br>
`# module load php56`<br>
`# php --version`<br>
`PHP 5.6.40 (cli)`<br>

##### Apache
Installation et lancement:<br>
`dnf install httpd`<br>
`systemctl enable --now httpd`<br>

Firewall :<br>
`firewall-cmd --zone=public --permanent --add-service=http`<br>
`firewall-cmd --reload`<br>

##### MySQL 8
Installation et lancement:<br>
`dnf install @mysql`<br>
`systemctl enable --now mysqld`<br>

Configuration :<br>
`mysql_secure_installation`<br>
<br>
<code>==><br>
y => Low secu (>=8) ********<br>
y => no anonymous<br>
n => root login remotely<br>
y => remove test DB<br>
y => reload privilege
</code>

##### Git
`dnf install git`<br>

##### Composer
`curl -sS https://getcomposer.org/installer | php`<br>
`mv composer.phar /usr/local/bin/composer`<br>

##### Security OFF
`vi /etc/selinux/config`<br>

<pre>
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=<b>disabled</b>
# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
</pre>

##### Samba
Installation et lancement:<br>
`dnf install samba samba-client`<br>
`systemctl enable --now {smb,nmb}`<br>

Firewall :<br>
`firewall-cmd --permanent --add-service=samba`<br>
`firewall-cmd --reload`<br>

Configuration :<br>
`vi /etc/samba/smb.conf`<br>

<pre>
# See smb.conf.example for a more detailed config file or
# read the smb.conf manpage.
# Run 'testparm' to verify the config is correct after
# you modified it.
[global]
        workgroup = SAMBA
        security = user
        server string = samba
        map to guest = Bad User
        hosts allow = all

#=== Share defnitions ==#

[web]
        path = /var/www/html/
        writeable = yes
        browseable = yes
        guest ok = yes
        read only = no
        create mask = 0777
        force create mode = 0777
        directory mask = 0777
        force directory mode = 0777
</pre>

`systemctl restart {smb,nmb}`<br>
`testparm`<br>

##### Droits OPENBAR
`cd /var/www/html`<br>
`chown nobody:nobody -R .`<br>
`chmod -R 777 .`<br>

#### Symfony 4.4
##### Installation
###### Skeleton
`composer create-project symfony/skeleton=^4.4 sf4-lts-base-secure`<br>

###### Base
`composer require annotations symfony/security-bundle`<br>
`composer require --dev maker profiler sensiolabs/security-checker reqcheck`<br>

###### ORM
`composer require doctrine`<br>
`composer require --dev doctrine/doctrine-fixtures-bundle fzaninotto/faker`<br>

###### Frontend
`composer require twig asset form symfony/validator`<br>

###### Mail
`composer require symfony/swiftmailer-bundle`<br>