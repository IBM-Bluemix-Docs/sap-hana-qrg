---



copyright:
  years: 2018
lastupdated: "2018-06-18"


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

1. Log in to the [{{site.data.keyword.cloud_notm}} infrastructure customer portal](https://control.softlayer.com) with your unique credentials.
2. Click **Devices** on the Account Summary page.
3. Click **Monthly** under {{site.data.keyword.baremetal_short}} on the Devices page. The Server List dialog box appears.
4. Click the hyperlink under **STARTING PRICE PER MONTH** to select server **BI.S1.NW32 (OS Options).**

## Configuring your server
{: #configure_server}

1. Leave **1** in the **Quantity** field.
2. Select **TOR01** for **Data Center.** The list of data centers depends on product availability within a particular data center.
3. **Server** defaults to a predefined value based on your server selection and cannot be change changed.
4. Click **32 GB RAM** even though the **RAM** selection defaults to a predefined value based on your sever selection and cannot be changed.
5. Click **Redhat** and select **Red Hat Enterprise Linux for SAP Business Application 6.X (64 bit)** as your **Operating System**.
6. Add a second 2 TB SATA drive by clicking the **Disk Controller 1** drop-down menu, and selecting **2 TB SATA**. Click **Add Disk**.
7. Click **Select All Disks** and click **Create RAID Storage Groups**.
8. Click **Type** and select **RAID 1**. Enter a **Size** that covers the total amount of storage you need.
9. Leave **LVM** unchecked and accept the default **Partition Template**, **Linux Basic**.
10. Click **Done**.

## Selecting your server options
{: #options_32GB}

1. Select **500 GB** for **Public Bandwidth.**
2.	Select **1 Gbps Redundant Public & Private Network Uplinks** for **Uplink Port Speed.**
3. Leave the default values for all other fields. For detailed option descriptions, see [Setting Up a Bare Metal Server](https://console.bluemix.net/docs/bare-metal/set-bare-metal-server-2.html#customerportal_setupbaremetal).
4.	Click **Add to Order** at the bottom of the page. You are redirected to the Checkout page after your order is verified.

## Setting up Advanced System Configurations
{: #adv_config}

Use the values in Table 1 for the fields under Advanced System Configuration. More information is available in [Advanced server configuration options](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#advanced-server-configuration-options) guidelines.

1. Scroll down and enter the values in Table 1 under **Advanced System Configuration.**

|              Field               |      Value                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Backend VLAN                      | Select from the drop-down list, for example, `tor01.bcr01a.1241`     |
|Subnet                            | Select from the drop-down list, for example, `10.114.63.64/26`       |
|Frontend VLAN                     | Select from the drop-down list, for example, `tor01.fcr01a.1199`     |
|Subnet                            | Select from the drop-down list, for example, `169.55.137.160/27`     |
|Provision Scripts                 | Defaults to None                                                     |
|SSH Keys                          | Add                                                                  |
|Hostname                          | For example, `e2e1270`                                               |
|Domain                            | For example, `saptest.com`                                           |
{: caption="Table 1. 32GB advanced configuration values" caption-side="top"}  

## Confirming your selections
{: #confirm_selections}

1. Confirm your selections on the Checkout page, and click **Cloud Service terms** and **3rd Party Software Agreement** on the right-hand side of the page.
2. Click **Submit Order** on the right-hand side of the form. You are redirected to a page with your order number; you can print the page because it's also your order receipt. In addition, you receive a confirmation email with the subject *Your IBM Cloud Order ## has been approved* with ## being your order number.

After the order is submitted, your server is, depending on your order, available for use within one to four hours. You can check the Device Details screen on the main Customer Portal page (**Devices > Device List**) for a status of the provisioning steps. Click the **Device Name** that matches your given Hostname and Domain to see its status.

## Next Steps
 
  [2. Preparing your server for your SAP installation](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-32GB.html)
  
  [3. Partitioning and filesystems](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-32GB.html)
