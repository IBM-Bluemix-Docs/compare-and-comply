---

copyright:
years: 2018
lastupdated: "2018-12-27"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Understanding deployment configurations
{: #configs}

The default deployment configuration for Compare and Comply is `2Cores 10G 1 concurrent document (2 VPC)`. This configuration can process approximately 1500 pages per hour. You can scale up your Compare and Comply deployment on IBM Cloud Private to different levels depending on your throughput requirements. Contact your IBM sales representative for more information on increasing the capacity of Compare and Comply on IBM Cloud Private.

Compare and Comply can be scaled on two dimensions: _vertical_ and _horizontal_.

 - _Vertical_ scaling increases the throughput at which a document is parsed.
 - _Horizontal_ scaling increases the number of concurrent documents parsed.

Both scaling options require increasing the number of Virtual Processor Cores (VPCs) used by Compare and Comply. For information about VPCs, see the IBM Cloud Private [licensing documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window}.

Vertical scaling options include the following:

| Configuration                             |Approximate throughput*         |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |1500 pages per hour (default)   |
|`4Cores 20G 1 concurrent document (4 VPC)` |2500 pages per hour             |
|`8Cores 40G 1 concurrent document (8 VPC)` |3000 pages per hour             |

Horizontal scaling options include the following:

| Configuration                               |Approximate throughput*         |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |3000 pages per hour             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |5000 pages per hour             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |6000 pages per hour             |

\***Note**: Throughput estimates are based on internal testing. Throughput in a production environment depends on multiple factors including other workloads on the same IBM Cloud Private cluster, latencies between the client application and the IBM Cloud Private cluster, and other environmental conditions.

To change the deployment configuration by adding or removing Compare and Comply clusters, perform the following steps.

**Note**: If you scale down a Compare and Comply deployment, IBM Cloud Private immediately reclaims the released resources, thus reducing the processing capacity of the deployment. Do not scale down a deployment that is being used, particularly if the deployment is in a production environment.

Similarly, if you scale up a Compare and Comply deployment, IBM Cloud Private immediately applies the requested resources to the deployment, assuming the resources are available. If you need more resources than your IBM Cloud Private installation can provide, talk with your IBM sales representative about increasing the capacity of your IBM Cloud Private installation.

  1. Log in to your IBM Cloud Private cluster.

  1. From the menu icon near the upper left-hand corner, select **Workloads -> Deployments**.

  1. The console displays the **Deployments** page. Locate the deployment you want to scale in the table of deployments.

  1. In the table row that lists your deployment, click the **Action** icon in the last column of the table, then click **Scale**.

  1. The console displays the **Scale Deployment** window. In the **Instances** field, enter the desired number of instances, or use the up and down arrows to select the desired value.

  1. Click **Scale Deployment**.

  1. The **Scale Deployment** window closes and the console returns to displaying the **Deployments** page. Find your deployment and use the **Desired** and **Current** columns to verify that your requested number of instances have been allocated to the deployment.

  1. Optionally, click the name of the deployment on the **Deployments** page to view details of the deployment.
