---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Particionamento e sistemas de arquivos
{: #3-partitioning-and-file-systems}

Para o exemplo de três camadas, um servidor de 192 GB (servidor de banco de dados) com um disco lógico (no RAID10) foi pedido e um servidor de 32 GB (servidor de aplicativos) com um disco lógico (no RAID 1). Ambos os servidores vêm com um grande
sistema de arquivos raiz que é igual ao tamanho total de disco (com algum espaço que é usado para `/boot`).

Para o servidor de 32 GB, crie o sistema de arquivos `/usr/sap/`. Observe que `sapmnt1` e `/usr/sap/trans` são criados no servidor de banco de dados. O Network File System (NFS) é exportado do servidor de banco de dados, que também hospeda a instância do Advanced Business Application Programming (ABAP) SAP Central Service [(A)SCS].

O layout do sistema de arquivos a seguir é uma abordagem possível. Para uso de produção, você pode desejar seguir as
informações de dimensionamento para o seu sistema, pois outros layouts podem melhor atender às suas necessidades ou requisitos
de SAP ou você pode querer usar cotas.

Os diretórios necessários para instalar o software SAP, `/sapmnt`, `/usr/sap` e
`/db2`, precisam ser criados usando os seguintes comandos:
```
[root@ sdb192 ~]# mkdir /sapmnt
[root@ sdb192 ~]# mkdir /usr/sap
[root@ sdb192 ~]# mkdir /db2
```

## Próximas etapas

[4. Preparando sua rede](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
