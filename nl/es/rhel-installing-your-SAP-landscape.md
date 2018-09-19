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

# Instalación del entorno SAP
{: #install_landscape}

## Instalación de paquetes RPM (requisito previo)
{: #RPM}

Una instalación de SAP exige determinados requisitos previos para los paquetes que se instalan en el sistema operativo y los daemons del sistema operativo en ejecución. Consulte las últimas [guías de instalación](https://support.sap.com/software/installations.html) de SAP (requiere un [ID S-user de SAP](/docs/infrastructure/sap-netweaver/sap-index.html#getting-started)) y las [notas de soporte](https://support.sap.com/notes) (requiere un ID S-user de SAP) para obtener una lista actualizada de los requisitos previos. Hay dos paquetes más que deben instalarse:
* compat-sap-c++: permite generalmente la compatibilidad del tiempo de ejecución C++ con los compiladores que utiliza SAP. Puesto que se ha seleccionado Red Hat Enterprise Linux for SAP Business Application 7.X como SO para el servidor de aplicaciones de 32 GB y el servidor de base de datos de 192 GB, se utilizará `compat-sap-c++-7`.
* uuidd: mantiene el soporte de sistema operativo para la creación de los UUID.

### Comprobación si uuidd está instalado

1. Compruebe si el `daemon uuid (uuidd)` está instalado. Si no lo está, instálelo e inícielo.
```
[root@sdb192 ~]# rpm -qa | grep uuidd
[root@sdb192 ~]# yum install uuidd
[root@sdb192 ~]# chkconfig uuidd on
[root@sdb192 ~]# service uuidd start
```

### Instalación del paquete compat-sap-c++-7

1. Siga la [Nota de SAP 2195019](https://launchpad.support.sap.com/#/notes/2195019) e instale el paquete compat-sap-c++-7 y cree un enlace dinámico específico, que necesitan los binarios de SAP. Consulte las Notas de SAP específicas del release del producto que va a instalar para determinar si la biblioteca es necesaria o no.
```
[root@sdb192 ~]# yum install compat-sap-c++-7-7.2.1-2.e17_4.x86_64.rpm
....
[root@sdb192 ~]# mkdir -p /usr/sap/lib
[root@sdb192 ~]# ln -s /opt/rh/SAP/lib64/compat-sap-c++.so /usr/sap/lib/libstdc++.so.6
```

## Descarga del software de SAP
{: download_software}

Inicie sesión en el [Portal de soporte de SAP](https://support.sap.com/en/index.html), pulse **Descargar software** y descargue los DVD necesarios en una unidad compartida local. Transfiera los archivos a su servidor suministrado. Otra opción es descargar [SAP Software Download Manager](https://support.sap.com/en/my-support/software-downloads.html#section_995042677), instalarlo en el servidor de destino y descargar directamente las imágenes de DVD en el servidor.

## Preparación de la interfaz gráfica de usuario SWPM de SAP
{: #prepare_for_GUI}

En función de la latencia y el ancho de banda de red, es posible que desee ejecutar la interfaz gráfica de usuario (GUI) de SAP Software Provisioning Manager (SWPM) de forma remota en una sesión de sistema de red virtual (VNC). Otra opción es ejecutar la interfaz gráfica de usuario de forma local y conectarse a SWPM en la máquina de destino. Consulte la [documentación de SWPM](https://wiki.scn.sap.com/wiki/display/SL/Software+Provisioning+Manager+1.0+and+2.0) si decide ejecutar la GUI de forma local.

Los siguientes pasos describen la ejecución de la interfaz gráfica de usuario de SWPM de forma remota en una sesión de VNC. Esta opción instala un servidor VNC, que podría no estar en consonancia con el refuerzo de su sistema operativo, asegúrese de que cumple con sus medidas de seguridad. Consulte la [documentación de VNC](http://searchnetworking.techtarget.com/definition/virtual-network-computing) para obtener una visión general de sus funciones, si no está familiarizado con él.

1. Utilice los mandatos siguientes para instalar un servidor VNC.
```
  [root@sdb192 ~]# yum install tigervnc-server

  ...
```

2. Utilice el siguiente mandato para instalar el gestor de ventanas X11, `twm`, que se incluye en la distribución Linux.

`[root@sdb192 ~]# yum install twm`

3. Instale un emulador de terminal, por ejemplo, `xterm`.

 `[root@sdb192 ~]# yum install xterm`

4. Inicie el servidor VNC desde la línea de mandatos.

 `[root@sdb192 ~]# vncserver`

Ahora necesita un programa cliente VNC; existen varias implementaciones gratuitas disponibles para todos los sistemas operativos. Normalmente, necesita tener acceso a un puerto `590X` (donde X es el número de servidores en ejecución, empezando por 1) desde el cliente.

Puede que tenga que iniciar un `xterm` desde el menú de fondo de `twm`. Puede iniciar `SWPM` (`sapinst`) desde `xterm`.

## Instalación del software de SAP
{: #install_software}

Después de descargar el soporte de instalación, siga el procedimiento de instalación de SAP estándar que se documenta en la guía de instalación de SAP para su versión y componentes de SAP, y las Notas de SAP correspondientes.

Puede iniciar SWPM de SAP desde `xterm`, y ejecutar los pasos de instalación.

### Instalación del software de SAP en un entorno de tres capas

Siga los pasos de SWPM de SAP para una configuración de tres capas.

1. Seleccione **Sistema distribuido** y realice la instalación de ASCS y la instalación de la base de datos en el servidor de base de datos.
2. Instale Application Server ABAP en el servidor de aplicaciones. Asegúrese de utilizar las direcciones privadas para ASCS y los nombres de host de base de datos durante la instalación del servidor de aplicaciones.

El uso de direcciones privadas y nombres de host garantiza que el tráfico de red entre el servidor de aplicaciones y ASCS, o la base de datos, pase a través de la red privada y no a través de la red pública.
