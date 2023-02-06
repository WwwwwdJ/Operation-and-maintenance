# 搭建NFS服务器

*NFS服务端：10.2.48.250*

**安装NFS服务和NFS安全传输服务**

`[root@nfs work]# yum install nfs-utils`
`[root@nfs work]# yum install rpcbind`

**关闭防火墙**

`[root@nfs ~]# systemctl stop firewalld`

**创建共享目录**

`[root@nfs ~]# mkdir /Service
`[root@nfs /]# mkdir /lamp-01`
`[root@nfs /]# mkdir /lnmp-02`

**设定SELinux安全上下文**

`[root@nfs ~]# semanage fcontext -a -t'public_content_t' '/WordPress(/.*)?'`
`[root@nfs ~]# semanage fcontext -a -t'public_content_t' '/lamp-01(/.*)?'`
`[root@nfs ~]# semanage fcontext -a -t'public_content_t' '/lnmp-02(/.*)?'`
`[root@nfs ~]# restorecon -vvRF /Service/`
`[root@nfs ~]# restorecon -vvRF /lamp-01/`
`[root@nfs ~]# restorecon -vvRF /lnmp-02/`

**编辑NFS配置文件**

`[root@nfs ~]# vim /etc/exports` 
`/Service 10.2.48.0/24((rw,sync,root_squash)`
`/lamp-01 10.2.48.0/24(rw,sync,root_squash)`
`/lnmp-02 10.2.48.0/24(rw,sync,root_squash)`


**重新启动服务**

`[root@nfs ~]# systemctl restart nfs-server nfs-secure`

**刷新并验证导出资源**

`[root@nfs ~]# vim /etc/exports`
`[root@nfs ~]# exportfs -ra`
`[root@nfs ~]# exportfs `
`/Service    	10.2.48.0/24`
`/lamp-01        10.2.48.0/24`
`/lnmp-02        10.2.48.0/24`


