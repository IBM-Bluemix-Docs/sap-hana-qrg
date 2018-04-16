---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-22"


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

Für eine SAP-Installation sind bestimmte Voraussetzungen für die Pakete erforderlich, die unter dem Betriebssystem und den aktiven Betriebssystemdämonen installiert sind. Eine aktuelle Liste dieser Voraussetzungen finden Sie in den neuesten [Installationshandbüchern](https://support.sap.com/software/installations.html) ([SAP S-User-ID](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started) erforderlich) und [Support-Hinweisen](https://support.sap.com/notes) (SAP S-User-ID erforderlich) von SAP. Es gibt noch zwei weitere Pakete, die installiert werden müssen:
* compat-sap-c++: Erzielt Kompatibilität der C++-Laufzeit mit den Compilern, die von SAP verwendet werden.
* uuidd: Verwaltet die Unterstützung von Betriebssystemen für die Erstellung von UUIDs.

### Prüfen, ob 'uuidd' installiert ist

1. Überprüfen Sie, ob `uuid daemon (uuidd)` installiert ist. Ist dies nicht der Fall, installieren Sie den Dämon und starten Sie ihn.
```
[root@e2e2690 ~]# rpm -qa | grep uuidd
[root@e2e2690 ~]# yum install uuidd
[root@e2e2690 ~]# chkconfig uuidd on
[root@e2e2690 ~]# service uuidd start
```

### Paket 'compat-sap-c++' installieren

1. Befolgen Sie den [SAP-Hinweis 2195019](https://launchpad.support.sap.com/#/notes/2195019) und installieren Sie das Paket 'compat-sap-c++' und erstellen Sie einen bestimmten Softlink, der von SAP-Binärdateien benötigt wird.
```
[root@e2e2690 ~]# yum install compat-sap-c++
....
[root@e2e2690 ~]# mkdir -p /usr/sap/lib
[root@e2e2690 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## SAP-Software herunterladen
{: download_software}

Melden Sie sich beim [SAP Service Marketplace](https://websmp201.sap-ag.de/) an und laden Sie die erforderlichen DVDs auf ein lokales, gemeinsam genutztes Laufwerk herunter. Übertragen Sie die Dateien auf den bereitgestellten Server. Eine weitere Option ist, den [SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677) herunterzuladen, auf dem Zielserver zu installieren und dann die DVD-Images direkt auf den Server herunterzuladen. 

## Vorbereitung für die SWPM-GUI von SAP
{: #prepare_for_GUI}

Je nach Netzbandbreite und Latenz können Sie die grafische Benutzerschnittstelle (GUI) von SAP Software Provisioning Manager (SWPM) remote in einer VNC-Sitzung (VNC, Virtual Network Computing) ausführen. Eine andere Option ist, dass die GUI lokal ausgeführt und eine Verbindung zu SWPM auf dem Zielserver hergestellt wird. Wenn die GUI lokal ausgeführt werden soll, finden Sie weitere Informationen dazu in der [SWPM-Dokumentation](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0). 

Anhand der folgenden Schritte wird beschrieben, wie die SWPM-GUI remote in einer VNC-Sitzung ausgeführt wird. Mit dieser Option wird ein VNC-Server installiert, was möglicherweise nicht im Einklang mit der Abschottung des Betriebssystems steht. Stellen Sie daher sicher, dass Sie die entsprechenden Sicherheitsmaßnahmen erfüllen. In der [VNC-Dokumentation](http://searchnetworking.techtarget.com/definition/virtual-network-computing) finden Sie eine Übersicht über die Funktionen, falls Sie mit diesen nicht vertraut sind.

1. Verwenden Sie zum Installieren eines VNC-Servers die folgenden Befehle.
```
  [root@e2e2690 ~]# yum install tigervnc-server

  ...
```

2. Verwenden Sie den folgenden Befehl, um den X11 Window Manager `twm` zu installieren, der in der Linux-Distribution enthalten ist.

`[root@e2e2690 ~]# yum install twm`

3. Installieren Sie einen Terminalemulator, beispielsweise `xterm`.
 
 `[root@e2e2690 ~]# yum install xterm`

4. Starten Sie den VNC-Server über die Befehlszeile.
 
 `[root@e2e2690 ~]# vncserver`

Sie benötigen nun ein VNC-Clientprogramm. Es stehen kostenlos mehrere Implementierungen für alle Betriebssysteme zur Verfügung. In der Regel muss Port `590X` (dabei ist X die Anzahl der aktiven Server, beginnend mit 1) über den Client zugänglich sein.

Möglicherweise müssen Sie einen `xterm` über das Hintergrundmenü von `twm` starten. Sie können `SWPM` (`sapinst`) über den `xterm` starten.

## SAP-Software installieren
{: #install_software}

Nachdem Sie die Installationsmedien heruntergeladen haben, führen Sie die SAP-Standardinstallationsprozedur durch, die im SAP-Installationshandbuch für Ihre SAP-Version und -Komponenten und in den entsprechenden SAP-Hinweisen dokumentiert ist.

Sie können SAP SWPM über den `xterm` starten und die Installationsschritte ausführen. 

### SAP-Software in einer dreischichtigen Umgebung installieren

Führen Sie die im SWPM von SAP angegebenen Schritte für ein dreischichtiges Setup aus. 

1. Wählen Sie die Option **Verteiltes System** aus und führen Sie die ASCS-Installation und die Datenbankinstallation für den Datenbankserver durch. 
2. Installieren Sie die ABAP für den Anwendungsserver. Sie müssen bei der Installation des Anwendungsservers private Adressen für ASCS und die Hostnamen der Datenbank verwenden. 

Durch die Verwendung der privaten Adressen und Hostnamen wird sichergestellt, dass Netzverkehr zwischen dem Anwendungsserver und ASCS (oder der Datenbank) durch das private, und nicht durch das öffentliche Netz geleitet wird.
