---



copyright:
  years: 2018
lastupdated: "2018-02-26"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Pedindo seu servidor de 32 GB
{: #install_32GB}

## Pedindo seu servidor
{: #order_32GB}

1. Efetue login no [{{site.data.keyword.cloud_notm}} portal do cliente de infraestrutura](https://control.softlayer.com) com suas credenciais exclusivas.
2. Clique em **Dispositivos** na página Resumo da conta.
3. Clique em **Mensal** em {{site.data.keyword.baremetal_short}} na página Dispositivos. A caixa de diálogo Lista de servidores aparece.
4. Clique no hiperlink em **INICIANDO PREÇO POR MÊS** para selecionar o servidor **BI.S1.NW32
(Opções de sistema operacional).**

## Selecionando suas opções do servidor
{: #options_32GB}

1. Deixe **1** no campo **Quantidade**.
2. Selecione **TOR01** para **Data Center**. A lista de data centers depende da
disponibilidade do produto em um data center específico.
3. O **servidor** é padronizado para um valor predefinido, com base em sua seleção do servidor, e não pode ser mudado.
4. Clique em **32 GB RAM**, embora a seleção de **RAM** seja padronizada para um valor predefinido com base em sua seleção de servidor e não possa ser mudada.
5. Selecione **Red Hat Enterprise Linux for SAP Business Application 6.X** como seu **Sistema
operacional.**
6. Em **Discos rígidos**, selecione um segundo disco SATA de 2 TB, crie um grupo de
armazenamento RAID de RAID1 dos dois discos que cubra a quantidade total de armazenamento e escolha **Linux
Básico** como o **Modelo de partição**. Deixe **LVM** desmarcado.
7. Selecione **500 GB** para **Largura da banda pública**.
8.	Selecione **Uplinks redundantes de rede pública e privada de 1 Gbps** para **Velocidade da porta de uplink**.
9. Deixe os valores padrão para todos os outros campos. Para obter descrições de opção detalhadas, consulte
[Configurando seus
servidores bare metal](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers).
10.	Clique em **Incluir no pedido** na parte inferior da página. Você é redirecionado à página de check-out após seu pedido ser verificado.

## Definindo as configurações do sistema avançado
{: #adv_config}

Use os valores na Tabela 1 para os campos em Configuração do sistema avançado. Mais informações estão disponíveis nas diretrizes de [Configuração do sistema avançado](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration).

1. Role para baixo e insira os valores na Tabela 1 em **Configuração do sistema avançado.**

|              Campo               |      Valor                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN de Backend                      | Selecione na lista suspensa, por exemplo, `tor01.bcr01a.1241`     |
|Sub-rede                            | Selecione na lista suspensa, por exemplo, `10.114.63.64/26`       |
|VLAN de frontend                     | Selecione na lista suspensa, por exemplo, `tor01.fcr01a.1199`     |
|Sub-rede                            | Selecione na lista suspensa, por exemplo, `169.55.137.160/27`     |
|Provisionar scripts                 | Padroniza para Nenhum                                                     |
|Chaves SSH                          | Incluir                                                                  |
|Nome do host                          | Por exemplo, `e2e1270`                                               |
|Domínio                            | Por exemplo, `saptest.com`                                           |
{: caption="Tabela 1. Valores de configuração avançada de 32 GB" caption-side="top"}  

## Confirmando suas seleções
{: #confirm_selections}

1. Confirme suas seleções na página de check-out e clique em **Termos de serviço de nuvem** e **Contrato de software de terceiro** no lado direito da página.
2. Clique em **Enviar pedido** no lado direito do formulário. Você é redirecionado para uma página com seu
número de pedido. É possível imprimir a página porque ela também é o seu recibo do pedido. Além disso, você receberá um e-mail de
confirmação com o assunto *Seu pedido ## do IBM Cloud foi aprovado*, em que ## é o seu número do pedido.

Depois do envio do pedido, seu servidor estará disponível para uso dentro de uma a quatro horas, dependendo de seu pedido. É
possível verificar a tela Detalhes do dispositivo na página principal do Portal do cliente (**Dispositivos >
Lista de dispositivos**) para um status das etapas de fornecimento. Clique no **Nome do dispositivo** que corresponde ao seu nome do host e domínio para ver o status.

## Próximas etapas
 
  [2. Preparando o servidor para
a sua instalação do SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)
  
  [3. Particionamento e sistemas de arquivos](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
