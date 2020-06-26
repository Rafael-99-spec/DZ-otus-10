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


## 2) Cоздать свой репо и разместить там свой RPM реализовать это все либо в вагранте, либо развернуть у себя через nginx и дать ссылку на репо
