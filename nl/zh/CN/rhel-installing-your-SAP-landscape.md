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

# 安装 SAP 场景
{: #install_landscape}

## 安装 RPM 软件包（先决条件）
{: #RPM}

针对操作系统上安装的软件包和运行的操作系统守护程序，SAP 安装需要特定的先决条件。请参阅 SAP 的最新[安装指南 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.sap.com/software/installations.html)}: new_window （需要新的 [SAP S 用户标识](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-getting-started#getting-started) 和[支持说明 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.sap.com/notes){: new_window} （需要新的 SAP S 用户标识），以获取这些先决条件的最新列表。另外还需要安装两个软件包：
* compat-sap-c++：通常实现 C++ 运行时与 SAP 所使用的编译器的兼容性。因为选择了 Red Hat Enterprise Linux for SAP Business Application 7.X 作为 32 GB 应用程序服务器和 192 GB 数据库服务器的操作系统，因此将使用 `compat-sap-c++-7`。
* uuidd：维护操作系统对创建 UUID 的支持。

### 检查是否安装了 uuidd

1. 检查是否安装了 `uuid 守护程序 (uuidd)`。如果未安装，请进行安装并将其启动。
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### 安装软件包 compat-sap-c++-7

1. 遵循 [SAP Note 2195019 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://launchpad.support.sap.com/#/notes/2195019){: new_window}，安装软件包 compat-sap-c++-7 并创建 SAP 二进制文件所需的特定软链接。针对要安装的产品，检查特定于发行版的 SAP Note，从而确定是否需要库。
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## 下载 SAP 软件
{: download_software}

登录到 [SAP Support Portal ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.sap.com/en/index.html){: new_window}，单击 **Download Software**，然后将必需的 DVD 下载到本地共享驱动器。将文件传输到为您供应的服务器。另一个选项是下载 [SAP Software Download Manager ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://support.sap.com/en/my-support/software-downloads.html#section_995042677){: new_window}，将其安装在目标服务器上，然后直接将 DVD 映像下载到该服务器。

## 准备 SAP 的 SWPM GUI
{: #prepare_for_GUI}

根据您的网络带宽和等待时间，您可能希望在虚拟网络计算 (VNC) 会话中远程运行 SAP Software Provisioning Manager (SWPM) 图形用户界面 (GUI)。另一个选项是让 GUI 在本地运行并连接到目标计算机上的 SWPM。如果您决定在本地运行 GUI，请参阅 [SWPM 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0){: new_window}。

以下步骤概述了如何在 VNC 会话中远程运行 SWPM GUI。此选项会安装 VNC 服务器，该服务器可能不符合对操作系统进行固化的要求；请确保您符合安全措施。如果不熟悉 VNC 的功能，请参阅 [VNC 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://searchnetworking.techtarget.com/definition/virtual-network-computing){: new_window} 以了解其功能的概述。

1. 使用以下命令来安装 VNC 服务器。
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. 使用以下命令来安装 Linux 分发版中包含的 X11 窗口管理器 `twm`。

`[root@sdb192 ~]# yum install twm`

3. 安装终端仿真器，例如 `xterm`。

 `[root@sdb192 ~]# yum install xterm`

4. 从命令行启动 VNC 服务器。

 `[root@sdb192 ~]# vncserver`

现在您需要 VNC 客户机程序；有多种实现可免费用于所有操作系统。通常，您需要能够从客户机访问端口 `590X`（其中 X 是运行的服务器数量，从 1 开始）。

您可能必须从 `twm` 的后台菜单启动 `xterm`。可以从 `xterm` 启动 `SWPM` (`sapinst`)。

## 安装 SAP 软件
{: #install_software}

下载安装介质后，请执行您的 SAP 版本和组件的 SAP 安装指南以及对应 SAP 说明中记录的标准 SAP 安装过程。

可以从 `xterm` 启动 SAP SWPM，然后运行安装步骤。

### 在三层环境中安装 SAP 软件

在 SAP 的 SWPM 中执行针对三层设置的步骤。

1. 选择**分布式系统**，然后在数据库服务器上执行 ASCS 安装和数据库安装。
2. 在应用程序服务器上安装应用程序服务器 ABAP。请确保在应用程序服务器安装期间使用 ASCS 的专用地址和数据库主机名。

使用专用地址和主机名可确保应用程序服务器和 ASCS 或数据库之间的网络流量流经专用网络而非公用网络。
