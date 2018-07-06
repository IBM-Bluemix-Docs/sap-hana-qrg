---



copyright:
  years: 2017, 2018
lastupdated: "2018-07-06"


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 1. Ordering  256 GB and 3 2GB servers for a three-tier setup
{: #install_three_tier}

## Ordering your servers
{: #order_servers}

Follow the steps in [Ordering your 32 GB server](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-set-up-infrastructure-32GB.html#order_32GB) to order the SAP NetWeaver application server. The following steps guide you through ordering the database server.

## Ordering your database server
{: #order-db-server}

1. Log in to the [{{site.data.keyword.cloud_notm}} infrastructure customer portal](https://control.softlayer.com) with your unique credentials.
2. Click the **Devices** icon on the Account Summary page.
3. Click **Monthly** under **{{site.data.keyword.baremetal_long}}** on the Devices page. The Server List appears; the SAP-Certified Servers are at the top of the list.
4. Click the hyperlink under **STARTING PRICE PER MONTH** to select server **BI.S1.NW256 (OS Options).**

## Configuring your database server
{: #options_32GB}

1. Leave **1** in the **Quantity** field.
2. Select **TOR01** for **Data Center.**
3. **Server** defaults to a predefined value based on your server selection and cannot be change changed.
4. Click **32 GB RAM** even though the **RAM** selection defaults to a predefined value based on your sever selection and cannot be changed.
5. Click **Redhat** and select **Red Hat Enterprise Linux for SAP Business Application 6.X (64 bit)** as your **Operating System**.
6. Add a second 2 TB STAT drive by clicking the **Disk Controller 1** drop-down ment, and selecting **2 TB SATA**. Click **Add Disk**.
7. Click **Select All Disks** and click **Create RAID Storage Groups**.
8. Click **Type** and select **RAID 1**. Enter a **Size** that covers the total amount of storage you need.
9. Leave **LVM** unchecked and accept the default **Partition Template.**, **Linux Basic**.
10. Click **Done**.

## Selecting your additional database server options
{: #addl-server-options}

1. Select **500 GB** for **Public Bandwidth.**
2. Select **1 Gbps Redundant Public & Private Network Uplinks** for **Uplink Port Speed.**
3. For this example, leave the default values for all other fields. You can consult [Building a custom bare metal server](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#addl-server-options) for detailed descriptions of the options.
4.	Click **Add to Order** at the bottom of the page. You are redirected to the Checkout page after your order is verified.

## Setting up Advanced System Configurations
{: #adv_config}

1. Use the values in Table 1 for the fields under Advanced System Configuration. More information is available in the [Advanced System Configuration](https://console.bluemix.net/docs/bare-metal/baremetal-provision.html#adv-system-config) guidelines.

|              Field               |      Value                                                           |
| -------------------------------- | -------------------------------------------------------------------- |
|Backend VLAN                      | Select from the drop-down list, for example, `tor01.bcr01a.1241`     |
|Subnet                            | Select from the drop-down list, for example, `10.114.63.64/26`       |
|Frontend VLAN                     | Select from the drop-down list, for example, `tor01.fcr01a.1199`     |
|Subnet                            | Select from the drop-down list, for example, `169.55.137.160/27`     |
|Provision Scripts                 | Defaults to None                                                     |
|SSH Keys                          | Defaults to None                                                     |
|Hostname                          | For example, `e2e2690`                                               |
|Domain                            | For example, `saptest.com`                                           |
{: caption="Table 1. Advanced configuration values" caption-side="top"}  

## Confirming your selections
{: #confirm_selections}

1. Confirm your selections on the Checkout page, and click **Cloud Service terms** and **3rd Party Software Agreement** on the right-hand side of the page.
2. Click **Submit Order**. You are redirected to a screen with your order number. You can print the screen, because it is also your order receipt.

After the order is submitted, the server is, depending on your order, available for use within one to four hours. You can check the Device Details screen on the main {{site.data.keyword.slportal}} page (**Devices > Device List**) for a status of the provisioning steps. Click the **Device Name** that matches your given Hostname and Domain to see its status.

## Next Steps

  [2. Preparing your server for your SAP installation](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-server-256GB.html)
  
  [3. Partitioning and filesystems](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-partition-256GB.html)
  
  [4. Preparing your network](/docs/infrastructure/sap-netweaver-rhel-qrg/rhel-prepare-network.html#network)
