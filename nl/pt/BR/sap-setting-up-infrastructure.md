---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, {{site.data.keyword.cloud_notm}}, deployment, BYOL

subcollection: sap-netweaver

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 2. Configurando a sua infraestrutura
{: #set_up_infrastructure}

A orientação para configurar os {{site.data.keyword.baremetal_long}}, a rede, a segurança, o armazenamento e o sistema operacional (S.O.) do SAP NetWeaver estão na seção a seguir. Entre em contato com o [Suporte do {{site.data.keyword.cloud_notm}}](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) se tiver alguma dúvida.

## Fazendo pedido de seu servidor
{: order-server}

Use as etapas a seguir para pedir seus {{site.data.keyword.baremetal_short}}. Informações adicionais podem ser localizadas em [Construindo um servidor bare metal customizado](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server).

1. Efetue login no portal do cliente da infraestrutura do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com){: new_window} usando suas credenciais exclusivas.
2. Clique em **Conta** > **Fazer um pedido** na página Resumo da conta.
3. Clique no link **Mensal** em {{site.data.keyword.baremetal_short}} na página Dispositivos. A caixa de diálogo Lista de servidores é exibida; os Servidores certificados por SAP estão na parte superior da lista.
4. Clique no hiperlink em **PREÇO INICIAL POR MÊS** para selecionar o servidor apropriado e ser levado à página Configurar/Pedir.

O servidor BI.S3.NW32 (Opções de S.O.) também está disponível para faturamento **Por hora**.
{: note}

   Os servidores certificados pelo SAP NetWeaver são identificados com **-NW** sob Modelo de CPU. A configuração do servidor baseada no Red Hat é descrita na etapa 7 e na etapa 1 em [Selecionando suas opções do servidor](#select_options); as etapas são as mesmas para o Microsoft Windows. Observe que para o VMWare, sua escolha é limitada, no entanto, as etapas são as mesmas.

   Este é um exemplo sobre como decifrar os nomes do servidor SAP NetWeaver.

| Nome do servidor | Componente de convenção de nomenclatura | O que significa |
| --- | --- | --- |
| BI.S3.NW768 | BI | Interface do Bluemix |
| | S3 | Série 2 (geração do processador) |
| | | S1 é Ivy Bridge/Haswell |
| | | S2 é Broadwell |
| | | S3 é Skylake/Kaby Lake |
| | NW | Servidor certificado por NetWeaver |
| | 768 | Quantidade de RAM |

5. Insira o número de servidores que você está pedindo no campo **Qualidade** e selecione seu **Data center**.
6. **Servidor**, **RAM** e sua opção de armazenamento privado são padronizados com base na seleção do servidor e não podem ser mudados. O {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} for {{site.data.keyword.cloud_notm}} ou {{site.data.keyword.filestorage_full_notm}} é solicitado depois de você ter pedido seu servidor.
7. Selecione seu **Sistema operacional** da Microsoft, Red Hat ou SUSE e selecione o sistema operacional específico ou hypervisor VMware para seu servidor.

Se você estiver trazendo sua própria licença (BYOL) para o sistema operacional, selecione **Outro** > **Nenhum sistema operacional**. Para obter mais informações, consulte [Traga sua própria licença](#byol).
{: note}

## Selecionando suas opções do servidor
{: #select_options}

Na próxima etapa, você selecionará o tipo e o número de discos que deseja incluir na configuração. Também será possível selecionar opções diferentes para grupos de armazenamentos Redundant Array of Independent Disks (RAID) e layouts de particionamento em cima dos grupos de armazenamentos RAID. Consulte [Sobre o RAID](/docs/bare-metal?topic=bare-metal-about-raid#about-raid) ou [Suporte do {{site.data.keyword.cloud_notm}} ](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) para obter mais informações.

1. Em **Servidores certificados SAP**, faça sua seleção com base em como você planeja usar seu servidor. Consulte a [Design Decision Tool ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool){: new_window} (role para abaixo até a ferramenta) ou o [Suporte do {{site.data.keyword.cloud_notm}} ](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) para obter mais informações
2. Clique no botão **Incluir no pedido** na parte inferior da página.

## Definindo as configurações do sistema avançado
{: #adv_config}

1. Siga as diretrizes de [Configuração do sistema avançado](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#ordering-baremetal-server) para obter ajuda com os valores na janela **Configuração do sistema avançado**.

Os nomes de host da SAP devem consistir em, no máximo, 13 caracteres alfanuméricos. Consulte as [Notas SAP 611361 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://launchpad.support.sap.com/#/611361){: new_window} e [129997 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://launchpad.support.sap.com/#/129997){: new_window} para obter mais detalhes do Nome de host do SAP.

Se você tiver decidido sobre uma configuração pública/privada para seu ambiente e planeja
  * Instalar múltiplos sistemas SAP que precisam se comunicar entre si *ou*
  * Escolher uma configuração SAP de três camadas (instâncias de banco de dados e aplicativo em hardware diferente)

Certifique-se de que sua resolução de nome reflita os endereços internos e externos. O endereço externo corresponde ao nome do host do servidor resolvido por seu nome de domínio completo (FQDN).

Os endereços internos não aparecerão no sistema de nome de domínio (DNS). Como os IPs internos devem ser usados para comunicação entre os servidores, certifique-se de ampliar seu arquivo `/etc/hosts` ou "host" do Microsoft Windows adequadamente. Essas informações também podem se aplicar ao sistema operacional guest das implementações baseadas no VMware ESXi.

## Confirmando suas seleções
{: #confirm_selections}

1. Confirme suas seleções na página de Check-out e clique nas caixas de seleção **Termos do serviço de nuvem** e **Contrato de software de terceiros**.
2. Role para baixo para Criar conta: entre em Faturamento e insira o **Tipo de pagamento, o nome, o cartão** e a **Expiração** (data) do cartão de crédito que será usado para faturamento.
3. Clique no botão **Enviar pedido**. Você é redirecionado para uma tela com o seu número de pedido. É possível
imprimir a tela porque ela também é o seu recibo do pedido.

Um e-mail de confirmação com o assunto Seu pedido nº do _{{site.data.keyword.cloud_notm}} foi aprovado_ é enviado para o endereço de e-mail em seu perfil. Esse e-mail é um aviso de que o servidor foi aprovado e está no processo de implementação. Depois que ele for implementado, outro aviso será enviado notificando-o de que o servidor está disponível e pode ser gerenciado por meio do portal do cliente da infraestrutura do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com){: new_window}.

## Traga sua própria licença
{: #byol}

Quando você tiver sua própria licença do sistema operacional, instale-a em seu {site.data.keyword.baremetal_short} com base nas instruções do fornecedor. Para obter mais informações, consulte [A opção sem S.O.](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Próximas etapas

Agora você está pronto para começar a gerenciar seus {{site.data.keyword.baremetal_short}}. Consulte [Gerenciando seu ambiente do SAP NetWeaver](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-manage_environment#manage_environment) para as próximas etapas.
