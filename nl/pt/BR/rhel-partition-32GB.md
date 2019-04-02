---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Particionamento e sistemas de arquivos
{: #partition_32GB}

Para o exemplo de único nó, você solicitou um servidor com um disco lógico (em RAID 1). Há um disco espelhado que aparece no
sistema operacional (SO) com um grande sistema de arquivos raiz igual ao tamanho total do disco (com algum espaço usado para
`/boot`). O layout do sistema de arquivos a seguir tem apenas uma abordagem possível. Para uso de produção, você
pode desejar seguir as informações de dimensionamento para o seu sistema, pois outros layouts podem melhor atender suas
necessidades ou requisitos do SAP.

1. Crie os três diretórios necessários para a instalação do SAP, `/sapmnt`, `usr\sap` e `/db2`.
```
[root@e2e1270 ~]# mkdir /sapmnt
[root@e2e1270 ~]# mkdir /usr/sap
[root@e2e1270 ~]# mkdir /db2
```
Agora, o seu {{site.data.keyword.baremetal_long}} está pronto para armazenamento externo e a instalação de seus
aplicativos e software SAP.

## Próximas etapas

  * [Incluindo armazenamento externo em seu {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-storage)
  * [Instalando sua paisagem do SAP (aplicativos e software)](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_landscape)
