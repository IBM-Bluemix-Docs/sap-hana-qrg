---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-01"

keywords:

subcollection: sap-netweaver-rhel-qrg

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Ordering your 32 GB server
{: #install_32GB}

## Ordering your server
{: #order_32GB}

1. Log in to the [{{site.data.keyword.cloud_notm}} infrastructure customer portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com){: new_window} with your unique credentials.
2. Click **Account** > **Place an Order** on the Account Summary page.
3. Click **Monthly** under {{site.data.keyword.baremetal_short}} on the Devices page. The Server List appears; the SAP-Certified Servers are at the top of the list.
4. Click the hyperlink under **STARTING PRICE PER MONTH** to select server **BI.S3.NW32 (OS Options).**

The BI.S3.NW32 (OS Options) server is also available for **Hourly** billing.
{: note}

## Configuring your server
{: #configure_server}

1. Leave **1** in the **Quantity** field.
2. Select **TOR01** for **Data Center.** The list of data centers depends on product availability within a particular data center.
3. **Server** defaults to a predefined value based on your server selection and cannot be change changed.
4. Click **32 GB RAM** even though the **RAM** selection defaults to a predefined value based on your sever selection and cannot be changed.
5. Click **Redhat** and select **Red Hat Enterprise Linux for SAP Business Application 7.X (64 bit)** as your **Operating System**. **Note**: If you are bringing your own license (BYOL) for your operating system, select **Other** > **No Operating System**. For more information, see [Bring your own license](#byol).
6. Add a second 2 TB SATA drive by clicking the **Disk Controller 1** drop-down menu, and selecting **2 TB SATA**. Click **Add Disk**.
7. Click **Select All Disks** and click **Create RAID storage group**.
8. Click **Type** and select **RAID 1**. Enter a **Size** that covers the total amount of storage you need.
9. Leave **LVM** unchecked and accept the default **Partition Template**, **Linux Basic**.
10. Click **Done**.

## Selecting your additional server options
{: #options_32GB}

1. Select **500 GB** for **Public Bandwidth.**
2.	Select **1 Gbps Redundant Public & Private Network Uplinks** for **Uplink Port Speed.**
3. Leave the default values for all other fields. For detailed option descriptions, see [Building a custom bare metal server](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#addl-server-options).
4.	Click **Add to Order** at the bottom of the page. You are redirected to the Checkout page after your order is verified.

## Setting up Advanced System Configurations
{: #adv_config}

Use the values in Table 1 for the fields under Advanced System Configuration. More information is available in [Advanced server configuration options](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server#adv-system-config) guidelines.

1. Scroll down and enter the values in Table 1 under **Advanced System Configuration.**

|              Field               |      Value                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Backend VLAN                      | Select from the drop-down list, for example, `tor01.bcr01a.1241`     |
|Subnet                            | Select from the drop-down list, for example, `10.114.63.64/26`       |
|Frontend VLAN                     | Select from the drop-down list, for example, `tor01.fcr01a.851`      |
|Subnet                            | Select from the drop-down list, for example, `158.85.65.224/28`      |
|Provision Scripts                 | Leave blank                                                          |
|SSH Keys                          | Defaults to `Add`, which means no SSH key                            |
|Hostname                          | For example, `e2e1270`                                               |
|Domain                            | For example, `saptest.com`                                           |
{: caption="Table 1. 32 GB advanced configuration values" caption-side="top"}  

## Confirming your selections
{: #confirm_selections}

1. Confirm your selections on the Checkout page, and click **Cloud Service terms** and **3rd Party Software Agreement** on the right-hand side of the page.
2. Click **Submit Order** on the right-hand side of the form. You are redirected to a page with your order number; you can print the page because it's also your order receipt. In addition, you receive a confirmation email with the subject *Your IBM Cloud Order ## has been approved* with ## being your order number.

After the order is submitted, your server is, depending on your order, available for use within one to four hours. You can check the Device Details screen on the main infrastructure customer portal page (**Devices > Device List**) for a status of the provisioning steps. Click the **Device Name** that matches your given Hostname and Domain to see its status.

## Bring your own license
{: #byol}

When you have your own operating system license, you install it on your {{site.data.keyword.baremetal_short}} based on the vendor's instructions. For more information, see [The no OS option](/docs/bare-metal?topic=bare-metal-the-no-os-option#how-to-install-an-operating-system-on-a-no-os-server-).

## Next Steps

  [2. Preparing your server for your SAP installation](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-prepare_32GB)

  [3. Partitioning and filesystems](/docs/infrastructure/sap-netweaver-rhel-qrg?topic=sap-netweaver-rhel-qrg-partition_32GB)
