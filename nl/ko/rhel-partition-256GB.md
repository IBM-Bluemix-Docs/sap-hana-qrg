---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. 파티셔닝 및 파일 시스템

3계층 예제의 경우 하나의 논리 디스크(RAID10)가 있는 192GB 서버(데이터베이스 서버) 및 하나의 논리 디스크(RAID 1)가 있는 32GB 서버(애플리케이션 서버)가 주문되었습니다. 두 서버에는 총 디스크 크기(일부 공간은 `/boot`를 위해 사용됨)와 같은 하나의 대형 루트 파일 시스템이 있습니다.

32GB 서버의 경우 `/usr/sap/` 파일 시스템을 작성하십시오. `sapmnt1` 및 `/usr/sap/trans`가 데이터베이스 서버에 작성됩니다. 네트워크 파일 시스템(NFS)을 데이터베이스 서버에서 내보냅니다. 또한 네트워크 파일 시스템(NFS)에서 ABAP(Advanced Business Application Programming) SAP Central Service[(A)SCS] 인스턴스를 호스팅합니다.

다음 파일 시스템 레이아웃은 한 가지 가능한 방식입니다. 프로덕션 사용의 경우, 다른 레이아웃이 사용자의 요구 또는 SAP 요구사항을 더 많이 충족시킬 수 있으므로 시스템의 크기 정보를 따르려 할 수도 있고, 할당량을 사용하려 할 수도 있습니다.

다음 명령을 사용하여 SAP 소프트웨어 설치에 필요한 디렉토리인 `/sapmnt`, `/usr/sap` 및 `/db2`를 작성해야 합니다.
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## 다음 단계

[4. 네트워크 준비](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
