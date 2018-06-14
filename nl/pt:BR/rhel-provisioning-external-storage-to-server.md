---



copyright:
  years: 2017, 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Incluindo armazenamento externo ao seu servidor
{: #storage}

## Configurando o armazenamento externo
{: #set_up_storage}

Armazenamento externo pode ser incluído em seu servidor ou servidores provisionados se você deseja usá-lo como um
dispositivo de backup ou usar uma captura instantânea para restaurar rapidamente o seu banco de dados em um ambiente de teste. Para
o exemplo de três camadas, o armazenamento de bloco é usado para arquivar arquivos de log do banco de dados e
backups on-line e off-line para o banco de dados. O armazenamento de bloco mais rápido (4 IOPS por GB) foi selecionado para ajudar a
garantir um tempo mínimo de backup. Armazenamento de bloco mais lento pode ser suficiente para as suas necessidades. Para obter mais
informações sobre o {{site.data.keyword.blockstoragefull}}, consulte
[Introdução
ao armazenamento de bloco](https://console.bluemix.net/docs/infrastructure/BlockStorage/index.html#getting-started-with-block-storage).


1. Efetue login no [{{site.data.keyword.cloud_notm}} portal do cliente de infraestrutura](https://control.softlayer.com/) com suas credenciais exclusivas.
2. Selecione **Armazenamento > Armazenamento de bloco.**
3. Clique em **Pedir armazenamento de bloco** no canto superior direito da página Armazenamento de bloco.
4. Selecione as especificidades para as suas necessidades de armazenamento. A Tabela 1 contém valores recomendados, incluindo
4 IOPS/GB para uma carga de trabalho de banco de dados típica.

|              Campo               |      Valor                                        |
| -------------------------------- | ------------------------------------------------- |
|Selecionar tipo de armazenamento               | Resistência (padrão)                               |
|Localização                          | TOR01                                             |
|Método de faturamento                    | Mensal (padrão)                                 |
|Selecionar pacote de armazenamento      | 4 IOPS/GB                                         |
|Selecionar tamanho de armazenamento               | 1000 GB                                           |
|Especificar tamanho do espaço de captura instantânea       | 0 GB                                              |
|Selecionar tipo de sistema operacional                    | Linux (padrão)                                   |
{: caption="Tabela 1. Valores recomendados para armazenamento de bloco" caption-side="top"}

5. Clique em **Continuar**.
6. Selecione **Eu li o Contrato de serviço principal** e clique em **Fazer
pedido**.
7. Clique em **Ações** à direita de seu LUN e selecione **Autorizar host** para acessar
o armazenamento provisionado.
8. Selecione **Dispositivos**; o **Tipo de dispositivo** é padronizado para Servidor
bare metal. Clique em **Hardware** e selecione os nomes de host de seus dispositivos.
9. Clique no botão **Enviar**.
10. Verifique o status de seu armazenamento provisionado em **Dispositivos** > (selecione seu
dispositivo) > **Guia Armazenamento.**
11. Observe o **Endereço de destino** e Nome qualificado de iSCSI (**IQN**) para o seu
servidor (inicializador iSCSI) e o **nome do usuário** e **senha** para autorização
com o servidor iSCSI. Você precisa dessas informações nas etapas a seguir.
12. Siga as etapas em
[Conectando
aos LUNs iSCSI MPIO no Linux](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#connecting-to-mpio-iscsi-luns-on-linux) para tornar seu armazenamento acessível de seu servidor provisionado.

## Fazendo caminhos múltiplos de armazenamento
{: #multipath}

Na implementação de amostra, você recuperou os seguintes dados da guia **Armazenamento**:
  * IP de destino: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * Usuário: `SL01SU276540-H896345`
  * Senha: `EtJ79F4RA33dXm2q`

1. Insira o seguinte com base nas informações recuperadas:
```
[root@e2e2690 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
``` 
   Uma entrada existente pode ter que ser substituída em `/etc/iscsi/initiatorname.iscsi`.

2. Inclua as seguintes linhas na parte inferior de `/etc/iscsi/iscsid.conf`:
```
[root@e2e2690 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. Substitua os valores `username` e `password` na etapa 2 por aqueles que você
anotou durante a etapa 11 de Configurando o armazenamento externo.

4. Descubra o destino iSCSI inserindo as seguintes linhas.
```
[root@e2e2690 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Configure o host para efetuar login automaticamente na matriz iSCSI.

      `[root@e2e2690 ~]# iscsiadm -m node -L automatic`

6. Instale e inicie o daemon de caminhos múltiplos.
```
[root@e2e2690 ~]# yum install device-mapper-multipath
…
[root@e2e2690 ~]# chkconfig multipathd on
[root@e2e2690 ~]# service multipathd start
```

7. Conclua todos os comandos no
[Montando
os volumes de armazenamento de bloco no Linux](https://console.bluemix.net/docs/infrastructure/BlockStorage/accessing_block_storage_linux.html#mounting-block-storage-volumes) para que outro LUN apareça na saída de caminhos múltiplos.
```
[root@e2e2690 ~]# multipath -ll
…
3600a098038303452543f464142755a42 dm-9 NETAPP,LUN C-Mode
size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
|-+- policy='round-robin 0' prio=50 status=active
| `- 10:0:0:169 sde 8:64 active ready running
`-+- policy='round-robin 0' prio=10 status=enabled
`- 9:0:0:169  sdf 8:80 active ready running
…`
```

Agora é possível usar o dispositivo de caminhos múltiplos como você usaria qualquer dispositivo de disco. Um caminho de
dispositivo aparece em `/dev/mapper/3600a098038303452543f464142755a42`.

Pegue a amostra `/etc/multipath.conf` do
[ `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-sample.html#sample) de
exemplo e crie-a em seu servidor. Qualquer caractere especial copiado, retorno de linha semelhante a DOS e entrada de feed de linha pode levar a um comportamento inesperado. Certifique-se de que você tenha um arquivo Unix ASCII depois
de copiar os conteúdos.

Adapte o bloco de caminhos múltiplos de `/etc/multipath.conf` para criar um alias do caminho para acessar o
dispositivo em `1/dev/mapper/mpath1`.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. Reinicie o `multipathd`. Agora é possível criar o `/backup filesystem` e montar o
dispositivo de bloco.
        
      [root@e2e2690 ~]# service multipathd restart
      [root@e2e2690 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@e2e2690 ~]# mkdir  /backup

9. Verifique os sistemas de arquivos em ambos os servidores. Sua saída deve ser semelhante à seguinte saída.

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db2690-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db2690-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@e2e2690 ~]# df -h
        Filesystem      	    Size  Used Avail Use% Mounted on
        /dev/sda3             549G  2,3G  519G   1% /
        tmpfs                 127G     0  127G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/mapper/mpath1    493G   70M  468G   1% /backup
        /dev/mapper/datavg-datalv
                      1,2T   71M  1,1T   1% /db2
        /dev/mapper/datavg-saplv
                      165G   60M  157G   1% /usr/sap
        /dev/mapper/datavg-sapmntlv
                      165G   60M  157G   1% /sapmnt

Se você instalar um aplicativo SAP baseado em SAP NetWeaver no {{site.data.keyword.Db2_on_Cloud_long}}, deve-se criar
subdiretórios em `/backup` pertencentes ao usuário administrativo do banco de dados (`db2SID`)
para backups completos e arquivos de log arquivados. Para arquivamento automático dos arquivos de log, é necessário configurar
`LOGMETH1` em seu banco de dados {{site.data.keyword.Db2_on_Cloud_short}}. Consulte a
[
documentação do {{site.data.keyword.Db2_on_Cloud_short}}](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html) para detalhes.
