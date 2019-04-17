---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL

subcollection: sap-netweaver

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 2. 인프라 설정
{: #set_up_infrastructure}

다음 섹션에는 SAP NetWeaver {{site.data.keyword.baremetal_long}}, 네트워크, 보안, 스토리지 및 운영 체제(OS) 설정에 대한 안내가 있습니다. 질문이 있는 경우에는 [{{site.data.keyword.cloud_notm}} 지원](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)에 문의하십시오.

## 서버 주문
{: order-server}

{{site.data.keyword.baremetal_short}}를 주문하려면 다음 단계를 사용하십시오. 추가 정보는 [사용자 설치 베어메탈 서버 빌드](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server)에서 찾을 수 있습니다. 

1. 고유 인증 정보를 사용하여 [{{site.data.keyword.cloud_notm}} 인프라 고객 포털![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com){: new_window}에 로그인하십시오.
2. 계정 요약 페이지에서 **계정** > **주문하기**를 클릭하십시오.
3. 디바이스 페이지의 {{site.data.keyword.baremetal_short}} 아래에 있는 **월별** 링크를 클릭하십시오. 서버 목록 대화 상자가 표시됩니다. SAP 인증 서버가 목록 맨 위에 표시됩니다. 
4. **월당 시작 요금** 아래의 하이퍼링크를 클릭하여 적절한 서버를 선택하고 구성/주문 페이지로 이동하십시오.

BI.S3.NW32(OS 옵션) 서버 또한 **시간별** 청구용으로 사용 가능합니다.
{: note}

   SAP NetWeaver 인증 서버는 CPU 모델에서 **-NW**로 식별됩니다. Red Hat 기반 서버 구성은 [서버 옵션 선택](#select_options)의 1단계 및 7단계에 설명되어 있으며, 이들 단계는 Microsoft Windows의 경우에도 동일합니다. VMWare의 경우에는 선택사항이 제한되긴 하지만 단계는 동일합니다.

   다음은 SAP NetWeaver 서버 이름을 해독하는 방법에 대한 예제입니다.

|서버 이름| 이름 지정 규칙 컴포넌트 | 의미 |
| --- | --- | --- |
| BI.S3.NW768 | BI | Bluemix Interface |
| | S3 | Series 2(프로세서 생성) |
| | | S1은 Ivy Bridge/Haswell |
| | | S2는 Broadwell |
| | | S3는 Skylake/Kaby Lake |
| | NW | NetWeaver 인증 서버 |
| | 768 | RAM 양 |

5. **수량** 필드에 주문하는 서버의 수를 입력하고 **데이터 센터**를 선택하십시오.
6. **서버**, **RAM** 및 개인용 스토리지 선택사항 기본값은 서버 선택에 따라 달라지며 변경할 수 없습니다. {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} for {{site.data.keyword.cloud_notm}} 또는 {{site.data.keyword.filestorage_full_notm}}는 서버를 주문한 후 주문합니다.
7. Microsoft, Red Hat 또는 SUSE 중에서 **운영 체제**를 선택하고 특정 운영 체제 또는 서버용 VMware 하이퍼바이저를 선택하십시오. 

운영 체제에 대해 BYOL(Bring Your Own License)을 이용 중이면 **기타** > **운영 체제 없음**을 선택하십시오. 자세한 정보는 [BYOL(Bring Your Own License)](#byol)을 참조하십시오.
{: note}

## 서버 옵션 선택
{: #select_options}

다음 단계에서는 구성에 추가할 디스크의 유형 및 수를 선택합니다. RAID(Redundant Array of Independent Disks) 스토리지 그룹과 RAID 스토리지 그룹의 파티션 레이아웃에 대해 다양한 선택사항을 선택할 수 있습니다. 자세한 정보는 [RAID 정보](/docs/bare-metal?topic=bare-metal-about-raid#about-raid) 또는 [{{site.data.keyword.cloud_notm}} 지원](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)을 참조하십시오.

1. **SAP 인증 서버**에서 서버의 용도에 따라 선택사항을 선택하십시오. 자세한 정보는 [Design Decision Tool![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window}(아래로 스크롤하여 도구로 이동) 또는 [{{site.data.keyword.cloud_notm}} 지원](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)에 문의하십시오. 
2. 페이지 맨 아래에서 **주문에 추가** 단추를 클릭하십시오.

## 고급 시스템 구성 설정
{: #adv_config}

1. **고급 시스템 구성** 창의 값에 대해 도움을 받으려면 [고급 시스템 구성](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server) 가이드라인을 따르십시오.

SAP 호스트 이름은 최대 13자의 영숫자 문자로 구성되어야 합니다. SAP 호스트 이름 세부사항은 [SAP Notes 611361 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://launchpad.support.sap.com/#/611361){: new_window} 및 [129997 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://launchpad.support.sap.com/#/129997){: new_window}을 참조하십시오.

환경에 대해 공용/개인용 설정을 결정한 후,
  * 서로 통신해야 하는 여러 SAP 시스템을 설치하려는 경우, *또는*
  * 3계층 SAP 설정을 선택하는 경우(데이터베이스와 애플리케이션 인스턴스가 서로 다른 하드웨어에 있음),

이름 확인이 내부 및 외부 주소를 반영하는지 확인하십시오. 외부 주소는 완전한 도메인 이름(FQDN)으로 확인된 서버의 호스트 이름과 일치합니다.

내부 주소는 도메인 이름 시스템(DNS)에 표시되지 않습니다. 서버 간의 통신에는 내부 IP를 사용해야 하므로 사용자는 `/etc/hosts` 또는 Microsoft Windows "호스트" 파일을 적절하게 확장해야 합니다. 이 정보는 VMware ESXi 기반 배치의 게스트 운영 체제에도 적용됩니다.

## 선택사항 확인
{: #confirm_selections}

1. 체크아웃 페이지에서 선택사항을 확인하고 **클라우드 서비스 이용 약관** 및 **서드파티 소프트웨어 계약** 선택란을 클릭하십시오.
2. 계정 작성으로 스크롤하십시오. 비용 청구에서 비용 청구에 사용되는 신용카드의 **지불 유형, 이름, 카드** 및 **만기**(날짜)를 입력하십시오.
3. **주문 제출** 단추를 클릭하십시오. 주문 번호가 있는 화면으로 이동하게 됩니다. 화면을 인쇄할 수 있으며, 이 화면은 주문 접수증이기도 합니다.

_Your {{site.data.keyword.cloud_notm}} Order ## has been approved_라는 제목의 확인 이메일이 사용자 프로파일의 이메일 주소로 발송됩니다. 이 이메일은 서버가 승인되었으며 배치 중이라는 알림입니다. 서버가 배치되고 나면 서버가 사용 가능하며 [{{site.data.keyword.cloud_notm}} 인프라 고객 포털![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com){: new_window}을 통해 관리할 수 있음을 알리는 또 다른 알림이 발송됩니다.

## BYOL(Bring Your Own License)
{: #byol}

고유한 운영 체제 라이센스가 있는 경우에는 공급업체의 지시사항에 따라  {site.data.keyword.baremetal_short}에 이를 설치하십시오. 자세한 정보는 [OS 없음 옵션](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os)을 참조하십시오.

## 다음 단계

이제 {{site.data.keyword.baremetal_short}}의 관리를 시작할 준비가 되었습니다. 다음 단계는 [SAP NetWeaver 환경 관리](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment)를 참조하십시오.
