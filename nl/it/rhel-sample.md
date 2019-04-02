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

# multipath.conf di esempio
{: #sample}

Il seguente multipath.conf di esempio è per le LUN iSCSI basate su NetApp e Red Hat 7.X.
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
Tutti i dati sotto blacklist devono essere specifici per il tuo sistema.
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

Estensioni a percorsi multipli multipath.conf di esempio per percorsi dispositivo 'leggibili':
```
	multipaths {
		multipath {
			wwid XXXXYYYZZZ
			alias pathname
		}
	}
```
