---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. Preparación de la red para una configuración de tres capas
{: #network}

Si tiene previsto instalar una configuración de tres capas, deberá configurar la red correctamente. En el ejemplo, se ha desplegado un servidor de base de datos de 192 GB (denominado `sdb192`) y un servidor de aplicaciones de 32 GB (denominado `e2e1270`). El servidor de base de datos también aloja la instancia de (A)SCS. Añadir las direcciones IP de la red privada a `/etc/hosts` ayuda en los pasos siguientes y garantiza que el tráfico de red interno de SAP pase a través de la red correcta.

![Figura 1. Ejemplo de configuración de tres capas](/images/network-01.png "Ejemplo de configuración de tres capas")

Utilice los siguientes pasos para establecer su red.

1. Inicie sesión en los servidores y busque su configuración de red privada.

        [root@sdb192 ~]# ifconfig bond0
            bond0	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A8
                    inet addr:10.17.139.35  Bcast:10.17.139.63 Mask:255.255.255.192
                    inet6 addr: fe80::ec4:7aff:fe66:2da8/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:128080 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:25491 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:19137850 (18.2 MiB)  TX bytes:3426015 (3.2 MiB)

        [root@sdb192 ~]# ifconfig bond1
            bond1	  Link encap:Ethernet  HWaddr 0C:C4:7A:66:2D:A9
                    inet addr:208.43.211.118  Bcast:208.43.211.127 Mask:255.255.255.224
                    inet6 addr: fe80::ec4:7aff:fe66:2da9/64 Scope:Link
                    UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
                    RX packets:289610 errors:0 dropped:0 overruns:0 frame:0
                    TX packets:109371 errors:0 dropped:0 overruns:0 carrier:0
                    collisions:0 txqueuelen:0
                    RX bytes:31216160 (29.7 MiB)  TX bytes:18591947 (17.7 MiB)

En el ejemplo de tres capas, 10.17.139.35 es la dirección IP privada del servidor de base de datos; es de uno de los rangos de direcciones IP desde RFC 1597. También puede determinar la dirección IP del servidor de aplicaciones y añadir ambas direcciones IP a los `/etc/hosts` de ambos servidores.

        [root@sdb192 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690

        10.17.139.35    sdb192-priv
        10.17.139.46    e2e1270-priv

Añada también las dos últimas líneas `e2e1270`.

## Instalación del software NFS

1. Instale el software NFS `nfs-utils` **en ambos servidores**.

      `[root@sdb192 ~]# yum install nfs-utils`

Asegúrese de iniciar y registrar `rpcbind` y el servicio NFS en el servidor de base de datos.

        [root@sdb192 ~]# service rpcbind start
        [root@sdb192 ~]# chkconfig nfs on
        [root@sdb192 ~]# service nfs start

## Utilización de NFS para exportar

1. Utilice NFS para exportar `/sapmnt` y `/usr/sap/trans` desde el servidor de base de datos al servidor de aplicaciones añadiendo la entrada necesaria a `/etc/exports` del servidor de base de datos.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

El valor de ejemplo `C10` debe sustituirse por el ID del sistema SAP para su sistema SAP. Debe crear el directorio antes de exportarlo. Ejecute los siguientes mandatos desde la línea de mandatos del servidor de base de datos:

        [root@sdb192 ~]# mkdir /sapmnt/C10
        [root@sdb192 ~]# mkdir -p /usr/sap/trans
        [root@sdb192 ~]# exportfs -a

## Montaje del recurso compartido NFS

1. Monte el recurso compartido NFS en el servidor de aplicaciones añadiendo la siguiente entrada a `/etc/fstab` y montándolo desde la línea de mandatos.

        sdb192-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        sdb192-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. Cree los directorios de destino en el servidor de aplicaciones y móntelos.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

Sus servidores ahora están preparados para alojar los componentes de una instalación SAP distribuida.

## Siguientes pasos

  * [Adición de almacenamiento externo a {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [Instalación de software y aplicaciones de SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
