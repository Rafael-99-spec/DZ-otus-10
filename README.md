# ДЗ №10 Управление пакетами. Дистрибьюция софта 
--------------------------------------------------------------------------------------------
## 1) Cоздать свой RPM
Установим все необходимые пакеты для сборки собственного пакета ```*.rpm``` пакета.
```yum install -y redhat-lsb-core wget rpmdevtools rpm-build createrepo yum-utils wget gcc```


```
[root@localhost ~]# wget https://nginx.org/packages/centos/7/SRPMS/nginx-1.14.1-1.el7_4.ngx.src.rpm
--2020-06-26 12:32:13--  https://nginx.org/packages/centos/7/SRPMS/nginx-1.14.1-1.el7_4.ngx.src.rpm
Resolving nginx.org (nginx.org)... 95.211.80.227, 62.210.92.35, 2001:1af8:4060:a004:21::e3
Connecting to nginx.org (nginx.org)|95.211.80.227|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1033399 (1009K) [application/x-redhat-package-manager]
Saving to: ‘nginx-1.14.1-1.el7_4.ngx.src.rpm’

100%[=============================================>] 1,033,399   2.29MB/s   in 0.4s   

2020-06-26 12:32:14 (2.29 MB/s) - ‘nginx-1.14.1-1.el7_4.ngx.src.rpm’ saved [1033399/1033399]
```
```
[root@localhost ~]# adduser builder
[root@localhost ~]# rpm -ihv nginx-1.14.1-1.el7_4.ngx.src.rpm
warning: nginx-1.14.1-1.el7_4.ngx.src.rpm: Header V4 RSA/SHA1 Signature, key ID 7bd9bf62: NOKEY
Updating / installing...
   1:nginx-1:1.14.1-1.el7_4.ngx       ################################# [100%]
   
```

```
[root@localhost ~]# wget https://www.openssl.org/source/latest.tar.gz
--2020-06-26 12:32:46--  https://www.openssl.org/source/latest.tar.gz
Resolving www.openssl.org (www.openssl.org)... 23.52.18.127, 2a02:26f0:103:1ad::c1e, 2a02:26f0:103:19d::c1e
Connecting to www.openssl.org (www.openssl.org)|23.52.18.127|:443... connected.
HTTP request sent, awaiting response... 302 Moved Temporarily
Location: https://www.openssl.org/source/openssl-1.1.1g.tar.gz [following]
--2020-06-26 12:32:46--  https://www.openssl.org/source/openssl-1.1.1g.tar.gz
Reusing existing connection to www.openssl.org:443.
HTTP request sent, awaiting response... 200 OK
Length: 9801502 (9.3M) [application/x-gzip]
Saving to: ‘latest.tar.gz’

100%[=============================================>] 9,801,502   10.7MB/s   in 0.9s   

2020-06-26 12:32:47 (10.7 MB/s) - ‘latest.tar.gz’ saved [9801502/9801502]
```


```
[root@localhost ~]# tar -xvf latest.tar.gz
```


```
[root@localhost ~]# yum-builddep rpmbuild/SPECS/nginx.spec
Loaded plugins: fastestmirror
Enabling base-source repository
Enabling extras-source repository
Enabling updates-source repository
Loading mirror speeds from cached hostfile
 * base: mirrors.datahouse.ru
 * extras: centos-mirror.rbc.ru
 * updates: centos-mirror.rbc.ru
base-source                                                     | 2.9 kB  00:00:00     
extras-source                                                   | 2.9 kB  00:00:00     
updates-source                                                  | 2.9 kB  00:00:00     
(1/3): extras-source/7/primary_db                               |  21 kB  00:00:00     
(2/3): updates-source/7/primary_db                              |  48 kB  00:00:00     
(3/3): base-source/7/primary_db                                 | 974 kB  00:00:02     
Checking for new repos for mirrors
Getting requirements for rpmbuild/SPECS/nginx.spec
 --> Already installed : redhat-lsb-core-4.1-27.el7.centos.1.x86_64
 --> Already installed : systemd-219-73.el7_8.5.x86_64
 --> 1:openssl-devel-1.0.2k-19.el7.x86_64
 --> zlib-devel-1.2.7-18.el7.x86_64
 --> pcre-devel-8.32-17.el7.x86_64
 ...
```

```
[root@localhost ~]# rpmbuild -bb rpmbuild/SPECS/nginx.spec
Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.AuYY8l
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd /root/rpmbuild/BUILD
+ rm -rf nginx-1.14.1
+ /usr/bin/gzip -dc /root/rpmbuild/SOURCES/nginx-1.14.1.tar.gz
+ /usr/bin/tar -xf -
+ STATUS=0
+ '[' 0 -ne 0 ']'
+ cd nginx-1.14.1
+ /usr/bin/chmod -Rf a+rX,u+w,g-w,o-w .
+ cp /root/rpmbuild/SOURCES/nginx.init.in .
+ sed -e 's|%DEFAULTSTART%|2 3 4 5|g' -e 's|%DEFAULTSTOP%|0 1 6|g' -e 's|%PROVIDES%|nginx|g'
+ sed -e 's|%DEFAULTSTART%||g' -e 's|%DEFAULTSTOP%|0 1 2 3 4 5 6|g' -e 's|%PROVIDES%|nginx-debug|g'
+ exit 0
Executing(%build): /bin/sh -e /var/tmp/rpm-tmp.f4Nce7
+ umask 022
...
```

```
[root@localhost ~]# yum localinstall /root/rpmbuild/RPMS/x86_64/nginx-1.14.1-1.el7_4.ngx.x86_64.rpm -y
Loaded plugins: fastestmirror
Examining /root/rpmbuild/RPMS/x86_64/nginx-1.14.1-1.el7_4.ngx.x86_64.rpm: 1:nginx-1.14.1-1.el7_4.ngx.x86_64
Marking /root/rpmbuild/RPMS/x86_64/nginx-1.14.1-1.el7_4.ngx.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package nginx.x86_64 1:1.14.1-1.el7_4.ngx will be installed
--> Finished Dependency Resolution

Dependencies Resolved
...
  Verifying  : 1:nginx-1.14.1-1.el7_4.ngx.x86_64                                                                                                                                                               1/1 

Installed:
  nginx.x86_64 1:1.14.1-1.el7_4.ngx                                                                                                                                                                                

Complete!
[root@localhost ~]# 
```

```
[root@localhost ~]# systemctl start nginx
[root@localhost ~]# systemctl status nginx
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-06-26 13:04:58 UTC; 4s ago
     Docs: http://nginx.org/en/docs/
  Process: 10548 ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf (code=exited, status=0/SUCCESS)
 Main PID: 10549 (nginx)
   CGroup: /system.slice/nginx.service
           ├─10549 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
           └─10550 nginx: worker process

Jun 26 13:04:58 localhost.localdomain systemd[1]: Starting nginx - high performance web server...
Jun 26 13:04:58 localhost.localdomain systemd[1]: Started nginx - high performance web server.
```


## 2) Cоздать свой репо и разместить там свой RPM реализовать это все либо в вагранте, либо развернуть у себя через nginx и дать ссылку на репо
