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

# 1. Pedindo servidores de 256 GB e de 32 GB para uma configuração de três camadas
{: #install_three_tier}

## Pedindo seus servidores
{: #order_servers}

Siga as etapas em
[Pedindo seu
servidor de 32 GB](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) para pedir o servidor de aplicativos SAP NetWeaver. As etapas a seguir o guiam pelo pedido do servidor
de banco de dados.

1. Efetue login no [{{site.data.keyword.cloud_notm}} portal do cliente de infraestrutura](https://control.softlayer.com) com suas credenciais exclusivas.
2. Clique no ícone **Dispositivos** na página Resumo da conta.
3. Clique em **Mensal** em **{{site.data.keyword.baremetal_long}}** na página
Dispositivos. A caixa de diálogo Lista de servidores aparece.
4. Clique no hiperlink em **INICIANDO PREÇO POR MÊS** para selecionar o servidor **BI.S1.NW256
(Opções de sistema operacional).**

## Selecionando suas opções do servidor
{: #options_32GB}

1. Deixe **1** no campo **Quantidade**.
2. Selecione **TOR01** para **Data Center**.
3. O **servidor** é padronizado para um valor predefinido, com base em sua seleção do servidor, e não pode ser mudado.
4. Clique em **32 GB RAM**, embora a seleção de **RAM** seja padronizada para um valor predefinido com base em sua seleção de servidor e não possa ser mudada.
5. Selecione **Red Hat Enterprise Linux for SAP Business Application 6.X** como o seu Sistema operacional.
6. Em **Discos rígidos,** selecione um segundo disco SATA de 2 TB, crie um grupo de
armazenamentos RAID de RAID1 dos dois discos que cubra a quantidade total de armazenamento e escolha **Linux
Básico** como o **Modelo de partição.** Deixe **LVM** desmarcado.
7. Selecione **500 GB** para **Largura da banda pública**.
8. Selecione **Uplinks redundantes de rede pública e privada de 1 Gbps** para **Velocidade da porta de uplink**.
9. Para este exemplo, deixe os valores padrão para todos os outros campos. É possível consultar
[Configurando seus
servidores bare metal](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers) para obter descrições detalhadas das opções.
10.	Clique em **Incluir no pedido** na parte inferior da página. Você é redirecionado à página de check-out após seu pedido ser verificado.

## Definindo as configurações do sistema avançado
{: #adv_config}

1. Use os valores na Tabela 1 para os campos em Configuração do sistema avançado. Mais informações estão disponíveis nas diretrizes de [Configuração do sistema avançado](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration).

|              Campo               |      Valor                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN de Backend                      | Selecione na lista suspensa, por exemplo, `tor01.bcr01a.1241`     |
|Sub-rede                            | Selecione na lista suspensa, por exemplo, `10.114.63.64/26`       |
|VLAN de frontend                     | Selecione na lista suspensa, por exemplo, `tor01.fcr01a.1199`     |
|Sub-rede                            | Selecione na lista suspensa, por exemplo, `169.55.137.160/27`     |
|Provisionar scripts                 | Padroniza para Nenhum                                                     |
|Chaves SSH                          | Padroniza para Nenhum                                                     |
|Nome do host                          | Por exemplo, `e2e2690`                                               |
|Domínio                            | Por exemplo, `saptest.com`                                           |
{: caption="Tabela 1. Valores de configuração avançada" caption-side="top"}  

## Confirmando suas seleções
{: #confirm_selections}

1. Confirme suas seleções na página de check-out e clique em **Termos de serviço de nuvem** e **Contrato de software de terceiro** no lado direito da página.
2. Clique em **Enviar pedido**. Você é redirecionado para uma tela com o seu número de pedido. É possível
imprimir a tela porque ela também é o seu recibo do pedido.

Após o envio do pedido, o servidor estará disponível para uso dentro de uma a quatro horas, dependendo de seu pedido. É
possível verificar a tela Detalhes do dispositivo na página principal do {{site.data.keyword.slportal}}
(**Dispositivos > Lista de dispositivos**) para obter um status das etapas de fornecimento. Clique no **Nome do dispositivo** que corresponde ao seu nome do host e domínio para ver o status.

## Próximas etapas

  [2. Preparando o servidor para a
sua instalação do SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)
  
  [3. Particionamento e sistemas de arquivos](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)
  
  [4. Preparando sua rede](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
