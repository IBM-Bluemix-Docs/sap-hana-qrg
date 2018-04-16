---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. Netz für ein dreischichtiges Setup einrichten
{: #network}

Wenn Sie ein dreischichtiges Setup installieren möchten, muss das Netz ordnungsgemäß einrichtet werden. Im vorliegenden Beispiel wurden ein 256 GB-Datenbankserver (namens `e2e2690`) und ein 32 GB-Anwendungsserver (namens `e2e1270`) bereitgestellt. Der Datenbankserver hostet zudem die (A)SCS-Instanz. Das Hinzufügen der IP-Adressen für das private Netz zu `/etc/hosts` dient als Grundlage für die nachfolgenden Schritte und stellt sicher, dass der interne SAP-Netzverkehr durch das richtige Netz geleitet wird.

![Abbildung 1. Beispiel für ein dreischichtiges Setup](/images/network-01.png "Beispiel für ein dreischichtiges Setup")

Verwenden Sie die folgenden Schritte zum Erstellen des Netzes.

1. Melden Sie sich bei den Servern an und suchen Sie nach den entsprechenden Netzkonfigurationen.

        [root@e2e2690 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@e2e2690 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

In dem dreischichtigen Beispiel ist '10.17.139.35' die private IP-Adresse des Datenbankservers; sie stammt aus einem der IP-Adressbereiche von RFC 1597. Sie können auch die IP-Adresse des Anwendungsservers bestimmen und beide IP-Adressen zum Verzeichnis `/etc/hosts` des jeweiligen Servers hinzufügen.

        [root@e2e2690 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690
        
        10.17.139.35    e2e2690-priv
        10.17.139.46    e2e1270-priv

Fügen Sie die letzten beiden Zeilen auch für `e2e1270` hinzu.

## NFS-Software installieren

1. Installieren Sie die NFS-Software `nfs-utils` **auf beiden Servern**.

      `[root@e2e2690a ~]# yum install nfs-utils`

Stellen Sie sicher, dass Sie `rpcbind` und den NFS-Service auf dem Datenbankserver starten und registrieren.

        [root@e2e2690 ~]# service rpcbind start
        [root@e2e2690 ~]# chkconfig nfs on
        [root@e2e2690 ~]# service nfs start

## NFS für den Export verwenden

1. Verwenden Sie NFS zum Exportieren der Verzeichnisse `/sapmnt` und `/usr/sap/trans` vom Datenbankserver auf den Anwendungsserver, indem Sie den erforderlichen Eintrag zum Verzeichnis `/etc/exports` des Datenbankservers hinzufügen.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

Der Beispielwert `C10` muss durch die SAP-System-ID für das SAP-System ersetzt werden. Sie müssen das Verzeichnis erstellen, bevor Sie es exportieren. Führen Sie über die Befehlszeile des Datenbankservers die folgenden Befehle aus:

        [root@e2e2690 ~]# mkdir /sapmnt/C10
        [root@e2e2690 ~]# mkdir -p /usr/sap/trans
        [root@e2e2690 ~]# exportfs -a

## Gemeinsam genutzte NFS-Ressource anhängen

1. Hängen Sie die gemeinsam genutzte NFS-Ressource an den Anwendungsserver an, indem Sie den folgenden Eintrag zum Verzeichnis `/etc/fstab` des Anwendungsservers hinzufügen und die Ressource über die Befehlszeile anhängen.

        e2e2690-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        e2e2690-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. Erstellen Sie die Zielverzeichnisse auf dem Anwendungsserver und hängen Sie sie an.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

Die Server sind nun so vorbereitet, dass die Komponenten einer verteilten SAP-Installationen auf ihnen gehostet werden können. 

## Nächste Schritte

  * [Externen Speicher zur {{site.data.keyword.baremetal_short}}-Instanz hinzufügen](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [SAP-Anwendungen und -Software installieren](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
