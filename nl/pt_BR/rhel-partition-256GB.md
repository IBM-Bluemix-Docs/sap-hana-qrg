---



copyright:
  years: 2017, 2018
lastupdated: "2017-02-21"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 3. Particionamento e sistemas de arquivos

Para o exemplo de três camadas, um servidor de 256 GB (servidor de banco de dados) com um disco lógico (em RAID10) foi
pedido e um servidor de 32 GB (servidor de aplicativos) com um disco lógico (em RAID 1). Ambos os servidores vêm com um grande
sistema de arquivos raiz que é igual ao tamanho total de disco (com algum espaço que é usado para `/boot`).

Para o servidor de 32 GB, siga as etapas de 1 a 10 em Preparando o seu servidor para a instalação do SAP (32 GB); em seguida,
continue com as etapas de 11 a 14, mas substituindo /`db2` por `/usr/sap/`. 
`sapmnt1` e `/usr/sap/trans` é criado e o Network File System (NFS) exportado do servidor
de banco de dados, que também mantém a instância do Serviço Advanced Business Application Programming (ABAP) SAP Central [(A)SCS].

O layout do sistema de arquivos a seguir é uma abordagem possível. Para uso de produção, você pode desejar seguir as
informações de dimensionamento para o seu sistema, pois outros layouts podem melhor atender às suas necessidades ou requisitos
de SAP ou você pode querer usar cotas.
Os diretórios necessários para instalar o software SAP, `/sapmnt`, `/usr/sap` e
`/db2`, precisam ser criados usando os seguintes comandos:
```
[root@ e2e2690 ~]# mkdir /sapmnt
[root@ e2e2690 ~]# mkdir /usr/sap
[root@ e2e2690 ~]# mkdir /db2
```
 
## Próximas etapas

[4. Preparando sua rede](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
