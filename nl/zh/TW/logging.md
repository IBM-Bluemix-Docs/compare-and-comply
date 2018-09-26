---

copyright:
years: 2017, 2018
lastupdated: "2018-07-25"

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

# 使用記載
{: #logging}

## 匯入記載儀表板

若要將 {{site.data.keyword.cnc_short}} 的記載儀表板匯入至 IBM Cloud Private，請執行下列步驟。

  1. 確定您已擷取並產生記載儀表板，如[步驟 1：下載、擷取及呈現儀表板範本](/docs/services/compare-and-comply/monitor.html#monitor)中所述。

  1. 登入 IBM Cloud Private 叢集。

  1. 從左上角的「功能表」圖示中，選取**平台 -> 記載**。<br />
    ![「IBM Cloud Private 功能表」圖示](images/icp-menu.png) <br />
    ![「平台 -> 記載」功能表](images/icp-logging.png)

  1. 按一下 Kibana 介面左側的**管理**。<br />
    ![Kibana 介面](images/kibana.png)

  1. 選取**儲存的物件**標籤。
    ![「儲存的物件」標籤](images/saved-obj.png)

  1. 選取**搜尋**標籤，然後按一下**匯入**。
    ![「從搜尋中匯入」標籤](images/searches-import.png)

  1. 個別匯入 `frontend - logging.json` 和 `exnal-process-logging.json` 檔案，這些檔案是在上述程序的「步驟 6」中產生的。系統提示時，請按一下**是，全部改寫**。
     ![「是，全部改寫」提示](images/overwrite-all.png)

  1. 儀表板即會出現在**搜尋**標籤中。
     ![「搜尋中的儀表板」標籤](images/searches-tab.png)

## 檢視記載儀表板
{: #view}

若要檢視記載儀表板，請執行下列步驟。

  1. 導覽至**探索**標籤。

  1. 按一下 Kibana 介面右上角附近的**開啟**。

  1. 選取您要檢視的儀表板。有兩個適用於服務日誌及外部程序日誌的記載儀表板。
    ![檢視記載儀表板](images/kibana-dboards.png)

您可以輕鬆地變更時間範圍及自動重新整理的頻率：
  ![變更時間範圍及重新整理頻率](images/log-dboard-change.png)

