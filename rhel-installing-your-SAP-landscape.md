---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Installing your SAP landscape
{: #install_landscape}

## Installing RPM packages (prerequisite)
{: #RPM}

An SAP installation requires certain prerequisites for the packages that are installed on the OS and the OS daemons running. Refer to the latest [installation guides](https://support.sap.com/software/installations.html) (requires an [SAP S-user ID](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started)) and [support notes](https://support.sap.com/notes) (requires an SAP S-user ID) from SAP for an up-to-date list of these prerequisites. There are two more packages that need to be installed:
* compat-sap-c++: Generally achieves compatibility of the C++ runtime with the compilers that are used by SAP. Because Red Hat Enterprise Linux for SAP Business Application 7.X was seelected as the OS for both the  32 GB application server and the 192 GB database server, you will use `compat-sap-c++-7`.
* uuidd: Maintains OS support for the creation of UUIDs.

### Checking if uuidd is installed

1. Check if `uuid daemon (uuidd)` is installed. If it is not, install and start it.
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### Installing package compat-sap-c++-7

1. Follow [SAP Note 2195019](https://launchpad.support.sap.com/#/notes/2195019) and install package compat-sap-c++-7 and create a specific soft-link, which is required by the SAP binaries. Check the release-specific SAP Notes for the product you are installing to determine if the library is required or not.
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## Downloading your SAP software
{: download_software}

Log in to the [SAP Support Portal](https://support.sap.com/en/index.html), click **Download Software**, and download the required DVDs to a local share drive. Transfer the files to your provisioned server. Another option is to download the [SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677), install it on your target server and directly download the DVD images to the server.

## Preparing for SAP's SWPM GUI
{: #prepare_for_GUI}

Depending on your network bandwidth and latency, you might want to run the SAP Software Provisioning Manager (SWPM) graphical user interface (GUI) remotely in a virtual network computing (VNC) session. Another option is to have the GUI running locally and connect to SWPM on the target machine. Consult the [SWPM documentation](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0) if you decide to run the GUI locally.

The following steps outline running the SWPM GUI remotely in a VNC session. This option installs a VNC server, which might not be inline with hardening your operating system; ensure that you are meeting your security measures. Refer to [VNC documentation](http://searchnetworking.techtarget.com/definition/virtual-network-computing) to get an overview on its functions, if you are not familiar with it.

1. Use the following commands to install a VNC server.
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. Use the following command to install the X11 window manager, `twm`, which is included in the Linux distribution.

`[root@sdb192 ~]# yum install twm`

3. Install a terminal emulator, for example, `xterm`.

 `[root@sdb192 ~]# yum install xterm`

4. Start the VNC server from the command line.

 `[root@sdb192 ~]# vncserver`

You now require a VNC client program; there are multiple implementations available for all operating systems at no cost. Typically, you need port `590X` (where X is the number of the servers running, starting at 1) to be accessible from your client.

You might have to start an `xterm` from the background menu of `twm`. You can start `SWPM` (`sapinst`) from the `xterm`.

## Installing SAP software
{: #install_software}

After downloading the installation media, follow the standard SAP installation procedure that is documented in the SAP installation guide for your SAP version and components, and the corresponding SAP Notes.

You can start SAP SWPM from the `xterm`, and run the installation steps.

### Installing the SAP software in a three-tier environment

Follow the steps in SAP's SWPM for a three-tier setup.

1. Select **Distributed System** and perform the ASCS installation and database installation on the database server.
2. Install the Application Server ABAP on the application server. Be sure to use the private addresses for the ASCS and the database host names during installation of the application server.

The use of the private addresses and host names assures that network traffic between the application server and ASCS, or database, passes through the private network and not through the public network.
