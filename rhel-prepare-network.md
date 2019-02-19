---



copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. Preparing your network for a three-tier setup
{: #network}

If you are planning to install a three-tier setup, the network needs to be set up correctly. In the example, a 192 GB database server (named `sdb192`) and a 32 GB application server (named `e2e1270`) have been deployed. The database server also hosts the (A)SCS instance. Adding the IP addresses on the private network to your `/etc/hosts` helps with the upcoming steps and ensures that SAP internal network traffic goes through the right network.

![Figure 1. Sample of three-tier setup](/images/network-01.png "Sample of three-tier setup")

Use the following steps to establish your network.

1. Log in to the servers and find their private network configuration.

        [root@sdb192 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@sdb192 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

In the three-tier example, 10.17.139.35 is the private IP address of the database server; it is from one of the IP address ranges from RFC 1597. You can determine the IP address of the application server, too, and add both IP addresses to both servers’ `/etc/hosts`.

        [root@sdb192 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690

        10.17.139.35    sdb192-priv
        10.17.139.46    e2e1270-priv

Add the last two lines on `e2e1270`, too.

## Installing NFS software

1. Install NFS software `nfs-utils` **on both servers**.

      `[root@sdb192 ~]# yum install nfs-utils`

Make sure you start and register the `rpcbind` and NFS service on the database server.

        [root@sdb192 ~]# service rpcbind start
        [root@sdb192 ~]# chkconfig nfs on
        [root@sdb192 ~]# service nfs start

## Using NFS to export

1. Use NFS to export `/sapmnt` and `/usr/sap/trans` from the database server to the application server by adding the required entry to `/etc/exports` of the database server.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

The sample value `C10` needs to be replaced with the SAP System ID for your SAP system. You must create the directory before you export it. Run the following commands from the command line of the database server:

        [root@sdb192 ~]# mkdir /sapmnt/C10
        [root@sdb192 ~]# mkdir -p /usr/sap/trans
        [root@sdb192 ~]# exportfs -a

## Mounting the NFS share

1. Mount the NFS share on the application server by adding the following entry to its `/etc/fstab` and mount it from the command line.

        sdb192-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        sdb192-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. Create the target directories on the application server and mount them.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

Your servers are now prepared to host the components of a distributed SAP installation.

## Next Steps

  * [Adding external storage to your {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-storage)
  * [Installing your SAP landscape (applications and software)](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_landscape)
