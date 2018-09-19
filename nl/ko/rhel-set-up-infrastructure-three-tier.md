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

# 1. 3계층 설정을 위한 192GB 및 32GB 서버 주문
{: #install_three_tier}

## 서버 주문
{: #order_servers}

[32GB 서버 주문](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB)의 단계를 수행하여 SAP NetWeaver 애플리케이션 서버를 주문하십시오. 다음 단계를 통해 데이터베이스 서버를 주문할 수 있습니다.

## 데이터베이스 서버 주문
{: #order-db-server}

1. 고유 신임 정보를 사용하여 [{{site.data.keyword.cloud_notm}} 인프라 고객 포털](https://control.softlayer.com)에 로그인하십시오.
2. 계정 요약 페이지에서 **계정** > **주문하기**를 클릭하십시오.
3. 디바이스 페이지의 **{{site.data.keyword.baremetal_long}}**에서 **월별**을 클릭하십시오. 서버 목록이 표시됩니다. SAP 인증 서버가 목록의 맨 위에 있습니다.
4. **월별 시작 가격**에서 하이퍼링크를 클릭하여 서버 **BI.S3.NW192(OS 옵션)**를 선택하십시오.

## 데이터베이스 서버 구성
{: #options_192GB}

1. **수량** 필드에 **1**을 그대로 두십시오.
2. **데이터 센터**에 대해 **TOR01**을 선택하십시오.
3. **서버**의 기본값은 서버 선택사항에 따라 사전 정의된 값으로 설정되며 변경될 수 없습니다.
4. **RAM** 선택사항의 기본값은 서버 선택사항에 따라 사전 정의된 값으로 설정되고 변경될 수 없지만 **192GB RAM**을 클릭하십시오.
5. **Redhat**을 클릭하고 **Red Hat Enterprise Linux for SAP Business Application 7.X(64비트)**를 **운영 체제**로 선택하십시오. **참고**: 운영 체제에 대해 BYOL(Bring Your Own License)을 사용하는 경우 **기타** > **운영 체제 없음**을 선택하십시오. 자세한 정보는 [BYOL(Bring Your Own License)](#byol)을 참조하십시오.
6. **디스크 제어기 1** 드롭 다운 메뉴를 클릭하고 **2TB SATA**를 선택하여 두 번째 2TB SATA 드라이브를 추가하십시오. **디스크 추가**를 클릭하십시오.
7. **모든 디스크 선택**을 클릭하고 **RAID 스토리지 그룹 작성**을 클릭하십시오.
8. **유형**을 클릭하고 **RAID 1**을 선택하십시오. 필요한 스토리지 총계를 포함하는 **크기**를 입력하십시오.
9. **LVM**을 선택하지 않은 상태로 두고 기본 **파티션 템플리트**인 **Linux Basic**을 승인하십시오.
10. **완료**를 클릭하십시오.

## 추가 데이터베이스 서버 옵션 선택
{: #addl-server-options}

1. **퍼블릭 데이터 전송량**에 대해 **500GB**를 선택하십시오.
2. **업링크 포트 속도**에 대해 **1Gbps 이중화된 퍼블릭 및 프라이빗 네트워크 업링크**를 선택하십시오.
3. 이 예제의 경우, 다른 모든 필드는 기본값을 그대로 두십시오. 옵션에 대한 자세한 설명은 [사용자 정의 베어메탈 서버 빌드](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options)를 참조할 수 있습니다.
4.	페이지의 맨 아래에서 **주문에 추가**를 클릭하십시오. 주문이 확인되면 체크아웃 페이지로 이동하게 됩니다.

## 고급 시스템 구성 설정
{: #adv_config}

1. 고급 시스템 구성 아래 필드에 표 1의 값을 사용하십시오. 자세한 정보는 [고급 시스템 구성](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config) 가이드라인에서 볼 수 있습니다.

|              필드                |        값                                                            |
| -------------------------------- | -------------------------------------------------------------------- |
|백엔드 VLAN                       | 드롭 다운 목록에서 선택하십시오(예: `tor01.bcr01a.1241`).            |
|서브넷                            | 드롭 다운 목록에서 선택하십시오(예: `10.114.63.64/26`).              |
|프론트 엔드 VLAN                  | 드롭 다운 목록에서 선택하십시오(예: `tor01.fcr01a.851`).             |
|서브넷                            | 드롭 다운 목록에서 선택하십시오(예: `158.85.65.224/28`).             |
|프로비저닝 스크립트               | 공백으로 두기                                                        |
|SSH 키                            | 기본적으로 SSH 키가 없음을 의미하는 `Add`로 설정됨                   |
|호스트 이름                       | 예: `sdb192`                                                         |
|도메인                            | 예: `saptest.com`                                                    |
{: caption="표 1. 192GB 고급 구성값" caption-side="top"}  

## 선택사항 확인
{: #confirm_selections}

1. 체크아웃 페이지에서 선택사항을 확인하고 페이지의 오른쪽에 있는 **클라우드 서비스 조항** 및 **써드파티 소프트웨어 계약**을 클릭하십시오.
2. **주문 제출**을 클릭하십시오. 주문 번호가 있는 화면으로 이동하게 됩니다. 화면을 인쇄할 수 있으며, 이 화면은 주문 접수증이기도 합니다.

주문이 제출된 후 주문에 따라 1 - 4시간 내에 서버를 사용할 수 있습니다. 기본 {{site.data.keyword.slportal}} 페이지의 디바이스 세부사항 화면(**디바이스 > 디바이스 목록**)에서 프로비저닝 단계의 상태를 확인할 수 있습니다. 지정된 호스트 이름 및 도메인과 일치하는 **디바이스 이름**을 클릭하여 해당 상태를 확인하십시오.

## BYOL(Bring Your Own License)
{: #byol}

자체 운영 체제 라이센스가 있는 경우 벤더의 지시사항에 따라 {{site.data.keyword.baremetal_short}}에 이를 설치합니다. 자세한 정보는 [OS 없음 옵션](https://console.bluemix.net/docs/bare-metal/introduction-no-os.html#how-to-install-an-operating-system-on-a-no-os-server-)을 참조하십시오.

## 다음 단계

  [2. SAP 설치를 위한 서버 준비](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)

  [3. 파티셔닝 및 파일 시스템](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)

  [4. 네트워크 준비](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
