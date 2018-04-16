---



copyright:
  years: 2017, 2018
lastupdated: "2017-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. 파티셔닝 및 파일 시스템

3계층 예제의 경우 하나의 논리 디스크(RAID10)가 있는 256GB 서버(데이터베이스 서버) 및 하나의 논리 디스크(RAID 1)가 있는 32GB 서버(애플리케이션 서버)가 주문되었습니다. 두 서버에는 총 디스크 크기(일부 공간은 `/boot`를 위해 사용됨)와 같은 하나의 대형 루트 파일 시스템이 있습니다.

32GB 서버의 경우, SAP 설치(32GB)를 위해 서버를 준비하는 1 - 10단계를 수행한 다음, 계속해서 11 - 14단계를 수행하지만 /`db2`를 `/usr/sap/`으로 대체합니다. `sapmnt1` 및 `/usr/sap/trans`가 작성되고, 데이터베이스 서버에서 내보낸 네트워크 파일 시스템(NFS)에 ABAP(Advanced Business Application Programming) SAP Central Service[(A)SCS] 인스턴스가 보관됩니다.

다음 파일 시스템 레이아웃은 한 가지 가능한 방식입니다. 프로덕션 사용의 경우, 다른 레이아웃이 사용자의 요구 또는 SAP 요구사항을 더 많이 충족시킬 수 있으므로 시스템의 크기 정보를 따르려 할 수도 있고, 할당량을 사용하려 할 수도 있습니다.
다음 명령을 사용하여 SAP 소프트웨어 설치에 필요한 디렉토리인 `/sapmnt`, `/usr/sap` 및 `/db2`를 작성해야 합니다.
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## 다음 단계

[4. 네트워크 준비](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
