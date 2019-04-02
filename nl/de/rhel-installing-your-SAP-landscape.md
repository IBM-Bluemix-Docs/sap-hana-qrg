---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, ABAP, ASCS Instance, Database Instance, ABAP SAP Central Services, SWPM, application server, database server

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# SAP-Landschaft installieren
{: #install_landscape}

## RPM-Pakete installieren (Voraussetzung)
{: #RPM}

Für eine SAP-Installation sind bestimmte Voraussetzungen für die Pakete erforderlich, die unter dem Betriebssystem und den aktiven Betriebssystemdämonen installiert sind. Eine aktuelle Liste dieser Voraussetzungen finden Sie in den neuesten [Installationshandbüchern ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://support.sap.com/software/installations.html){: new_window} ([SAP S-User-ID](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-getting-started#getting-started) erforderlich) und bei den [Support-Hinweisen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://support.sap.com/notes){: new_window} (SAP S-User-ID erforderlich) von SAP. Es gibt noch zwei weitere Pakete, die installiert werden müssen:
* compat-sap-c++: Erzielt grundsätzlich Kompatibilität der C++-Laufzeit mit den Compilern, die von SAP verwendet werden. Da Red Hat Enterprise Linux for SAP Business Application 7.X als Betriebssystem sowohl für den 32 GB-Anwendungsserver als auch für den 192 GB-Datenbankserver ausgewählt wurde, wird `compat-sap-c++-7` verwendet.
* uuidd: Verwaltet die Unterstützung von Betriebssystemen für die Erstellung von UUIDs.

### Prüfen, ob 'uuidd' installiert ist

1. Überprüfen Sie, ob `uuid daemon (uuidd)` installiert ist. Ist dies nicht der Fall, installieren Sie den Dämon und starten Sie ihn.
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### Paket 'compat-sap-c++-7' installieren

1. Führen Sie die in [SAP Note 2195019 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://launchpad.support.sap.com/#/notes/2195019){: new_window} beschriebenen Schritte aus, installieren Sie das Paket 'compat-sap-c++-7' und erstellen Sie einen speziellen Softlink, der für die SAP-Binärdateien erforderlich ist. Lesen Sie in den releasespezifischen SAP-Hinweisen die Informationen zu dem Produkt, das Sie installieren, um festzustellen, ob die Bibliothek erforderlich ist.
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## SAP-Software herunterladen
{: download_software}

Melden Sie sich beim [SAP-Support-Portal ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://support.sap.com/en/index.html){: new_window} an, klicken Sie auf **Software herunterladen** und laden Sie die erforderlichen DVDs in ein lokales, gemeinsam genutztes Laufwerk herunter. Übertragen Sie die Dateien auf den bereitgestellten Server. Eine weitere Option ist, den [SAP Software Download Manager ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://support.sap.com/en/my-support/software-downloads.html#section_995042677){: new_window} herunterzuladen, auf dem Zielserver zu installieren und dann die DVD-Images direkt auf den Server herunterzuladen.

## Vorbereitung für die SWPM-GUI von SAP
{: #prepare_for_GUI}

Je nach Netzbandbreite und Latenz können Sie die grafische Benutzerschnittstelle (GUI) von SAP Software Provisioning Manager (SWPM) remote in einer VNC-Sitzung (VNC, Virtual Network Computing) ausführen. Eine andere Option ist, dass die GUI lokal ausgeführt und eine Verbindung zu SWPM auf dem Zielserver hergestellt wird. Wenn die GUI lokal ausgeführt werden soll, finden Sie weitere Informationen dazu in der [SWPM-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0){: new_window}.

Anhand der folgenden Schritte wird beschrieben, wie die SWPM-GUI remote in einer VNC-Sitzung ausgeführt wird. Mit dieser Option wird ein VNC-Server installiert, was möglicherweise nicht im Einklang mit der Abschottung des Betriebssystems steht. Stellen Sie daher sicher, dass Sie die entsprechenden Sicherheitsmaßnahmen erfüllen. In der [VNC-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://searchnetworking.techtarget.com/definition/virtual-network-computing){: new_window} finden Sie eine Übersicht über die Funktionen, falls Sie mit diesen nicht vertraut sind.

1. Verwenden Sie zum Installieren eines VNC-Servers die folgenden Befehle.
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. Verwenden Sie den folgenden Befehl, um den X11 Window Manager `twm` zu installieren, der in der Linux-Distribution enthalten ist.

`[root@sdb192 ~]# yum install twm`

3. Installieren Sie einen Terminalemulator, beispielsweise `xterm`.

 `[root@sdb192 ~]# yum install xterm`

4. Starten Sie den VNC-Server über die Befehlszeile.

 `[root@sdb192 ~]# vncserver`

Sie benötigen nun ein VNC-Clientprogramm. Es stehen kostenlos mehrere Implementierungen für alle Betriebssysteme zur Verfügung. In der Regel muss Port `590X` (dabei ist X die Anzahl der aktiven Server, beginnend mit 1) über den Client zugänglich sein.

Möglicherweise müssen Sie einen `xterm` über das Hintergrundmenü von `twm` starten. Sie können `SWPM` (`sapinst`) über den `xterm` starten.

## SAP-Software installieren
{: #install_software}

Nachdem Sie die Installationsmedien heruntergeladen haben, führen Sie die SAP-Standardinstallationsprozedur durch, die im SAP-Installationshandbuch für Ihre SAP-Version und -Komponenten und in den entsprechenden SAP-Hinweisen dokumentiert ist.

Sie können SAP SWPM über den `xterm` starten und die Installationsschritte ausführen.

### SAP-Software in einer Umgebung mit drei Ebenen installieren

Führen Sie die im SWPM von SAP angegebenen Schritte für ein Konfiguration mit drei Ebenen Setup aus.

1. Wählen Sie die Option **Verteiltes System** aus und führen Sie die ASCS-Installation und die Datenbankinstallation für den Datenbankserver durch.
2. Installieren Sie die ABAP für den Anwendungsserver. Sie müssen bei der Installation des Anwendungsservers private Adressen für ASCS und die Hostnamen der Datenbank verwenden.

Durch die Verwendung der privaten Adressen und Hostnamen wird sichergestellt, dass Netzverkehr zwischen dem Anwendungsserver und ASCS (oder der Datenbank) durch das private, und nicht durch das öffentliche Netz geleitet wird.
