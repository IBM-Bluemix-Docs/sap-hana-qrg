---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, database server, deployment

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 向服务器添加外部存储器
{: #storage}

## 设置外部存储器
{: #set_up_storage}

如果您希望将外部存储器用作备份设备或使用快照在测试环境中快速复原数据库，那么可以向供应的一个或多个服务器添加外部存储器。对于三层示例，块存储器用于归档数据库的日志文件以及联机和脱机备份数据库。选择了最快的块存储器 (4 IOPS/GB)，以帮助确保最短备份时间。较慢的块存储器可能已足够满足您的需求。有关 {{site.data.keyword.blockstoragefull}} 的更多信息，请参阅 [Block Storage 入门](/docs/infrastructure/BlockStorage?topic=BlockStorage-GettingStarted#getting-started-with-block-storage)。


1. 使用您的唯一凭证登录到 [{{site.data.keyword.cloud_notm}} 基础架构客户门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){: new_window}。
2. 选择**存储器** > **Block Storage**。
3. 单击 Block Storage 页面右上角的**订购 Block Storage**。
4. 选择适合您的存储需求的具体规格。表 1 包含建议值，包括适合典型数据库工作负载的 4 IOPS/GB。

|字段                |值                                                              |
| -------------------------------- | ------------------------------------------------- |
|位置                              |TOR01                                             |
|计费方式                          |每月（缺省值）                                    |
|新存储器大小                      |1000 GB                                           |
|存储器 IOPS 选项                  |耐久性（分层 IOPS）（缺省值）                     |
|耐久性分层 IOPS                   |10 GB                                             |
|快照空间大小                      |0 GB                                              |
|操作系统类型                      |缺省为 Linux                                      |
{: caption="表 1. 块存储器的建议值" caption-side="top"}

5. 单击两个复选框，然后单击**下订单**。

## 授权主机
{: authorize-host}

1. 单击 LUN 右侧的**操作**，并选择**授权主机**以访问供应的存储器。
2. 选择**设备**；**设备类型**缺省为“裸机服务器”。单击**硬件**并选择设备的主机名。
3. 单击**提交**按钮。
4. 在**设备** >（选择您的设备）> **存储器**选项卡下检查供应的存储器的状态。
5. 记下您的服务器（iSCSI 启动器）的**目标地址**和 iSCSI 限定名 (**IQN**)，以及**用户名**和**密码**，以用于授权 iSCSI 服务器。在以下步骤中需要该信息。
6. 执行[连接到 Linux 上的 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#connecting-to-mpio-iscsi-luns-on-linux) 中的步骤以使您的存储器可从供应的服务器访问。

## 将存储器设置为多路径
{: #multipath}

在样本部署中，您从**存储器**选项卡检索了以下数据：
  * 目标 IP：`10.2.62.78`
  * IQN：`iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * 用户：`SL01SU276540-H896345`
  * 密码：`EtJ79F4RA33dXm2q`

1. 基于检索的信息输入以下内容：
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   可能必须替换 `/etc/iscsi/initiatorname.iscsi` 中的一个现有条目。

2. 在 `/etc/iscsi/iscsid.conf` 底部添加以下行：
```
[root@sdb192 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. 将步骤 2 中的 `username` 和 `password` 值替换为*授权主机*的步骤 5 中记下的相应值。

4. 输入以下行来发现 iSCSI 目标。
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. 将主机设置为自动登录到 iSCSI 阵列。

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. 安装并启动多路径守护程序。
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. 完成[连接到 Linux 上的 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux) 中的所有命令，以便多路径输出中显示另一个 LUN。
```
[root@sdb192 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

您现在可以像使用任何磁盘设备一样使用多路径设备。设备路径将显示在 `/dev/mapper/3600a098038303452543f464142755a42` 下。

采用[示例 `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) 中的样本 `/etc/multipath.conf`，然后在您的服务器上创建此文件。请注意，任何复制的特殊字符、类 DOS 的回车符和换行输入都可能导致意外行为。请确保您在复制内容之后产生的是 ASCII Unix 文件。

调整 `/etc/multipath.conf` 中的多路径块以创建路径别名来访问 `1/dev/mapper/mpath1` 下的设备。

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. 重新启动 `multipathd`。您现在可以创建 `/backup filesystem` 并在块设备上进行安装。

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. 检查两个服务器上的文件系统。您的输出应该类似于以下输出。

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db192-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db192-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@sdb192 ~]# df -h
        Filesystem      	    Size  Used Avail Use% Mounted on
        /dev/sda3             549G  2,3G  519G   1% /
        tmpfs                 127G     0  127G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/mapper/mpath1    493G   70M  468G   1% /backup
        /dev/mapper/datavg-datalv
                      1,2T   71M  1,1T   1% /db2
        /dev/mapper/datavg-saplv
                      165G   60M  157G   1% /usr/sap
        /dev/mapper/datavg-sapmntlv
                      165G   60M  157G   1% /sapmnt

如果在 {{site.data.keyword.Db2_on_Cloud_long}} 上安装基于 SAP NetWeaver 的 SAP 应用程序，那么在数据库管理员用户 (`db2SID`) 拥有的 `/backup` 下为完全备份和归档的日志文件创建子目录。对于日志文件的自动归档，您应该在 {{site.data.keyword.Db2_on_Cloud_short}} 数据库中设置 `LOGMETH1`。请参阅 [{{site.data.keyword.Db2_on_Cloud_short}} 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: new_window} 以获取详细信息。
