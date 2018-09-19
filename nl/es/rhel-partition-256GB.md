---



copyright:
  years: 2017, 2018
lastupdated: "2017-08-13"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Particionamiento y sistemas de archivos

Para el ejemplo de tres capas, se realizó un pedido de un servidor de 192 GB (servidor de base de datos) con un disco lógico (en RAID10) y un servidor de 32 GB (servidor de aplicaciones) con un disco lógico (en RAID 1). Ambos servidores incluyen un sistema de archivos raíz grande, que es igual al tamaño total del disco (un poco de espacio se utiliza para `/boot`).

Para el servidor de 32 GB, cree el sistema de archivos `/usr/sap/`. Tenga en cuenta que `sapmnt1` y `/usr/sap/trans` se crean en el servidor de base de datos. El sistema de archivos de red (NFS) se exporta desde el servidor de base de datos, que también aloja la instancia Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS].

El siguiente diseño para el sistema de archivos es uno de los enfoques posibles. Para uso producción, quizá debería seguir la información de dimensionamiento para su sistema, ya que probablemente otros diseños se adaptaran mejor a sus necesidades o requisitos de SAP, o es posible que desee utilizar cuotas.

Los directorios necesarios para instalar el software de SAP, `/sapmnt`, `/usr/sap` y `/db2`, tienen que crearse utilizando los siguientes mandatos:
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## Siguientes pasos

[4. Preparación de la red](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
