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

# Adición de almacenamiento externo al servidor
{: #storage}

## Configuración de almacenamiento externo
{: #set_up_storage}

Puede añadir almacenamiento externo al servidor suministrado (o servidores) si desea utilizarlo como dispositivo de copia de seguridad o utilizar una instantánea para restaurar rápidamente la base de datos en un entorno de prueba. Para el ejemplo de tres capas, se utiliza almacenamiento en bloque para archivar los archivos de registro de la base de datos y copias de seguridad en línea y fuera de línea para la base de datos. Se ha seleccionado el almacenamiento en bloque más rápido (4 IOPS por GB) para garantizar el tiempo mínimo de copia de seguridad. Un almacenamiento en bloque más lento puede ser suficiente según sus necesidades. Para obtener más información sobre {{site.data.keyword.blockstoragefull}}, consulte [Iniciación a Almacenamiento en bloque](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started#getting-started).


1. Inicie sesión en el [portal de clientes de la infraestructura de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){: new_window} con sus credenciales exclusivas.
2. Seleccione **Almacenamiento** > **Almacenamiento en bloque.**
3. Pulse **Realizar pedido de almacenamiento en bloque** en la esquina superior derecha de la página de almacenamiento en bloque.
4. Seleccione los detalles en función de sus necesidades de almacenamiento. La tabla 1 contiene los valores recomendados, incluyendo 4 IOPS/GB para una carga de trabajo de base de datos típica.

|              Campo               |      Valor                                        |
| -------------------------------- | ------------------------------------------------- |
|Ubicación                          | TOR01                                             |
|Método de facturación                    | Mensual (predeterminado)                                 |
|Nuevo tamaño de almacenamiento                  | 1000 GB                                           |
|Opciones de IOPS de almacenamiento              | Resistencia (IOPS por niveles) (predeterminado)                 |
|IOPS por niveles de resistencia             | 10 GB                                             |
|Tamaño de espacio para instantáneas               | 0 GB                                              |
|Tipo de SO                           | El valor predeterminado es Linux                                 |
{: caption="Tabla 1. Valores recomendados para el almacenamiento en bloque" caption-side="top"}

5. Pulse los dos recuadros de selección y pulse **Realizar pedido**.

## Autorización de hosts
{: authorize-host}

1. Pulse **Acciones** a la derecha del LUN y seleccione **Autorizar host** para acceder al almacenamiento suministrado.
2. Seleccione **Dispositivos**; el valor predeterminado para **Tipo de dispositivo** es Servidor nativo. Pulse **Hardware** y seleccione los nombres de host de los dispositivos.
3. Pulse el botón **Enviar**.
4. Compruebe el estado del almacenamiento suministrado en el separador **Dispositivos** > (seleccione el dispositivo) > **Almacenamiento**.
5. Apunte la **Dirección de destino** y el Nombre calificado iSCSI (**IQN**) para su servidor (iniciador iSCSI) y el **nombre de usuario** y **contraseña** para la autorización con el servidor iSCSI. Necesitará esta información en los siguientes pasos.
6. Siga los pasos descritos en [Conexión a los LUN iSCSI en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#connecting-to-mpio-iscsi-luns-on-linux) para poder acceder al almacenamiento desde el servidor suministrado.

## Creación de multivía de acceso para el almacenamiento
{: #multipath}

En el despliegue de ejemplo, recuperaba los siguientes datos del separador **Almacenamiento**:
  * IP de destino: `10.2.62.78`
  * IQN: `iqn.2005-05.com.softlayer:SL01SU276540-H896345`
  * Usuario: `SL01SU276540-H896345`
  * Contraseña: `EtJ79F4RA33dXm2q`

1. Escriba lo siguiente sobre la base de la información recuperada:
```
[root@sdb192 ~]# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2005-05.come.softlayer:SL01SU276540-H986345
```
   Una entrada existente podría tener que sustituirse en `/etc/iscsi/initiatorname.iscsi`.

2. Añada las siguientes líneas al final de `/etc/iscsi/iscsid.conf`:
```
[root@sdb192 ~]# tail /etc/iscsi/iscsid.conf
# continua para responder a R2Ts. Para habilitar esto, elimine el comentario de esta línea
# node.session.iscsi.FastAbort = No
node.session.auth.authmethod = CHAP
node.session.auth.username = SL01SU276540-H896345
node.session.auth.password = EtJ79F4RA33dXm2q
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = SL01SU276540-H896345
discovery.sendtargets.auth.password = EtJ79F4RA33dXm2q
```

3. Sustituya los valores de `nombre de usuario` y `contraseña` del paso 2 por los que ha anotado en el paso 5 de *Autorización de hosts*.

4. Descubra el destino iSCSI especificando las siguientes líneas.
```
[root@sdb192 ~]# iscsiadm -m discovery -t sendtargets -p "10.2.62.78"
10.2.62.78:3260,1031 iqn.1992-08.com.netapp:tor0101
10.2.62.87:3260,1032 iqn.1992-08.com.netapp:tor0101
```

5. Establezca el host para que inicie sesión automáticamente en la matriz iSCSI.

      `[root@sdb192 ~]# iscsiadm -m node -L automatic`

6. Instale e inicie el daemon de multivía de acceso.
```
[root@sdb192 ~]# yum install device-mapper-multipath
…
[root@sdb192 ~]# chkconfig multipathd on
[root@sdb192 ~]# service multipathd start
```

7. Complete todos los mandatos de [Conexión a los LUN iSCSI en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux) para que aparezca otro LUN en la salida multivía.
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

Ahora ya puede utilizar el dispositivo con multivía de acceso como utilizaría cualquier dispositivo de disco. Aparece una vía de acceso del dispositivo en `/dev/mapper/3600a098038303452543f464142755a42`.

Tome la muestra `/etc/multipath.conf` del [ejemplo `multipath.conf`](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-sample) y créela en su servidor. Tenga en cuenta que los caracteres especiales copiados, retornos de carro como DOS o entradas de salto de línea podrían provocar un comportamiento inesperado. Asegúrese de tener un archivo ASCII Unix después de copiar el contenido.

Adapte el bloque de multivía de acceso de `/etc/multipath.conf` para crear un alias de la vía de acceso para acceder al dispositivo en `1/dev/mapper/mpath1`.

      multipaths {
	       multipath {
		         wwid 3600a098038303452543f464142755a42
		         alias mpath1
	       }
     }

8. Reinicie `multipathd`. Ahora puede crear `/backup filesystem` y montarlo en el dispositivo de bloque.

      [root@sdb192 ~]# service multipathd restart
      [root@sdb192 ~]# mkfs.ext4 /dev/mapper/mpath1
      [root@sdb192 ~]# mkdir  /backup

9. Compruebe los sistemas de archivos en ambos servidores. El resultado debería ser similar a la siguiente salida.

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

Si instala una aplicación SAP basada en SAP NetWeaver on {{site.data.keyword.Db2_on_Cloud_long}}, deberá crear subdirectorios en `/backup` propiedad del usuario de administrador de base de datos (`db2SID`) para copias de seguridad completa y archivos de registro archivados. Para el archivado automático de los archivos de registro, debe establecer `LOGMETH1` en la base de datos {{site.data.keyword.Db2_on_Cloud_short}}. Consulte la [documentación de {{site.data.keyword.Db2_on_Cloud_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/support/knowledgecenter/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0051344.html){: new_window} para obtener detalles.
