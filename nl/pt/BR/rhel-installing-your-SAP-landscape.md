---



copyright:
  years: 2017, 2018
lastupdated: "2018-08-14"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Instalando a paisagem de seu SAP
{: #install_landscape}

## Instalando pacotes RPM (pré-requisito)
{: #RPM}

Uma instalação do SAP requer determinados pré-requisitos para os pacotes que estão instalados no sistema operacional e nos daemons do sistema operacional em execução. Consulte os [guias de instalação](https://support.sap.com/software/installations.html)
mais recentes (requer um [ID do usuário S do
SAP](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started)) e [notas de suporte](https://support.sap.com/notes) (requer um ID do usuário S do
SAP) do SAP para uma lista atualizada desses pré-requisitos. Há mais dois pacotes que precisam ser instalados:
* compat-sap-c++: geralmente, atinge a compatibilidade do tempo de execução C++ com os compiladores usados pelo SAP. Como o Red Hat Enterprise Linux for SAP Business Application 7.X foi selecionado como o S.O. para o servidor de aplicativos de 32 GB e o servidor de banco de dados de 192 GB, você usará `compat-sap-c++-7`.
* uuidd: mantém o suporte de sistema operacional para a criação de UUIDs.

### Verificando se o uuidd está instalado

1. Verifique se `uuid daemon (uuidd)` está instalado. Se não estiver, instale-o e inicie-o.
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### Instalando o pacote compat-sap-c++-7

1. Siga a [Nota SAP 2195019](https://launchpad.support.sap.com/#/notes/2195019), instale o pacote compat-sap-c++-7 e crie um soft link específico, necessário para os binários do SAP. Verifique as Notas do SAP específicas da liberação do produto que você está instalando para determinar se a biblioteca é necessária ou não.
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## Fazendo download do software SAP
{: download_software}

Efetue login no [Portal de Suporte do SAP](https://support.sap.com/en/index.html), clique em **Fazer download do software** e faça download dos DVDs necessários para uma unidade de compartilhamento local. Transfira os arquivos para o seu servidor provisionado. Outra opção é fazer download do
[SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677),
instalá-lo em seu servidor de destino e diretamente fazer o download das imagens do DVD para o servidor.

## Preparando para a GUI SWPM do SAP
{: #prepare_for_GUI}

Dependendo da largura de banda e da latência de sua rede, você pode desejar executar a interface gráfica com o usuário (GUI)
do SAP Software Provisioning Manager (SWPM) remotamente em uma sessão de computação de rede virtual (VNC). Outra opção é ter a GUI
executando localmente e conectar-se ao SWPM na máquina de destino. Consulte a
[documentação do SWPM](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0) se você decidir
executar a GUI localmente.

As etapas a seguir descrevem a execução da GUI do SWPM remotamente em uma sessão de VNC. Essa opção instala um servidor VNC, que pode não ser sequencial com o reforço de seu sistema operacional; verifique se você atende às medidas de segurança. Consulte a
[documentação do VNC](http://searchnetworking.techtarget.com/definition/virtual-network-computing) para obter uma
visão geral sobre suas funções, se você não estiver familiarizado com ele.

1. Use os seguintes comandos para instalar um servidor VNC.
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. Use o comando a seguir para instalar o gerenciador de janela X11, `twm`, que está incluído na
distribuição Linux.

`[root@sdb192 ~]# yum install twm`

3. Instale um emulador de terminal, por exemplo, `xterm`.

 `[root@sdb192 ~]# yum install xterm`

4. Inicie o servidor VNC da linha de comandos.

 `[root@sdb192 ~]# vncserver`

Agora, você requer um programa cliente VNC; há múltiplas implementações disponíveis para todos os sistemas operacionais
sem nenhum custo. Geralmente, é necessário que a porta `590X` (em que X é o número de servidores em
execução, iniciando em 1) esteja acessível de seu cliente.

Você pode ter que iniciar um `xterm` por meio do menu de plano de fundo de `twm`. É
possível iniciar o `SWPM` (`sapinst`) do `xterm`.

## Instalando o software SAP
{: #install_software}

Depois de fazer download da mídia de instalação, siga o procedimento de instalação SAP padrão que está documentado no guia de
instalação do SAP para a sua versão e componentes do SAP e as Notas sobre o SAP correspondentes.

É possível iniciar o SWPM do SAP do `xterm` e executar as etapas de instalação.

### Instalando o software SAP em um ambiente de três camadas

Siga as etapas no SWPM do SAP para uma configuração de três camadas.

1. Selecione **Sistema distribuído** e execute a instalação do ASCS e a instalação do banco
de dados no servidor de banco de dados.
2. Instale o Application Server ABAP no servidor de aplicativos. Certifique-se de usar os endereços privados para o ASCS e os
nomes de host do banco de dados durante a instalação do servidor de aplicativos.

O uso dos endereços privados e dos nomes de host garante que o tráfego de rede entre o servidor de aplicativos e o ASCS ou
o banco de dados, passe pela rede privada e não pela rede pública.
