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

# SAP Landscape 설치
{: #install_landscape}

## RPM 패키지(필수 소프트웨어) 설치
{: #RPM}

SAP 설치에는 OS에 설치된 패키지 및 실행 중인 OS 디먼에 대한 특정 필수 소프트웨어가 필요합니다. 이러한 필수 소프트웨어의 최신 목록은 SAP에서 최신 [설치 안내서](https://support.sap.com/software/installations.html)([SAP S-사용자 ID](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started) 필요) 및 [지원 정보](https://support.sap.com/notes)(SAP S-사용자 ID 필요)를 참조하십시오. 설치해야 하는 패키지가 두 개 더 있습니다.
* compat-sap-c++: SAP에서 사용하는 컴파일러와 C++ 런타임의 호환을 가능하게 합니다.
* uuidd: UUID 작성에 대한 OS 지원을 유지보수합니다.

### uuidd가 설치되었는지 확인

1. `uuid daemon (uuidd)`가 설치되었는지 확인하십시오. 설치되지 않은 경우, 설치하고 시작하십시오.
```
[root@e2e2690 ~]# rpm -qa | grep uuidd
[root@e2e2690 ~]# yum install uuidd
[root@e2e2690 ~]# chkconfig uuidd on
[root@e2e2690 ~]# service uuidd start
```

### 패키지 compat-sap-c++ 설치

1. [SAP Note 2195019](https://launchpad.support.sap.com/#/notes/2195019)에 따라 패키지 compat-sap-c++를 설치하고 SAP 바이너리에 필요한 특정 소프트 링크를 작성하십시오.
```
[root@e2e2690 ~]# yum install compat-sap-c++
....
[root@e2e2690 ~]# mkdir -p /usr/sap/lib
[root@e2e2690 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## SAP 소프트웨어 다운로드
{: download_software}

[SAP Service Marketplace](https://websmp201.sap-ag.de/)에 로그인하고 필요한 DVD를 로컬 공유 드라이브에 다운로드하십시오. 프로비저닝된 서버에 파일을 전송하십시오. 다른 옵션은 [SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677)를 다운로드하고 대상 서버에 설치한 다음 서버에 DVD 이미지를 직접 다운로드하는 것입니다. 

## SAP의 SWPM GUI 준비
{: #prepare_for_GUI}

네트워크 대역폭 및 대기 시간에 따라 SAP Software Provisioning Manager(SWPM) 그래픽 사용자 인터페이스(GUI)를 가상 네트워크 컴퓨팅(VNC) 세션에서 원격으로 실행할 수 있습니다. 다른 옵션은 GUI를 로컬로 실행하고 대상 시스템의 SWPM에 연결하는 것입니다. GUI를 로컬로 실행할지 결정하는 경우 [SWPM 문서](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0)를 참조하십시오. 

다음 단계에서는 VNC 세션에서 SWPM GUI를 원격으로 실행하는 데 대해 설명합니다. 이 옵션에서는 VNC 서버를 설치하며, 이는 운영 체제 강화와는 다릅니다. 보안 조치를 충족하는지 확인하십시오. 익숙하지 않은 경우 해당 기능에 대한 개요를 보려면 [VNC 문서](http://searchnetworking.techtarget.com/definition/virtual-network-computing)를 참조하십시오.

1. 다음 명령을 사용하여 VNC 서버를 설치하십시오.
```
  [root@e2e2690 ~]# yum install tigervnc-server

  ...
```

2. 다음 명령을 사용하여 Linux 배포판에 포함된 X11 창 관리자 `twm`을 설치하십시오.

`[root@e2e2690 ~]# yum install twm`

3. 터미널 에뮬레이터(예: `xterm`)를 설치하십시오.
 
 `[root@e2e2690 ~]# yum install xterm`

4. 명령행에서 VNC 서버를 시작하십시오.
 
 `[root@e2e2690 ~]# vncserver`

이제 VNC 클라이언트 프로그램이 필요합니다. 무료로 모든 운영 체제에서 사용 가능한 여러 구현이 있습니다. 일반적으로 클라이언트에서 액세스 가능한 포트 `590X`(여기서 X는 1부터 시작되는 실행 중인 서버의 수임)가 필요합니다.

`twm`의 백그라운드 메뉴에서 `xterm`을 시작해야 합니다. `xterm`에서 `SWPM`(`sapinst`)을 시작할 수 있습니다.

## SAP 소프트웨어 설치
{: #install_software}

설치 미디어를 다운로드한 후 SAP 버전 및 컴포넌트에 대한 SAP 설치 안내서에 문서화된 표준 SAP 설치 프로시저 및 해당 SAP Note를 따르십시오.

`xterm`에서 SAP SWPM을 시작하고 설치 단계를 실행할 수 있습니다. 

### 3계층 환경에 SAP 소프트웨어 설치

3계층 설정을 위해 SAP SWPM의 단계를 수행하십시오. 

1. **분산 시스템**을 선택하고 데이터베이스 서버에서 ASCS 설치 및 데이터베이스 설치를 수행하십시오. 
2. 애플리케이션 서버에 애플리케이션 서버 ABAP를 설치하십시오. 애플리케이션 서버의 설치 중에 ASCS의 개인 주소 및 데이터베이스 호스트 이름을 사용하십시오. 

개인 주소 및 호스트 이름을 사용하면 애플리케이션 서버와 ASCS 간 네트워크 트래픽이 공용 네트워크가 아닌 사설 네트워크를 통과합니다.
