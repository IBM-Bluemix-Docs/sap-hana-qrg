---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, ABAP, ASCS instance, ABAP SAP Central Services, application server, database server, three-tier

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 4. Preparando sua rede para uma configuração de três camadas
{: #network}

Se você estiver planejando instalar uma configuração de três camadas, a rede precisará ser configurada corretamente. No exemplo, um servidor de banco de dados de 192 GB (denominado `sdb192`) e um servidor de aplicativos de 32 GB (denominado `e2e1270`) foram implementados. O servidor de banco de dados também hospeda a instância (A)SCS. Incluir os endereços IP na rede privada para o seu `/etc/hosts` ajuda com as próximas etapas e assegura que o
tráfego de rede interno do SAP atravesse a rede certa.

![Figura 1. Amostra de configuração de três camadas](/images/network-01.png "Amostra de configuração de três camadas")

Use as etapas a seguir para estabelecer a sua rede.

1. Efetue login nos servidores e localize a sua configuração de rede privada.

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

No exemplo de três camadas, 10.17.139.35 é o endereço IP privado do servidor de banco de dados; é de um dos
intervalos de endereço IP de RFC 1597. Também é possível determinar o endereço IP do servidor de aplicativos e incluir
os dois endereços IP no `/etc/hosts` dos dois servidores.

        [root@sdb192 ~]# cat /etc/hosts
        127.0.0.1 localhost.localdomain localhost
        208.43.211.118 e2e2690.saptest.com e2e2690

        10.17.139.35    sdb192-priv
        10.17.139.46    e2e1270-priv

Inclua também as duas últimas linhas em `e2e1270`.

## Instalando o software NFS

1. Instale o software NFS `nfs-utils` **nos dois servidores**.

      `[root@sdb192 ~]# yum install nfs-utils`

Certifique-se de iniciar e registrar o `rpcbind` e o serviço NFS no servidor de banco de dados.

        [root@sdb192 ~]# service rpcbind start
        [root@sdb192 ~]# chkconfig nfs on
        [root@sdb192 ~]# service nfs start

## Usando o NFS para exportar

1. Use o NFS para exportar `/sapmnt` e `/usr/sap/trans` do servidor de banco de dados para o
servidor de aplicativos, incluindo a entrada necessária para `/etc/exports` do servidor de banco de dados.

        /sapmnt/C10		10.17.139.46(rw,no_root_squash,sync,no_subtree_check)
        /usr/sap/trans	10.17.139.46(rw,no_root_squash,sync,no_subtree_check)

A valor de amostra `C10` precisa ser substituído pelo ID do sistema SAP para seu sistema SAP. Deve-se criar
o diretório antes de exportá-lo. Execute os seguintes comandos na linha de comandos do servidor de banco de dados:

        [root@sdb192 ~]# mkdir /sapmnt/C10
        [root@sdb192 ~]# mkdir -p /usr/sap/trans
        [root@sdb192 ~]# exportfs -a

## Montando o compartilhamento de NFS

1. Monte o compartilhamento de NFS no servidor de aplicativos incluindo a entrada a seguir em seu
`/etc/fstab` e monte-o por meio da linha de comandos.

        sdb192-priv:/sapmnt/C10 /sapmnt/C10 nfs defaults 0 0
        sdb192-priv:/usr/sap/trans /usr/sap/trans nfs defaults 0 0

2. Crie os diretórios de destino no servidor de aplicativos e monte-os.

        [root@e2e1270 ~]# mkdir -p /sapmnt/C10
        [root@e2e1270 ~]# mkdir /usr/sap/trans
        [root@e2e1270 ~]# mount /sapmnt/C10
        [root@e2e1270 ~]# mount /usr/sap/trans

Os seus servidores agora estão preparados para hospedar os componentes de uma instalação do SAP distribuída.

## Próximas etapas

  * [Incluindo armazenamento externo em seu {{site.data.keyword.baremetal_short}}](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-storage)
  * [Instalando sua paisagem do SAP (aplicativos e software)](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_landscape)
