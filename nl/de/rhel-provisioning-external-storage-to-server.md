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

# Externen Speicher zum Server hinzufügen
{: #storage}

## Externen Speicher einrichten
{: #set_up_storage}

Sie können externen Speicher zu den bereitgestellten Servern hinzufügen, wenn Sie diesen als Backup-Einheit verwenden oder die Datenbank anhand einer Momentaufnahme schnell in einer Testumgebung wiederherstellen möchten. Bei dem Beispiel mit drei Ebenen wird Blockspeicher sowohl für das Archivieren von Protokolldateien der Datenbank als auch für Online- und Offline-Backups für die Datenbank verwendet. Es wurde der schnellste Blockspeicher (4 IOPS pro GB) ausgewählt, um eine schnelle Backupzeit zu gewährleisten. Ein langsamerer Blockspeicher könnte für Ihre Anforderungen nicht ausreichend sein. Weitere Informationen zu {{site.data.keyword.blockstoragefull}} finden Sie unter [Einführung in Blockspeicher](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started).


1. Melden Sie sich beim [Kundenportal der {{site.data.keyword.cloud_notm}}-Infrastruktur ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){: new_window} mit Ihren eindeutigen Berechtigungsnachweisen an.
2. Wählen Sie **Speicher** > **Blockspeicher** aus.
3. Klicken Sie oben rechts auf der Seite für den Blockspeicher auf **Blockspeicher bestellen**.
4. Wählen Sie die Spezifikationen für Ihre Speicheranforderungen aus. Tabelle 1 enthält empfohlene Werte, einschließlich 4 IOPS/GB für eine typische Auslastung für Datenbanken.

|              Feld               |      Wert                                        |
| -------------------------------- | ------------------------------------------------- |
|Position                          | TOR01                                             |
|Zahlungsmethode                    | Monatlich (Standard)                                 |
|Neue Speichergröße                  | 1000 GB                                           |
|IOPS-Speicheroptionen              | Belastbarkeit (gestaffelte IOPS) (Standard)                 |
|Belastbarkeit (gestaffelte IOPS)             | 10 GB                                             |
|Speicherbereich f. Momentaufnahme               | 0 GB                                              |
|Betriebssystemtyp                           | Standardmäßig Linux                                 |
{: caption="Tabelle 1. Empfohlene Werte für Blockspeicher" caption-side="top"}

5. Klicken Sie auf die beiden Kontrollkästchen und anschließend auf **Bestellen**.

## Hosts autorisieren
{: authorize-host}

1. Klicken Sie rechts neben der LUN auf **Aktionen** und wählen Sie **Host authorisieren** aus, um auf den bereitgestellten Speicher zuzugreifen.
2. Wählen Sie **Geräte** aus; der Wert für **Gerätetyp** nimmt standardmäßig den Wert 'Bare-Metal-Server' ein. Klicken Sie auf **Hardware**, und wählen Sie die Hostnamen der Geräte aus.
3. Klicken Sie auf **Abschicken**.
4. Überprüfen Sie den Status des bereitgestellten Speichers auf der Registerkarte **Geräte** > (wählen Sie Ihr Gerät aus) > **Speicher**.
5. Notieren Sie sich die **Zieladresse** und den qualifizierten iSCSI-Namen (**IQN**) für Ihren Server (iSCSI-Initiator) sowie den **Benutzernamen** und das **Kennwort** für die Autorisierung beim iSCSI-Server. Sie benötigen diese Informationen für die folgenden Schritte.
6. Führen Sie die Schritte unter [Verbindung zu iSCSI-LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#connecting-to-mpio-iscsi-luns-on-linux) aus, damit der Speicher über den bereitstellten Server zugänglich ist.

## Speicher als Multipath angeben
{: #multipath}

In der Beispielbereitstellung haben Sie die folgenden Daten von der Registerkarte **Speicher** abgerufen:
  * Ziel-IP: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * Benutzer: `SL01SU276540-H896345`
  * Kennwort: `EtJ79F4RA33dXm2q`

1. Geben Sie basierend auf den abgerufenen Informationen Folgendes ein:
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   Möglicherweise muss ein vorhandener Eintrag in `/etc/iscsi/initiatorname.iscsi` ersetzt werden.

2. Fügen Sie unten in `/etc/iscsi/iscsid.conf` die folgenden Zeilen hinzu:
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

3. Ersetzen Sie die Werte für `username` und `password` in Schritt 2 durch die Werte, die Sie in Schritt 5 unter *Hosts autorisieren* notiert haben.

4. Rufen Sie das iSCSI-Ziel ab, indem Sie die folgenden Zeilen eingeben.
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Legen Sie den Host so fest, dass eine automatische Anmeldung beim iSCSI-Array erfolgt.

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. Installieren und starten Sie den Multipath-Dämon.
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. Führen Sie alle Befehle in [Verbindung zu iSCSI-LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux) aus, damit in der Multipath-Ausgabe eine weitere LUN angezeigt wird.
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

Sie können das Multipath-Gerät nun wie alle anderen Platteneinheiten verwenden. Unter `/dev/mapper/3600a098038303452543f464142755a42` wird ein Gerätepfad angezeigt.

Nehmen Sie die Beispieldatei `/etc/multipath.conf` aus dem [Beispiel für `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) und erstellen Sie sie auf dem Server. Beachten Sie, dass alle kopierten Sonderzeichen, DOS-ähnlichen Wagenrückläufe und Zeilenvorschübe zu unerwartetem Verhalten führen kann. Stellen Sie sicher, dass Sie nach dem Kopieren des Inhalts über eine ASCII-UNIX-Datei verfügen.

Passen Sie den Multipath-Block aus `/etc/multipath.conf` so an, um einen Alias des Pfades für den Zugriff auf das Gerät unter `1/dev/mapper/mpath1` zu erstellen.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. Starten Sie `multipathd` erneut. Sie können jetzt das `/backup`-Dateisystem erstellen und es an die Blockeinheit anhängen.

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. Überprüfen Sie die Dateisysteme auf beiden Servern. Ihre Ausgabe sollte der folgenden Ausgabe ähneln.

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

Wenn Sie eine SAP NetWeaver-basierte SAP-Anwendung für {{site.data.keyword.Db2_on_Cloud_long}} installiert haben, müssen Sie unter `/backup` Unterverzeichnisse, die dem Datenbankbenutzer mit Administratorberechtigung (`db2SID`) gehören, für umfassende Backups und archivierte Protokolldateien erstellen. Für eine automatische Archivierung der Protokolldateien sollten Sie `LOGMETH1` in der {{site.data.keyword.Db2_on_Cloud_short}}-Datenbank angeben. Details finden Sie in der [{{site.data.keyword.Db2_on_Cloud_short}}-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: new_window}.
