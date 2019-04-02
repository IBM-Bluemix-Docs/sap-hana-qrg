---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, ABAP, ASCS instance, ABAP SAP Central Services, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. 3계층 설정을 위한 네트워크 준비
{: #network}

3계층 설정을 설치하려는 경우 네트워크를 올바르게 설정해야 합니다. 예제에서는 192GB 데이터베이스 서버(`sdb192`)와 32GB 애플리케이션 서버(`e2e1270`)가 배치되었습니다. 또한 데이터베이스 서버는 (A)SCS 인스턴스를 호스팅합니다. 사설 네트워크의 IP 주소를 `/etc/hosts`에 추가하면 다음 단계에 도움이 되며 SAP 내부 네트워크 트래픽이 올바른 네트워크를 거치게 됩니다.

![그림 1. 3계층 설정 샘플](/images/network-01.png "3계층 설정 샘플")

다음 단계를 사용하여 네트워크를 설정하십시오.

1. 서버에 로그인하고 사설 네트워크 구성을 찾으십시오.

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

3계층 예제에서 10.17.139.35는 데이터베이스 서버의 사설 IP 주소이며, RFC 1597의 IP 주소 범위 중 하나입니다. 애플리케이션 서버의 IP 주소를 판별할 수 있으며 두 IP 주소를 두 서버의 `/etc/hosts`에 추가할 수 있습니다.

        [root@sdb192 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690

        10.17.139.35    sdb192-priv
        10.17.139.46    e2e1270-priv

`e2e1270`에 마지막 두 행도 추가하십시오.

## NFS 소프트웨어 설치

1. **두 서버에** NFS 소프트웨어 `nfs-utils`를 설치하십시오.

      `[root@sdb192 ~]# yum install nfs-utils`

데이터베이스 서버에서 `rpcbind` 및 NFS 서비스를 시작하고 등록하십시오.

        [root@sdb192 ~]# service rpcbind start
        [root@sdb192 ~]# chkconfig nfs on
        [root@sdb192 ~]# service nfs start

## 내보내기에 NFS 사용

1. 필수 항목을 데이터베이스 서버의 `/etc/exports`에 추가하여 NFS를 통해 데이터베이스 서버에서 애플리케이션 서버로 `/sapmnt` 및 `/usr/sap/trans`를 내보내십시오.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

샘플 값 `C10`은 SAP 시스템의 SAP 시스템 ID로 대체해야 합니다. 내보내기 전에 디렉토리를 작성해야 합니다. 데이터베이스 서버의 명령행에서 다음 명령을 실행하십시오.

        [root@sdb192 ~]# mkdir /sapmnt/C10
        [root@sdb192 ~]# mkdir -p /usr/sap/trans
        [root@sdb192 ~]# exportfs -a

## NFS 공유 마운트

1. 다음 항목을 해당 `/etc/fstab`에 추가하고 명령행에서 이를 마운트하여 애플리케이션 서버에서 NFS 공유를 마운트하십시오.

        sdb192-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        sdb192-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. 애플리케이션 서버에서 대상 디렉토리를 작성하고 마운트하십시오.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

서버는 이제 분산 SAP 설치의 컴포넌트를 호스팅할 준비가 되었습니다.

## 다음 단계

  * [{{site.data.keyword.baremetal_short}}에 외부 스토리지 추가](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-storage)
  * [SAP 랜드스케이프 설치(애플리케이션 및 소프트웨어)](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_landscape)
