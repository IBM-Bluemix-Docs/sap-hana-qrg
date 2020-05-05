---

copyright:
years: 2017, 2020
lastupdated: "2020-05-05"

keywords: SAP NetWeaver, database server, deployment, storage,

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Adding external storage to your server
{: #storage}

External storage can be added to your provisioned server, or servers, if you want to use it as a backup device, or use a snapshot to quickly restore your database in a test environment. For the three-tier example, block storage is used for both archiving log files of the database and online and offline backups for the database. The fastest block storage (10 IOPS per GB) was selected to help assure a minimum backup time. Slower block storage might be sufficient for your needs. For more information about {{site.data.keyword.blockstoragefull}}, see [Getting started with Block Storage](/docs/BlockStorage?topic=BlockStorage-getting-started#getting-started).
{: shortdesc}

{{site.data.keyword.cloud_notm}} storage LUNS can be provisioned with two options - Endurance and Performance. Endurance tiers feature pre-defined performance levels and other features, such as [snapshot](/docs/BlockStorage?topic=BlockStorage-snapshots) and replication. A custom Performance environment is built with allocated input/output operations per second (IOPS) between 100 and 1,000.

## Setting up external storage
{: #set_up_storage}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external} with your unique credentials.
2. Expand the Menu icon ![Menu icon](../../icons/icon.hamburger.svg) and select *Classic Infrastructure*.
3. Select *Storage* > *Block Storage* > *Order Block Storage*.
4. Select the specifics for your storage needs. Table 1 contains recommended values, including 10 IOPS/GB for a demanding database workload.

|              Field               |      Value                                        |
| -------------------------------- | ------------------------------------------------- |
|Location                          | US South, DAL10                                   |
|Billing Method                    | Monthly (default)                                 |
|Size                              | 1000 GB                                           |
|Endurance (IOPS tiers)            | 10 IOPS/GB                                        |
|Snapshot space                    | 0 GB                                              |
|OS Type                           | Linux (default)                                   |
{: caption="Table 1. Recommended values for block storage" caption-side="top"}

5. Review the Order Summary.
6. Select **I have read and agree to the terms and conditions listed below**.
7. Click **Create**.

### Authorizing host
{: #authorizing-hosts-console}

1. Select **Storage** > **Block Storage**.
2. Highlight your LUN and expand the Action menu ![Action menu](../../icons/action-menu-icon.svg) and select **Authorize Host**.
3. Select a **Device Type** of **Bare Metal Server**.
4. Click **Hardware** to load available devices and select the hostname of your database server.
5. Click **Save**.
6. Check the status of your provisioned storage under **Devices** > (select your device) > **Storage** tab.
7. Note the **Target Address** and iSCSI Qualified Name (**IQN**) for your server (iSCSI initiator), and the **username** and **password** for authorization with the iSCSI server. You need that information in the following steps.

  Additional provisioning information can be found under [Ordering Block Storage through the Console](/docs/BlockStorage?topic=BlockStorage-orderingthroughConsole).
  {: tip}  

Follow the steps in [Connecting to MPIO iSCSCI LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows#mountingWindows) to make your storage accessible from your provisioned server.

## Making storage multipath
{: #multipath}

In the sample deployment, you retrieved the following data from the **Storage** tab:
  * Target IP: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * User: `SL01SU276540-H896345`
  * Password: `EtJ79F4RA33dXm2q`

1. Enter the following based on the retrieved information:
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   An existing entry might have to be replaced in `/etc/iscsi/initiatorname.iscsi`.

2. Add the following lines to the bottom of `/etc/iscsi/iscsid.conf`:
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

3. Replace the `username` and `password` values in step 2 with those that you noted during step 5 of *Authorizing hosts*.

4. Discover the iSCSI target by entering the following lines.
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Set the host to automatically log in to the iSCSI array.

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. Install and start the multipath daemon.
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. Complete all the commands in [Connecting to iSCSI LUNS on Linux](/docs/BlockStorage?topic=BlockStorage-mountingLinux) so another LUN appears in the multipath output.
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

You can now use the multipath device as you would use any disk device. A device path appears under `/dev/mapper/3600a098038303452543f464142755a42`.

Take the sample `/etc/multipath.conf` from the [example `multipath.conf`](/docs/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) and create it on your server. Be aware that any copied special characters, DOS-like carriage returns, line-feed entries might lead to unexpected behavior. Make sure that you have an ASCII Unix file after you copy the contents.

Adapt the multipath block from `/etc/multipath.conf` to create an alias of the path to access the device under `1/dev/mapper/mpath1`.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. Restart `multipathd`. You can now create the `/backup filesystem` and mount on the block device.

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. Check the file systems on both servers. Your output should be similar to the following output.

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

If you install an SAP NetWeaver-based SAP application on {{site.data.keyword.Db2_on_Cloud_long}}, you must create subdirectories under `/backup` owned by the database admin user (`db2SID`) for full backups and archived log files. For automatic archiving of the log files, you should set `LOGMETH1` in your {{site.data.keyword.Db2_on_Cloud_short}} database. Refer to the [{{site.data.keyword.Db2_on_Cloud_short}} documentation)](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: external} for details.
