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

# 1. Pedindo servidores de 192 GB e de 32 GB para uma configuração de três camadas
{: #install_three_tier}

## Pedindo seus servidores
{: #order_servers}

Siga as etapas em
[Pedindo seu
servidor de 32 GB](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) para pedir o servidor de aplicativos SAP NetWeaver. As etapas a seguir o guiam pelo pedido do servidor
de banco de dados.

## Pedindo seu servidor de banco de dados
{: #order-db-server}

1. Efetue login no [{{site.data.keyword.cloud_notm}} portal do cliente de infraestrutura](https://control.softlayer.com) com suas credenciais exclusivas.
2. Clique em **Conta** > **Fazer um pedido** na página Resumo da conta.
3. Clique em **Mensal** em **{{site.data.keyword.baremetal_long}}** na página
Dispositivos. A Lista de servidores aparece; os Servidores certificados por SAP estão na parte superior da lista.
4. Clique no hiperlink em **INICIANDO PREÇO POR MÊS** para selecionar o servidor **BI.S3.NW192 (Opções do S.O.).**

## Configurando seu servidor de banco de dados
{: #options_192GB}

1. Deixe **1** no campo **Quantidade**.
2. Selecione **TOR01** para **Data Center**.
3. O **servidor** é padronizado para um valor predefinido, com base em sua seleção do servidor, e não pode ser mudado.
4. Clique em **192 GB de RAM**, mesmo que a seleção de **RAM** esteja padronizada como um valor predefinido com base em sua seleção do servidor e não puder ser mudada.
5. Clique em **Redhat** e selecione **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bits)** como seu **Sistema operacional**. **Nota**: se você estiver trazendo sua própria licença (BYOL) para o sistema operacional, selecione **Outro** > **Nenhum sistema operacional**. Para obter mais informações, consulte [Traga sua própria licença](#byol).
6. Inclua uma segunda unidade SATA de 2 TB clicando no menu suspenso **Controlador de disco 1** e selecionando **SATA de 2 TB**. Clique em **Incluir disco**.
7. Clique em **Selecionar todos os discos** e em **Criar grupo de armazenamento RAID**.
8. Clique em **Tipo** e selecione **RAID 1**. Insira um **Tamanho** que cubra a quantia total de armazenamento necessário.
9. Deixe **LVM** desmarcado e aceite o padrão **Modelo de partição**, **Linux básico**.
10. Clique em **Concluído**.

## Selecionando suas opções adicionais do servidor de banco de dados
{: #addl-server-options}

1. Selecione **500 GB** para **Largura da banda pública**.
2. Selecione **Uplinks redundantes de rede pública e privada de 1 Gbps** para **Velocidade da porta de uplink**.
3. Para este exemplo, deixe os valores padrão para todos os outros campos. É possível consultar [Construindo um servidor bare metal customizado](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options) para obter descrições detalhadas das opções.
4.	Clique em **Incluir no pedido** na parte inferior da página. Você é redirecionado à página de check-out após seu pedido ser verificado.

## Definindo as configurações do sistema avançado
{: #adv_config}

1. Use os valores na Tabela 1 para os campos em Configuração do sistema avançado. Mais informações estão disponíveis nas diretrizes de [Configuração do sistema avançado](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config).

|              Campo               |      Valor                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN de Backend                      | Selecione na lista suspensa, por exemplo, `tor01.bcr01a.1241`     |
|Sub-rede                            | Selecione na lista suspensa, por exemplo, `10.114.63.64/26`       |
|VLAN de frontend                     | Selecione na lista suspensa, por exemplo, `tor01.fcr01a.851`      |
|Sub-rede                            | Selecione na lista suspensa, por exemplo, `158.85.65.224/28`      |
|Provisionar scripts                 | Deixar em branco                                                          |
|Chaves SSH                          | Padronizado como `Add`, o que significa nenhuma chave SSH                            |
|Nome do host                          | Por exemplo, `sdb192`                                                |
|Domínio                            | Por exemplo, `saptest.com`                                           |
{: caption="Tabela 1. Valores de configuração avançada de 192 GB" caption-side="top"}  

## Confirmando suas seleções
{: #confirm_selections}

1. Confirme suas seleções na página de check-out e clique em **Termos de serviço de nuvem** e **Contrato de software de terceiro** no lado direito da página.
2. Clique em **Enviar pedido**. Você é redirecionado para uma tela com o seu número de pedido. É possível
imprimir a tela porque ela também é o seu recibo do pedido.

Após o envio do pedido, o servidor estará disponível para uso dentro de uma a quatro horas, dependendo de seu pedido. É
possível verificar a tela Detalhes do dispositivo na página principal do {{site.data.keyword.slportal}}
(**Dispositivos > Lista de dispositivos**) para obter um status das etapas de fornecimento. Clique no **Nome do dispositivo** que corresponde ao seu nome do host e domínio para ver o status.

## Traga sua própria licença
{: #byol}

Quando você tiver sua própria licença do sistema operacional, instale-a no {{site.data.keyword.baremetal_short}} com base nas instruções do fornecedor. Para obter mais informações, consulte [A opção Nenhum S.O.](https://console.bluemix.net/docs/bare-metal/introduction-no-os.html#how-to-install-an-operating-system-on-a-no-os-server-).

## Próximas etapas

  [2. Preparando o servidor para a
sua instalação do SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)

  [3. Particionamento e sistemas de arquivos](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)

  [4. Preparando sua rede](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
