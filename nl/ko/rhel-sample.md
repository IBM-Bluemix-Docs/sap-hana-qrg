---



copyright:
  years: 2018
lastupdated: "2018-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 샘플 multipath.conf
{: #sample}

Red Hat 7.X 및 NetApp 기반 iSCSI LUN의 샘플 multipath.conf는 다음과 같습니다.
```
	defaults {
	        user_friendly_names no
	        max_fds max
	        flush_on_last_del yes
	        queue_without_daemon no
	        dev_loss_tmo infinity
	        fast_io_fail_tmo 5
	}
```
블랙리스트의 모든 데이터는 사용자 시스템에 한정되어야 합니다.
```
	blacklist {
	        wwid "SAdaptec*"
	        devnode "^hd[a-z]"
	        devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]*"
	        devnode "^cciss.*"
	}
	devices {
	        device {
	                vendor "NETAPP"
	                product "LUN"
	                path_grouping_policy group_by_prio
	                features "3 queue_if_no_path pg_init_retries 50"
	                prio "alua"
	                path_checker tur
	                failback immediate
	                path_selector "round-robin 0"
	                hardware_handler "1 alua"
	                rr_weight uniform
	                rr_min_io 128
	        }
	}
```

‘사람이 읽을 수 있는’ 디바이스 경로의 샘플 multipath.conf 다중 경로 확장:
```
	multipaths {
		multipath {
			wwid XXXXYYYZZZ
			alias pathname
		}
	}
```
