---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN, application server, database server, three-tier, SAP certified servers

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Realización del pedido de servidores de 192 GB y 32 GB para una configuración de tres capas
{: #install_three_tier}

## Realización del pedido de servidores
{: #order_servers}

Siga los pasos descritos en [Realización del pedido del servidor de 32 GB](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_32GB#order_32GB) para realizar el pedido de un servidor de aplicaciones SAP NetWeaver. Los siguientes pasos le guiarán para realizar el pedido del servidor de base de datos.

## Realización del pedido de servidor de base de datos
{: #order-db-server}

1. Inicie sesión en el [portal de clientes de la infraestructura de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com){: new_window} con sus credenciales exclusivas.
2. Pulse **Cuenta** > **Realizar un pedido** en la página Resumen de la cuenta.
3. Pulse **Mensual** en **{{site.data.keyword.baremetal_long}}** en la página Dispositivos. Aparecerá la Lista de servidores; los servidores certificados para SAP están en la parte superior de la lista.
4. Pulse el hiperenlace en **Precio inicial al mes** para seleccionar el servidor **BI.S3.NW192 (Opciones de sistema operativo).**

El servidor BI.S3.NW32 (Opciones de sistema operativo) también está disponible para la facturación **Por hora**.
{: note}

## Configuración del servidor de base de datos
{: #options_192GB}

1. Deje **1** en el campo **Cantidad**.
2. Seleccione **TOR01** para **Centro de datos.**
3. **Servidor** se establece de forma predeterminada en un valor predefinido sobre la base de la selección de servidor y no se puede cambiar.
4. Pulse **192 GB RAM** aunque **RAM** se establece de forma predeterminada en un valor predefinido sobre la base de la selección de servidor y no se puede cambiar.
5. Pulse **Redhat** y seleccione **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bits)** como **Sistema operativo**. **Nota**: Si trae su propia licencia (BYOL) para el sistema operativo, seleccione **Otro** > **Sin sistema operativo**. Para obtener más información, consulte [Traiga su propia licencia](#byol).
6. Añada una segunda unidad SATA de 2 TB pulsando el menú desplegable **Controlador de disco 1** y seleccionando **SATA de 2 TB**. Pulse **Añadir disco**.
7. Pulse **Seleccionar todos los discos** y pulse **Crear depósito de almacenamiento RAID**.
8. Pulse **Tipo** y seleccione **RAID 1**. Indique un **Tamaño** que cubra la cantidad total de almacenamiento que necesite.
9. Deje **LVM** sin seleccionar y acepte la **Plantilla de partición** predeterminada, **Linux Basic**.
10. Pulse **Listo**.

## Selección de las opciones adicionales de servidor de base de datos
{: #addl-server-options}

1. Seleccione **500 GB** para el **Ancho de banda público.**
2. Seleccione **1 Gbps de enlaces ascendentes de red pública y privada redundantes** para **Velocidad de puerto de enlace ascendente.**
3. Para este ejemplo, deje los valores predeterminados para todos los demás campos. Puede consultar [Creación de un servidor nativo personalizado](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options) para obtener descripciones detalladas de las opciones.
4.	Pulse **Añadir a pedido** en la parte inferior de la página. Se le redirigirá a la página de pago una vez verificado su pedido.

## Ajuste de las configuraciones avanzadas del sistema
{: #adv_config}

1. Utilice los valores de la Tabla 1 para los campos de la Configuración avanzada del sistema. Encontrará más información disponible en las directrices de [Configuración avanzada del sistema](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config).

|              Campo               |      Valor                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN de fondo                      | Seleccione de la lista desplegable, por ejemplo `tor01.bcr01a.1241`     |
|Subred                            | Seleccione de la lista desplegable, por ejemplo `10.114.63.64/26`       |
|VLAN frontal                     | Seleccione de la lista desplegable, por ejemplo `tor01.fcr01a.851`      |
|Subred                            | Seleccione de la lista desplegable, por ejemplo `158.85.65.224/28`      |
|Scripts de suministro                 | Déjelo en blanco                                                          |
|Claves SSH                          | El valor predeterminado es `Añadir`, lo que significa que no hay clave SSH                            |
|Nombre de host                          | Por ejemplo, `sdb192`                                                |
|Dominio                            | Por ejemplo, `saptest.com`                                           |
{: caption="Tabla 1. Valores de la configuración avanzada de 192 GB" caption-side="top"}  

## Confirmación de las selecciones
{: #confirm_selections}

1. Confirme las selecciones en la página de pago, y pulse **Términos de los servicios en la nube** y **Acuerdo de software de terceros** en la parte derecha de la página.
2. Pulse **Enviar pedido**. Se le redirigirá a una página con el número de pedido. Puede imprimir la página, ya que también es su recibo del pedido.

Una vez enviado el pedido, el servidor estará disponible, dependiendo de su pedido, para su uso en un plazo de una a cuatro horas. Puede comprobar el estado de los distintos pasos del suministro en la pantalla Detalles del dispositivo en la página principal del {{site.data.keyword.slportal}} (**Dispositivos > Lista de dispositivos**). Pulse el **Nombre de dispositivo** que coincida con el nombre de host y dominio para ver su estado.

## Traiga su propia licencia
{: #byol}

Cuando tenga su propia licencia de sistema operativo, instálela en el {{site.data.keyword.baremetal_short}}, de acuerdo con las instrucciones del proveedor. Para obtener más información, consulte [La opción sin SO](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Siguientes pasos

  [2. Preparación del servidor para la instalación de SAP](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_256GB)

  [3. Particionamiento y sistemas de archivos](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-3-partitioning-and-file-systems)

  [4. Preparación de la red](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
