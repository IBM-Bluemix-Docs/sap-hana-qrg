---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, multipath.conf

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 範例 multipath.conf
{: #sample}

下列範例 multipath.conf 用於 Red Hat 7.X 和以 NetApp 為基礎的 iSCSI LUN。
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
黑名單上的所有資料都必須是您系統特有的。
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

「人類可讀」裝置路徑的範例 multipath.conf 多路徑延伸：
```
	multipaths {
		multipath {
			wwid XXXXYYYZZZ
			alias pathname
		}
	}
```
