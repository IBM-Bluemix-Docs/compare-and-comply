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

# 瞭解部署配置
{: #configs}

{{site.data.keyword.cnc_short}} 的預設部署配置為 `2Cores 10G 1 concurrent document (2 VPC)`。此配置每小時大約可以處理 1500 個頁面。您可以根據您的傳輸量需求，將 IBM Cloud Private 上的 {{site.data.keyword.cnc_short}} 部署擴增至不同的層次。如需在 IBM Cloud Private 上增加 {{site.data.keyword.cnc_short}} 容量的相關資訊，請與 IBM 業務代表聯絡。

{{site.data.keyword.cnc_short}} 可在兩個維度上進行調整：_垂直_ 及_水平_。

 - _垂直_ 調整會增加剖析文件的傳輸量。
 - _水平_ 調整會增加剖析的並行文件數目。

這兩個調整選項都需要增加 {{site.data.keyword.cnc_short}} 所使用的「虛擬處理器核心 (VPC)」的數目。如需 VPC 的相關資訊，請參閱 IBM Cloud Private [授權文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window}。

垂直調整選項包括下列：

| 配置                             |大約傳輸量*         |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |每小時 1500 個頁面（預設值）   |
|`4Cores 20G 1 concurrent document (4 VPC)` |每小時 2500 個頁面             |
|`8Cores 40G 1 concurrent document (8 VPC)` |每小時 3000 個頁面             |

水平調整選項包括下列：

| 配置                             |大約傳輸量*         |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |每小時 3000 個頁面             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |每小時 5000 個頁面             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |每小時 6000 個頁面             |

\***附註**：傳輸量預估值係根據內部測試。正式作業環境中的傳輸量取決於多個因素，包括相同 IBM Cloud Private 叢集上的其他工作量、用戶端應用程式與 IBM Cloud Private 叢集之間的延遲，以及其他環境狀況。

若要新增或移除 {{site.data.keyword.cnc_short}} 叢集來變更部署配置，請執行下列步驟。

**附註**：如果您縮減 {{site.data.keyword.cnc_short}} 部署，則 IBM Cloud Private 會立即收回釋放的資源，因而減少部署的處理能力。請不要縮減正在使用的部署，尤其如果部署處於正式作業環境中，更是如此。
	
同樣地，如果您擴增 {{site.data.keyword.cnc_short}} 部署，則 IBM Cloud Private 會立即將所要求的資源套用至部署，假設資源可用。如果您需要的資源超過 IBM Cloud Private 安裝可以提供的資源，請與 IBM 業務代表談論如何增加 IBM Cloud Private 安裝的容量。

  1. 登入 IBM Cloud Private 叢集。

  1. 從左上角附近的功能表圖示中，選取**工作負載 -> 部署**。
  
  1. 主控台即會顯示**部署**頁面。在部署表格中，找出您要調整的部署。
  
  1. 在列出部署的表格列中，按一下表格最後一欄中的**動作**圖示，然後按一下**調整**。
  
  1. 主控台即會顯示**調整部署**視窗。在**實例**欄位中，輸入所需的實例數目，或使用上移鍵和下移鍵來選取所需的值。
  
  1. 按一下**調整部署**。
  
  1. **調整部署**視窗即會關閉，而主控台會回到顯示**部署**的頁面。尋找您的部署並使用**所需**及**現行**直欄，來驗證您所要求的實例數已配置給部署。
  
  1. 您可以選擇按一下**部署**頁面上的部署名稱，以檢視部署的詳細資料。
  
  

