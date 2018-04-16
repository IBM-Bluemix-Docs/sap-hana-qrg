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

# 1. Realización del pedido del servidor de 32 GB
{: #install_32GB}

## Realización del pedido de servidor
{: #order_32GB}

1. Inicie sesión en el [portal del cliente de la infraestructura de {{site.data.keyword.cloud_notm}}](https://control.softlayer.com) con sus credenciales exclusivas.
2. Pulse **Dispositivos** en la página Resumen de la cuenta.
3. Pulse **Mensual** en {{site.data.keyword.baremetal_short}} en la página Dispositivos. Se abre el recuadro de diálogo Lista de servidores.
4. Pulse el hiperenlace en **Precio inicial al mes** para seleccionar el servidor **BI.S1.NW32 (Opciones de sistema operativo).**

## Selección de las opciones de servidor
{: #options_32GB}

1. Deje **1** en el campo **Cantidad**.
2. Seleccione **TOR01** para **Centro de datos.** La lista de los centros de datos depende de la disponibilidad del producto en un centro de datos determinado.
3. **Servidor** se establece de forma predeterminada en un valor predefinido sobre la base de la selección de servidor y no se puede cambiar. 
4. Pulse **32 GB RAM** aunque **RAM** se establece de forma predeterminada en un valor predefinido sobre la base de la selección de servidor y no se puede cambiar.
5. Seleccione **Red Hat Enterprise Linux for SAP Business Application 6.X** como **Sistema operativo.**
6. En **Discos duros**, seleccione un segundo disco SATA de 2 TB, cree un grupo de almacenamiento RAID de RAID1 desde ambos discos que cubra la cantidad total de almacenamiento, y seleccione **Linux Basic** como **Plantilla de partición.** Deje **LVM** sin marcar.
7. Seleccione **500 GB** para el **Ancho de banda público.**
8.	Seleccione **1 Gbps de enlaces ascendentes de red pública y privada redundantes** para **Velocidad de puerto de enlace ascendente.**
9. Deje los valores predeterminados para todos los demás campos. Para obtener descripciones detalladas de las opciones, consulte [Configuración de los servidores nativos](https://console.bluemix.net/docs/bare-metal/configuring.html#setting-up-your-bare-metal-servers).
10.	Pulse **Añadir a pedido** en la parte inferior de la página. Se le redirigirá a la página de pago una vez verificado su pedido.

## Ajuste de las configuraciones avanzadas del sistema
{: #adv_config}

Utilice los valores de la Tabla 1 para los campos de la Configuración avanzada del sistema. Encontrará más información disponible en las directrices de [Configuración avanzada del sistema](https://console.bluemix.net/docs/bare-metal/configuring.html#advanced-system-configuration).

1. Desplácese hacia abajo y especifique los valores en la Tabla 1 para la **Configuración avanzada del sistema.**

|              Campo               |      Valor                                        |
| -------------------------------- | -------------------------------------------------------------------- |
|VLAN de fondo                     | Seleccione de la lista desplegable, por ejemplo `tor01.bcr01a.1241`  |
|Subred                            | Seleccione de la lista desplegable, por ejemplo `10.114.63.64/26`    |
|VLAN frontal                      | Seleccione de la lista desplegable, por ejemplo `tor01.fcr01a.1199`  |
|Subred                            | Seleccione de la lista desplegable, por ejemplo `169.55.137.160/27`  |
|Scripts de suministro             | El valor predeterminado es Ninguno                                                    |
|Claves SSH                        | Añadir                                                                  |
|Nombre de host                    | Por ejemplo, `e2e1270`                                               |
|Dominio                           | Por ejemplo, `saptest.com`                                           |
{: caption="Tabla 1. Valores de configuración avanzada de 32 GB" caption-side="top"}  

## Confirmación de las selecciones
{: #confirm_selections}

1. Confirme las selecciones en la página de pago, y pulse **Términos de los servicios en la nube** y **Acuerdo de software de terceros** en la parte derecha de la página.
2. Pulse **Enviar pedido** en la parte derecha del formulario. Se le redirigirá a una página con el número de pedido. Puede imprimir la página, ya que también es su recibo del pedido. Además, recibirá un correo electrónico de confirmación con el asunto *Se ha aprobado el pedido número ## de IBM Cloud*, donde ## es el número de pedido.

Una vez enviado el pedido, su servidor estará disponible, dependiendo de su pedido, para su uso en un plazo de una a cuatro horas. Puede comprobar el estado de los distintos pasos del suministro en la pantalla Detalles del dispositivo en la página principal del portal del cliente (**Dispositivos > Lista de dispositivos**). Pulse el **Nombre de dispositivo** que coincida con el nombre de host y dominio para ver su estado.

## Siguientes pasos
 
  [2. Preparación del servidor para la instalación de SAP](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)
  
  [3. Particionamiento y sistemas de archivos](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
