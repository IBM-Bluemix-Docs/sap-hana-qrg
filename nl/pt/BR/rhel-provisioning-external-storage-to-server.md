---

copyright:
years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, database server, deployment

subcollection: sap-netweaver-rhel-qrg

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
ao Block Storage](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started).


1. Efetue login no portal do cliente da infraestrutura do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){: new_window} com suas credenciais exclusivas.
2. Selecione **Armazenamento** > **Block Storage.**
3. Clique em **Pedir armazenamento de bloco** no canto superior direito da página Block Storage.
4. Selecione as especificidades para as suas necessidades de armazenamento. A Tabela 1 contém valores recomendados, incluindo
4 IOPS/GB para uma carga de trabalho de banco de dados típica.

|              Campo               |      Valor                                        |
| -------------------------------- | ------------------------------------------------- |
|Localização                          | TOR01                                             |
|Método de faturamento                    | Mensal (padrão)                                 |
|Novo tamanho de armazenamento                  | 1000 GB                                           |
|Opções de IOPS de armazenamento              | Endurance (IOPS em camada) (padrão)                 |
|IOPS em camada do Endurance             | 10 GB                                             |
|Tamanho do espaço de captura instantânea               | 0 GB                                              |
|Tipo de S.O.                           | Padronizado para Linux                                 |
{: caption="Tabela 1. Valores recomendados para armazenamento de bloco" caption-side="top"}

5. Clique nas duas caixas de seleção e em **Fazer pedido**.

## Autorizando hosts
{: authorize-host}

1. Clique em **Ações** à direita de seu LUN e selecione **Autorizar host** para acessar
o armazenamento provisionado.
2. Selecione **Dispositivos**; o **Tipo de dispositivo** é padronizado para Bare Metal Server. Clique em **Hardware** e selecione os nomes de host de seus dispositivos.
3. Clique no botão **Enviar**.
4. Verifique o status de seu armazenamento provisionado em **Dispositivos** > (selecione seu dispositivo) > guia **Armazenamento**.
5. Observe o **Endereço de destino** e o Nome qualificado de iSCSI (**IQN**) para o servidor (inicializador iSCSI), além do **nome do usuário** e a **senha** para autorização com o servidor iSCSI. Você precisa dessas informações nas etapas a seguir.
6. Siga as etapas em [Conectando-se às LUNs do iSCSI no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#connecting-to-mpio-iscsi-luns-on-linux) para tornar seu armazenamento acessível de seu servidor provisionado.

## Fazendo caminhos múltiplos de armazenamento
{: #multipath}

Na implementação de amostra, você recuperou os seguintes dados da guia **Armazenamento**:
  * IP de destino: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * Usuário: `SL01SU276540-H896345`
  * Senha: `EtJ79F4RA33dXm2q`

1. Insira o seguinte com base nas informações recuperadas:
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   Uma entrada existente pode ter que ser substituída em `/etc/iscsi/initiatorname.iscsi`.

2. Inclua as seguintes linhas na parte inferior de `/etc/iscsi/iscsid.conf`:
```
[root@sdb192 ~]# tail /etc/iscsi/iscsid.conf
# it continue to respond to R2Ts. To enable this, uncomment this line
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. Substitua os valores `username` e `password` na etapa 2 por aqueles observados durante a etapa 5 de *Autorizando hosts*.

4. Descubra o destino iSCSI inserindo as seguintes linhas.
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Configure o host para efetuar login automaticamente na matriz iSCSI.

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. Instale e inicie o daemon de caminhos múltiplos.
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. Conclua todos os comandos em [Conectando-se às LUNS do iSCSI no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux) para que outra LUN apareça na saída de caminhos múltiplos.
```
[root@sdb192 ~]# multipath -ll
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

Pegue a amostra `/etc/multipath.conf` do [ `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) de exemplo e crie-o em seu servidor. Qualquer caractere especial copiado, retorno de linha semelhante a DOS e entrada de feed de linha pode levar a um comportamento inesperado. Certifique-se de que você tenha um arquivo Unix ASCII depois
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

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. Verifique os sistemas de arquivos em ambos os servidores. Sua saída deve ser semelhante à seguinte saída.

        [root@e2e1270 ~]# df -h
        Filesystem		    Size  Used Avail Use% Mounted on
        /dev/sda3             879G  1,5G  833G   1% /
        tmpfs                  16G     0   16G   0% /dev/shm
        /dev/sda1             248M   63M  173M  27% /boot
        /dev/sdb2             849G  201M  805G   1% /usr/sap
        db192-priv:/usr/sap/trans
                      165G   59M  157G   1% /usr/sap/trans
                      db192-priv:/sapmnt/C10
                      165G   59M  157G   1% /sapmnt/C10

        [root@sdb192 ~]# df -h
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
`LOGMETH1` em seu banco de dados {{site.data.keyword.Db2_on_Cloud_short}}. Consulte a [Documentação do {{site.data.keyword.Db2_on_Cloud_short}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: new_window} para obter detalhes.
