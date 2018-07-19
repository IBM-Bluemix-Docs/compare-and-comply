---

copyright:
years: 2018
lastupdated: "2018-06-28"

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

# 了解部署配置
{: #configs}

{{site.data.keyword.cnc_short}} 的缺省部署配置是 `2 个核心，10G，1 个并行文档（2 个 VPC）`。此配置每小时可以处理约 1500 页。可以根据吞吐量需求，将 IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} 部署向上扩展到其他级别。有关在 IBM Cloud Private 上增大 {{site.data.keyword.cnc_short}} 能力的更多信息，请联系 IBM 销售代表。

{{site.data.keyword.cnc_short}} 可以在两个维度上进行扩展：_垂直_和_水平_。

 - _垂直_扩展会增加解析文档时的吞吐量。
 - _水平_扩展会增加解析的并发文档数。

这两个扩展选项都需要增加 {{site.data.keyword.cnc_short}} 使用的虚拟处理器核心 (VPC) 数量。有关 VPC 的信息，请参阅 IBM Cloud Private [许可文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window}。

垂直扩展选项包括以下配置：

|配置|近似吞吐量*|
|:-----------------------------------------:|--------------------------------|
|`2 个核心，10G，1 个并行文档（2 个 VPC）` |1500 页/小时（缺省值）|
|`4 个核心，20G，1 个并行文档（4 个 VPC）` |2500 页/小时|
|`8 个核心，40G，1 个并行文档（8 个 VPC）` |3000 页/小时|

水平扩展选项包括以下配置：

|配置|近似吞吐量*|
|:-------------------------------------------:|--------------------------------|
|`2 个核心，10G，2 个并行文档（4 个 VPC）`  |3000 页/小时|
|`4 个核心，20G，2 个并行文档（8 个 VPC）`  |5000 页/小时|
|`8 个核心，40G，2 个并行文档（16 个 VPC）` |6000 页/小时|

\***注**：吞吐量估算基于内部测试。生产环境中的吞吐量取决于多个因素，包括同一 IBM Cloud Private 集群中的其他工作负载、客户机应用程序与 IBM Cloud Private 集群之间的等待时间以及其他环境条件。

要通过添加或除去 {{site.data.keyword.cnc_short}} 集群来更改部署配置，请执行以下步骤。

**注**：如果向下扩展 {{site.data.keyword.cnc_short}} 部署，IBM Cloud Private 将立即回收释放的资源，从而降低部署的处理能力。不要向下扩展正在使用的部署，尤其是部署处于生产环境中时。
	
与此类似，如果向上扩展 {{site.data.keyword.cnc_short}} 部署，在假定资源可用的情况下，IBM Cloud Private 会立即将所请求的资源应用于部署。如果您需要的资源多于 IBM Cloud Private 安装可提供的资源，请联系 IBM 销售代表，以增加 IBM Cloud Private 安装的容量。

  1. 登录到 IBM Cloud Private 集群。

  1. 从左上角附近的“菜单”图标中，选择**工作负载 -> 部署**。
  
  1. 控制台将显示**部署**页面。在部署表中找到要扩展的部署。
  
  1. 在列出部署的表行中，单击表的最后一列中的**操作**图标，然后单击**扩展**。
  
  1. 控制台将显示**扩展部署**窗口。在**实例**字段中，输入所需的实例数，或者使用向上和向下箭头来选择所需的值。
  
  1. 单击**扩展部署**。
  
  1. **扩展部署**窗口将关闭，并且控制台会恢复为显示**部署**页面。找到您的部署，并使用**所需**和**当前**列来验证是否已为部署分配了请求的实例数。
  
  1. （可选）单击**部署**页面上的部署名称以查看该部署的详细信息。
  
  

