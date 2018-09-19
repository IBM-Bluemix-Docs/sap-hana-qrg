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

# 3. Particionamiento y sistemas de archivos
{: #partition_32GB}

Para el ejemplo de un solo nodo, el pedido es de un servidor con un disco lógico (en RAID 1). Hay un disco duplicado en el sistema operativo, con un sistema de archivos raíz grande, que es igual al tamaño total del disco (un poco de espacio se utiliza para `/boot`). El siguiente diseño de sistema de archivos es solo uno de los enfoques posibles. Para uso producción, quizá debería seguir la información de dimensionamiento para su sistema, ya que probablemente otros diseños se adaptaran mejor a sus necesidades o requisitos de SAP.

1. Cree los tres directorios necesarios para la instalación de SAP, `/sapmnt`, `usr\sap` y `/db2`.
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Su {{site.data.keyword.baremetal_long}} ya está preparado para el almacenamiento externo y la instalación de su software y aplicaciones de SAP.

## Siguientes pasos

  * [Adición de almacenamiento externo a {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-provisioning-external-storage-to-server.html)
  * [Instalación de software y aplicaciones de SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-installing-your-SAP-landscape.html)
