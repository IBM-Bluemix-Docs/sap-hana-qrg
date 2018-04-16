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

# 3. 파티셔닝 및 파일 시스템
{: #partition_32GB}

단일 노드 예제의 경우 하나의 논리 디스크(RAID1)가 있는 서버를 주문했습니다. 총 디스크 크기(일부 공간은 `/boot`를 위해 사용됨)와 같은 하나의 대형 루트 파일 시스템이 있는 하나의 미러링된 디스크가 운영 체제(OS)에 나타납니다. 다음 파일 시스템 레이아웃은 단지 하나의 가능한 방식입니다. 프로덕션 사용의 경우, 다른 레이아웃이 사용자의 요구 또는 SAP 요구사항을 더 많이 충족시킬 수 있으므로 시스템의 크기 정보를 따르려 할 수도 있습니다. 

SAP 설치에 필요한 세 개의 디렉토리, `/usr/sap`, `/sapmnt` 및 `/db2`가 작성되었습니다.
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
{{site.data.keyword.baremetal_long}}는 이제 SAP 애플리케이션과 소프트웨어 설치 및 외부 스토리지를 위해 준비가 되었습니다.

## 다음 단계

  * [{{site.data.keyword.baremetal_short}}에 외부 스토리지 추가](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [SAP 애플리케이션과 소프트웨어 설치](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
