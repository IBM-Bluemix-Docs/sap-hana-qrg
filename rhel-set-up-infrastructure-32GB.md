---

copyright:
  years: 2018, 2020
lastupdated: "2020-05-05"

keywords: SAP NetWeaver, bring your own license, BYOL, VLAN, 32 GB infrastructure

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 1. Ordering your 32 GB server
{: #install_32GB}

## Ordering your server
{: #order_32GB}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} with your unique credentials.
2. Click **Create** > **Compute** > **Infrastructure** > **Bare Metal Server**.
3. Click **Continue**. If you can't click **Continue**, you don't have the correct permissions to create a server. Check with your system administrator about your permissions.
4. Leave **1** in the **Quantity** field.
5. Enter `e2e1270` in the **Hostname** field. Hostname is a permanent or temporary name for your servers. Click **Information** for formatting specifics.
6. Enter `mycloud.com` in the **Domain** field. Domain is the identification string that defines administrative control within the internet. Click **Information** for formatting specifics.
7. **Billing** defaults to **Monthly**. At this time 1-year contract and 3-year contract are not available for SAP-certified servers.
8. The data centers displayed under **Location** depend on product availability within a particular data center. Select **NA East TOR01-Toronto**.
9. Click **All servers** > **SAP certified**.

## Configuring your server
{: #configure_server-32GB}

1. Select **CPU Model BI.S3.NW32 (OS Options)**. For information on how to decipher the server names, see [Provisioning your {{site.data.keyword.baremetal_short}} using the {{site.data.keyword.cloud_notm}} console](/docs/sap-netweaver?topic=sap-netweaver-set_up_infrastructure#order-server).
2. **RAM** defaults to a predefined value based on your server selection and cannot be changed.
3. Enter an optional public key for your **SSH key**, which you can use to log in to your server after it's provisioned. The default is **None**.
4. Select **RedHat** as your **Image** (OS). The default is **7.x (64 bit)**.

   If you're bringing your own license (BYOL) for your OS, select **No OS**. For more information, see [Bring your own license](#byol-32GB).
   {: note}

## Adding storage disks
{: #adding-storage-disks-32GB}

1. Under **Type**, select **RAID 10**.
2. **Disks**, **Hot Spare**, and **Disk Media** have default values. Select a **Disk size** that covers the total amount of storage you need.
3. Click the Menu icon ![Menu icon](../../icons/action-menu-icon.svg) > **Advanced configuration** and leave **Controller** unchecked. Click **OK**.

## Network interface
{: #network-interface-32GB}

1. Select **1 Gbps Redundant Public and Private Network Uplinks** for **Uplink Port Speed**.
2. Select the values in Table 1 for the following fields:

|              Field               |      Value              |
| -------------------------------- | ------------------------|
| Private VLAN                     | `tor01.bcr01a.1241`     |
| Public VLAN                      | `tor01.fcr01a.851`      |
| Private Subnet                   | `10.114.63.64/26`       |
| Public Subnet                    | `158.85.65.224/28`      |
{: caption="Table 1. 32 GB network interface values" caption-side="top"}  

3. Leave the default values for all other fields.
4. Review your Order Summary.
5. Click **I read and agree to the following Third-Party Service Agreements**.

   You can create your server, save the order as a quote to provision at a later time, or add the order to an estimate, which may include multiple services.
   {: note}

6. Click **Create** to be redirected to the Checkout page after your order has been verified.

You are redirected to a page with your order number. You can print the page because it's your order receipt. In addition, you receive a confirmation email with the subject *Your IBM Cloud Order ## has been approved* with ## being your order number.

After the order is submitted, your server is, depending on your order, available for use within one to four hours. You can check the Device Details from the {{site.data.keyword.cloud_notm}} console (Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Resource List > Devices) for a status of the provisioning steps. Click the **Device Name** that matches your device's Hostname and Domain to see its status.

## Bring your own license
{: #byol-32GB}

When you have your own operating system license, you install it on your {{site.data.keyword.baremetal_short}} based on the vendor's instructions. For more information, see [The no OS option](/docs/bare-metal?topic=bare-metal-bm-no-os#bm-no-os).

## Next Steps
{: #next-steps-32GB}

  [2. Preparing your server for your SAP installation](/docs/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_32GB)

  [3. Partitioning and filesystems](/docs/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-partition_32GB)
