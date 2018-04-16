---



copyright:
  years: 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. 32GB 서버 주문
{: #install_32GB}

## 서버 주문
{: #order_32GB}

1. 고유 신임 정보를 사용하여 [{{site.data.keyword.cloud_notm}} 인프라 고객 포털](https://control.softlayer.com)에 로그인하십시오.
2. 계정 요약 페이지에서 **디바이스**를 클릭하십시오.
3. 디바이스 페이지의 {{site.data.keyword.baremetal_short}}에서 **월별**을 클릭하십시오. 서버 목록 대화 상자가 나타납니다.
4. **월별 시작 가격**에서 하이퍼링크를 클릭하여 서버 **BI.S1.NW32(OS 옵션)**를 선택하십시오.

## 서버 옵션 선택
{: #options_32GB}

1. **수량** 필드에 **1**을 그대로 두십시오.
2. **데이터 센터**에 대해 **TOR01**을 선택하십시오. 데이터 센터의 목록은 특정 데이터 센터 내 제품 가용성에 따라 다릅니다.
3. **서버**의 기본값은 서버 선택사항에 따라 사전 정의된 값으로 설정되며 변경될 수 없습니다.
4. **RAM** 선택사항의 기본값이 서버 선택사항에 따라 사전 정의된 값으로 설정되고 변경될 수 없더라도 **32GB RAM**을 클릭하십시오.
5. **Red Hat Enterprise Linux for SAP Business Application 6.X**를 **운영 체제**로 선택하십시오.
6. **하드 드라이브**에서 두 번째 2TB SATA 디스크를 선택하고, 두 디스크에서 총 스토리지 크기를 포함하는 RAID1이라는 RAID 스토리지 그룹을 작성하고, **Linux Basic**을 **파티션 템플리트**로 선택하십시오. **LVM**은 선택하지 않은 채로 두십시오.
7. **공용 대역폭**에 대해 **500GB**를 선택하십시오.
8.	**업링크 포트 속도**에 대해 **1Gbps 중복 공용 및 사설 네트워크 업링크**를 선택하십시오.
9. 다른 모든 필드는 기본값을 그대로 두십시오. 자세한 옵션 설명은 [베어메탈 서버 설정](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers)을 참조하십시오.
10.	페이지의 맨 아래에서 **주문에 추가**를 클릭하십시오. 주문이 확인되면 체크아웃 페이지로 이동하게 됩니다.

## 고급 시스템 구성 설정
{: #adv_config}

고급 시스템 구성 아래 필드에 표 1의 값을 사용하십시오. 자세한 정보는 [고급 시스템 구성](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration) 가이드라인에서 볼 수 있습니다.

1. 아래로 스크롤하고 **고급 시스템 구성**에서 표 1의 값을 입력하십시오.

|              필드                |      값                                                              |
| -------------------------------- | -------------------------------------------------------------------- |
|백엔드 VLAN                       | 드롭 다운 목록에서 선택하십시오(예: `tor01.bcr01a.1241`).            |
|서브넷                            | 드롭 다운 목록에서 선택하십시오(예: `10.114.63.64/26`).              |
|프론트 엔드 VLAN                  | 드롭 다운 목록에서 선택하십시오(예: `tor01.fcr01a.1199`).            |
|서브넷                            | 드롭 다운 목록에서 선택하십시오(예: `169.55.137.160/27`).            |
|프로비저닝 스크립트               | 없음으로 기본값 설정                                                 |
|SSH 키                            | 추가                                                                 |
|호스트 이름                       | 예: `e2e1270`                                                        |
|도메인                            | 예: `saptest.com`                                                    |
{: caption="표 1. 32GB 고급 구성 값" caption-side="top"}  

## 선택사항 확인
{: #confirm_selections}

1. 체크아웃 페이지에서 선택사항을 확인하고 페이지의 오른쪽에 있는 **클라우드 서비스 이용 약관** 및 **써드파티 소프트웨어 계약**을 클릭하십시오.
2. 양식의 오른쪽에 있는 **주문 제출**을 클릭하십시오. 주문 번호가 있는 페이지로 이동하게 됩니다. 페이지를 인쇄할 수 있으며, 이 페이지는 주문 접수증이기도 합니다. 또한 제목이 *Your IBM Cloud Order ## has been approved*(##은 주문 번호임)인 확인 이메일을 수신하게 됩니다.

주문이 제출된 후 주문에 따라 1 - 4시간 내에 서버를 사용할 수 있습니다. 기본 고객 포털 페이지의 디바이스 세부사항 화면(**디바이스 > 디바이스 목록**)에서 프로비저닝 단계의 상태를 확인할 수 있습니다. 지정된 호스트 이름 및 도메인과 일치하는 **디바이스 이름**을 클릭하여 해당 상태를 확인하십시오.

## 다음 단계
 
  [2. SAP 설치를 위한 서버 준비](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)
  
  [3. 파티셔닝 및 파일 시스템](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
