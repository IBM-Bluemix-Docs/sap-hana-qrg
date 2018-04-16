---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 서버에 외부 스토리지 추가
{: #storage}

## 외부 스토리지 설정
{: #set_up_storage}

외부 스토리지를 백업 디바이스로 사용하거나 스냅샷을 사용하여 테스트 환경에서 신속하게 데이터베이스를 복원하려는 경우 프로비저닝된 서버에 외부 스토리지를 추가할 수 있습니다. 3계층 예제에서 블록 스토리지는 데이터베이스의 로그 파일을 아카이브하고 데이터베이스의 온라인 및 오프라인 백업을 수행하는 데 사용됩니다. 가장 빠른 블록 스토리지(GB당 4IOPS)는 최단 백업 시간을 보장하도록 선택되었습니다. 느린 블록 스토리지가 사용자 요구에 맞을 수도 있습니다. {{site.data.keyword.blockstoragefull}}에 대한 자세한 정보는 [블록 스토리지 시작하기](https://console.bluemix.net/docs/infrastructure/BlockStorage/index.html#getting-started-with-block-storage)를 참조하십시오.


1. 고유 신임 정보를 사용하여 [{{site.data.keyword.cloud_notm}} 인프라 고객 포털](https://control.softlayer.com/)에 로그인하십시오.
2. **스토리지 > 블록 스토리지**를 선택하십시오.
3. 블록 스토리지 페이지의 오른쪽 상단 구석에 있는 **블록 스토리지 주문**을 클릭하십시오.
4. 스토리지 요구에 맞는 사양을 선택하십시오. 표 1에는 일반 데이터베이스 워크로드에 대한 4IOPS/GB를 포함하여 권장되는 값이 나와 있습니다.

|              필드                |      값                                           |
| -------------------------------- | ------------------------------------------------- |
|스토리지 유형 선택                | 내구성(기본값)                                    |
|위치                              | TOR01                                             |
|대금 청구 방법                    | 월별(기본값)                                      |
|스토리지 패키지 선택              | 4IOPS/GB                                          |
|스토리지 크기 선택                | 1000GB                                            |
|스냅샷 공간 크기 지정             | 0GB                                               |
|OS 유형 선택                      | Linux(기본값)                                     |
{: caption="표 1. 블록 스토리지의 권장 값" caption-side="top"}

5. **계속**을 클릭하십시오.
6. **마스터 서비스 계약을 읽었음**을 선택하고 **주문하기**를 클릭하십시오.
7. LUN의 오른쪽에 있는 **조치**를 클릭하고 **호스트 권한 부여**를 선택하여 프로비저닝된 스토리지에 액세스하십시오.
8. **디바이스**를 선택하십시오. **디바이스 유형**의 기본값은 베어메탈 서버로 설정됩니다. **하드웨어**를 클릭하고 디바이스의 호스트 이름을 선택하십시오.
9. **제출** 단추를 클릭하십시오.
10. **디바이스** > (디바이스 선택) > **스토리지 탭**에서 프로비저닝된 스토리지의 상태를 확인하십시오.
11. 서버(iSCSI 개시자)의 **대상 주소** 및 **IQN**(iSCSI Qualified Name)과 iSCSI 서버의 권한 부여를 위한 **사용자 이름** 및 **비밀번호**를 기록해 두십시오. 다음 단계에서 이 정보가 필요합니다.
12. [Linux의 MPIO iSCSI LUN에 연결](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#connecting-to-mpio-iscsi-luns-on-linux)의 단계를 수행하여 스토리지가 프로비저닝된 서버에서 액세스 가능하도록 설정하십시오.

## 스토리지를 다중 경로로 설정
{: #multipath}

샘플 배치에서 **스토리지** 탭을 사용하여 다음 데이터를 검색했습니다.
  * 대상 IP: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * 사용자: `SL01SU276540-H896345`
  * 비밀번호: `EtJ79F4RA33dXm2q`

1. 검색된 정보에 따라 다음을 입력하십시오.
```
[root@e2e2690 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
``` 
   `/etc/iscsi/initiatorname.iscsi`에서 기존 항목을 바꿔야 할 수 있습니다.

2. `/etc/iscsi/iscsid.conf`의 맨 아래에 다음 행을 추가하십시오.
```
[root@e2e2690 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. 2단계의 `username` 및 `password` 값을 외부 스토리지 설정의 11단계에서 기록한 값으로 대체하십시오.

4. 다음 행을 입력하여 iSCSI 대상을 검색하십시오.
```
[root@e2e2690 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. iSCSI 배열에 자동으로 로그인하도록 호스트를 설정하십시오.

      `[root@e2e2690 ~]# iscsiadm -m node -L automatic`

6. 다중 경로 디먼을 설치하고 시작하십시오.
```
[root@e2e2690 ~]# yum install device-mapper-multipath
…
[root@e2e2690 ~]# chkconfig multipathd on
[root@e2e2690 ~]# service multipathd start
```

7. 다른 LUN이 다중 경로 출력에 나타나도록 [Linux에 블록 스토리지 볼륨 마운트](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#mounting-block-storage-volumes)의 모든 명령을 완료하십시오.
```
[root@e2e2690 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

이제 디스크 디바이스를 사용할 때 다중 경로 디바이스를 사용할 수 있습니다. 디바이스 경로는 `/dev/mapper/3600a098038303452543f464142755a42` 아래에 표시됩니다.

[`multipath.conf` 예제](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-sample.html#sample)에서 `/etc/multipath.conf` 샘플을 가져와 서버에 작성하십시오. 복사된 특수 문자, DOS와 같은 캐리지 리턴, 줄 바꾸기 항목으로 인해 예상치 못한 동작이 나타날 수 있습니다. 컨텐츠를 복사한 후 ASCII Unix 파일이 있는지 확인하십시오.

`/etc/multipath.conf`에서 다중 경로 블록을 조정하여 `1/dev/mapper/mpath1` 아래에서 디바이스에 액세스하는 경로의 별명을 작성하십시오.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. `multipathd`를 다시 시작하십시오. 이제 `/backup filesystem`을 작성하고 블록 디바이스에 마운트할 수 있습니다.
        
      [root@e2e2690 ~]# service multipathd restart
      [root@e2e2690 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@e2e2690 ~]# mkdir  /backup

9. 두 서버에서 파일 시스템을 검사하십시오. 출력은 다음과 유사합니다.

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db2690-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db2690-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@e2e2690 ~]# df -h
        Filesystem      	    Size  Used Avail Use% Mounted on
        /dev/sda3             549G  2,3G  519G   1% /
        tmpfs                 127G     0  127G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/mapper/mpath1    493G   70M  468G   1% /backup
        /dev/mapper/datavg-datalv
                      1,2T   71M  1,1T   1% /db2
        /dev/mapper/datavg-saplv
                      165G   60M  157G   1% /usr/sap
        /dev/mapper/datavg-sapmntlv
                      165G   60M  157G   1% /sapmnt

{{site.data.keyword.Db2_on_Cloud_long}}에 SAP NetWeaver 기반 SAP 애플리케이션을 설치하는 경우, 전체 백업과 아카이브된 로그 파일을 위해 데이터베이스 관리자(`db2SID`)가 소유한 `/backup` 아래에 서브디렉토리를 작성해야 합니다. 로그 파일 자동 아카이브를 위해서는 {{site.data.keyword.Db2_on_Cloud_short}} 데이터베이스에 `LOGMETH1`을 설정해야 합니다. 세부사항은 [{{site.data.keyword.Db2_on_Cloud_short}} 문서](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html)를 참조하십시오.
