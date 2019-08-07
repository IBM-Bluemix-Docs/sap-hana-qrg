---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-07"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN, application server, database server, three tier, SAP certified servers

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 1. Ordering 192 GB and 32 GB servers for a three-tier setup
{: #install_three_tier}

## Ordering your servers
{: #order_servers}

Follow the steps in [Ordering your 32 GB server](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-install_32GB#order_32GB) to order the SAP NetWeaver application server. The following steps guide you through ordering the database server.

## Ordering your database server
{: #order-db-server}

Use the following steps to order an SAP-certified server as your database server.

1. Log in to the [{{site.data.keyword.cloud}} console)](https://cloud.ibm.com){: external} with your unique credentials.
2. Click **Create resource** > **Compute** > **{{site.data.keyword.baremetal_short_sing}}**.
3. Click **Continue.** If you can't click **Continue**, you don't have the correct permissions to create a server. Check with your system administrator about your permissions.
4. Leave **1** in the **Quantity** field.
5. Enter `sdb192` in the **Hostname** field. Hostname is a permanent or temporary name for your servers. Click **Information** for formatting specifics.
6. Enter 'mycloud.com' in the **Domain** field. Domain is the identification string that defines administrative control within the internet. Click **Information** for formatting specifics.
7. **Billing** defaults to **Monthly**. At this time, 1-year contract and 3-year contract are not available for SAP-certified servers.
8. The data centers displayed under **Location** depend on product availability within a particular data center. Select **NA East, TOR01-Toronto**.
9. Click **All servers** > **SAP certified**.

## Configuring your database server
{: #options_192GB}

Use the following steps to configure your database server and OS.

1. Select **CPU Model BI.S3.NW192 (OS Options)**. For information on how to decipher the server names, see [Provisioning your {{site.data.keyword.barmetal_short}} using the {{site.data.keyword.cloud_notm}} console](/docs/infrastructure/sap-netweaver?topic=sap-netweaver-set_up_infrastructure#using-console).
2. **RAM** defaults to a predefined value based on your server selection and cannot be changed.
3. Enter an optional public key for **SSH key**, which you can use to log in to your server after it's provisioned. The default is **None**.
4. Select **RedHat** as your **Image** (OS). The default is **7.x (64 bit)**.

  If you're bringing your own license (BYOL) for your OS, select **No OS** as your image. For more information, see [Bring your own license](#byol).
  {: note}

## Adding storage disks
{: #adding-storage-disks}

Use the following steps to add a second 2 TB SATA drive to your database server.

1. For **Disk 1**, click the Menu icon ![Menu icon](../../icons/action-menu-icon.svg) > **Advanced configuration** > and verify that  **Primary disk partition** is set to the default of **Windows Basic**. Click **OK**.
2. Click **Add new**.
3. **Disks**, **Hot Spare**, and **Disk Media** have default values. Select a **Disk Size** that covers the total amount of storage you need.

## Setting up the network interface
{: #network-options}

Use the following steps to set up the network interface for your database server.

1. Select **1 Gbps Redundant Public & Private Network Uplinks** for **Uplink Port Speed**.
2. Select the values in Table 1 for the following fields:

  Make sure the network interface values for your database server match those of your application server.
  {: note}

|              Field               |      Value              |
| -------------------------------- | ------------------------|
| Private VLAN                     | `tor01.bcr01a.1241`     |
| Public VLAN                      | `tor01.fcr01a.851`      |
| Private Subnet                   | `10.114.63.64/26`       |
| Public Subnet                    | `158.85.65.224/28`      |
{: caption="Table 1. 192 GB network interface values" caption-side="top"}

3. Leave the default values for all other fields.
4. Review your Order Summary.
5. Select **I read and agree to the following Third-Party Service Agreements**.

  You can create your server, save the order as a quote to provision at a later time, or add the order to an estimate, which may include multiple services.
  {: note}

6. Click **Create** to be redirected to the Checkout page after your order is verified.

You are redirected to a page with your order number. you can print the page, because it's your receipt. In addition, you receive a confirmation email with the subject _Your {{site.data.keyword.cloud_notm}} Order ## has been approved_ with ## being your order number.

After the order is submitted, the server, depending on your order, is available for use within one to four hours. You can check Device Details from the {{site.data.keyword.cloud_notm}} console (Menu icon ![Menu icon](../../icons/icon-hamburger.svg) > Resource List > Devices) for a status of the provisioning steps. Click the **Device Name** that matches your given Hostname and Domain to see its status.

## Bring your own license
{: #byol}

When you have your own operating system license, you install it on your {{site.data.keyword.baremetal_short}} based on the vendor's instructions. For more information, see [The no OS option](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Next Steps

  [2. Preparing your server for your SAP installation](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_256GB)

  [3. Partitioning and filesystems](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-3-partitioning-and-file-systems)

  [4. Preparing your network](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-network#network)
