# NFS
> 

# NAS(Network Access Storage)

## 1. NAS 구축

1. Server
```
# yum install nfs-utils
```
2. NAS Dir 설정 및 /etc/exports 설정
```
[root@master /]# ls -lrt | grep backup
drwxr-xr-x    5 root root    146 Jan  3 17:24 backup

[root@master /]# cat /etc/exports
/backup *(rw,no_root_squash)
```
3. Options
```
```
---
# Quota
## Quota 설정

```
```
---
# AutoFS
## AutoFS 설정
1. Client

- Install autofs
```
# yum install autofs
```
- autofs.conf 설정
```
[root@slave1 etc]# cat autofs.conf | grep -v '#'
[ autofs ]
# 
timeout = 300
# autofs를 통하여 설정한 Mount Point 위치에 실시간으로 리스트를 볼 수 있는 옵션
browse_mode = no

mount_nfs_default_protocol = 4
```
- auto.master 설정
- <span style="color:yellow">"Client Mount Point"</span> <span style="color:green">"Mount Point Map File"</span>
```
[root@slave1 ~]# cat /etc/auto.master
#
# Sample auto.master file
# This is a 'master' automounter map and it has the following format:
# mount-point [map-type[,format]:]map [options]
# For details of the format look at auto.master(5).
#
/backup /etc/auto.backupdir
```
- Mount Map File
```
[root@slave1 etc]# cat /etc/auto.backupdir | grep -v '#'
async   -fstype=nfs,rw,intr     192.168.50.6:/backup/async_dir
sync    -fstype=nfs,rw,intr     192.168.50.6:/backup/sync_dir
```
- Options
```
```
- autofs restart
```
[root@slave1 ~]# systemctl restart autofs
```
- Mount 확인

```
# Client
[root@slave1 backup]# pwd
/backup
[root@slave1 backup]# ls
async  sync

# Server
[root@master backup]# pwd
/backup
[root@master backup]# ls | grep dir
async_dir
sync_dir
```
---
# SAMBA vs CIFS
## SAMBA란?
```
```
---