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

# デプロイメントの構成について
{: #configs}

{{site.data.keyword.cnc_short}} のデフォルトのデプロイメントの構成は、`2Cores 10G 1 concurrent document (2 VPC)` です。この構成では、1 時間あたり約 1500 ページを処理できます。スループットの要件に応じて、IBM Cloud Private 上の {{site.data.keyword.cnc_short}} デプロイメントをさまざまなレベルに拡大できます。IBM Cloud Private 上の {{site.data.keyword.cnc_short}} の容量を増やす方法について詳しくは、IBM 営業担当員にお問い合わせください。

{{site.data.keyword.cnc_short}} は、_垂直_ と_水平_ の 2 つの次元でスケーリングできます。

 - _垂直_ スケーリングを行うと、1 つの文書の解析時のスループットが増えます。
 - _水平_ スケーリングを行うと、同時に解析される文書の数が増えます。

両方のスケーリング・オプションとも、{{site.data.keyword.cnc_short}} で使用される仮想プロセッサー・コア (VPC) の数を増やす必要があります。VPC について詳しくは、IBM Cloud Private の[使用許諾の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window} を参照してください。

垂直スケーリングのオプションには、以下のものがあります。

| 構成                                      |スループットの概算*             |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |1 時間あたり 1500 ページ (デフォルト)|
|`4Cores 20G 1 concurrent document (4 VPC)` |1 時間あたり 2500 ページ             |
|`8Cores 40G 1 concurrent document (8 VPC)` |1 時間あたり 3000 ページ             |

水平スケーリングのオプションには、以下のものがあります。

| 構成                                        |スループットの概算*             |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |1 時間あたり 3000 ページ             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |1 時間あたり 5000 ページ             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |1 時間あたり 6000 ページ             |

\***注**: スループットの見積もりは、社内テストに基づいています。実稼働環境でのスループットは、同じ IBM Cloud Private クラスター上の他のワークロード、クライアント・アプリケーションと IBM Cloud Private クラスターの間の待ち時間、その他の環境条件などの複数の要因に応じて異なります。

{{site.data.keyword.cnc_short}} クラスターを追加したり削除したりしてデプロイメントの構成を変更するには、以下のステップを実行します。

**注**: {{site.data.keyword.cnc_short}} デプロイメントを縮小すると、解放されたリソースが IBM Cloud Private で即時に再利用されるので、そのデプロイメントの処理能力が低下します。使用中のデプロイメント、特に実稼働環境で使用しているデプロイメントでは、縮小しないでください。
	
同様に、{{site.data.keyword.cnc_short}} デプロイメントを拡大すると、IBM Cloud Private では要求されたリソースが使用可能であると想定し、リソースをそのデプロイメントに即時に適用します。IBM Cloud Private インストール済み環境で提供できる量より多くのリソースが必要な場合は、IBM Cloud Private インストール済み環境の容量を増やす方法について IBM 営業担当員に相談してください。

  1. IBM Cloud Private クラスターにログインします。

  1. 左上隅付近のメニュー・アイコンから、**「ワークロード」->「デプロイメント」**を選択します。
  
  1. コンソールに**「デプロイメント」**ページが表示されます。デプロイメントの表で、スケーリングしようとしているデプロイメントを見つけます。
  
  1. 表の、当該デプロイメントがリストされている行で、末尾の列の**「アクション」**アイコンをクリックしてから、**「スケーリング」**をクリックします。
  
  1. コンソールに**「デプロイメントのスケーリング」**ウィンドウが表示されます。**「インスタンス」**フィールドで、ご希望のインスタンスの数を入力するか、上矢印と下矢印を使用してご希望の値を選択します。
  
  1. **「デプロイメントのスケーリング」**をクリックします。
  
  1. **「デプロイメントのスケーリング」**ウィンドウが閉じ、コンソールが**「デプロイメント」**ページの表示に戻ります。ご使用のデプロイメントを見つけ、**「必要数」**と**「現在」**の列を使用して、要求した数のインスタンスがデプロイメントに割り振られていることを確認します。
  
  1. オプションで、**「デプロイメント」**ページ上のデプロイメントの名前をクリックして、そのデプロイメントの詳細を表示します。
  
  

