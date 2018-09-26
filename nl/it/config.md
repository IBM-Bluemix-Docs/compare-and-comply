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

# Descrizione delle configurazioni di distribuzione
{: #configs}

La configurazione di distribuzione predefinita per {{site.data.keyword.cnc_short}} è `2Cores 10G 1 concurrent document (2 VPC)`. Questa configurazione può elaborare circa 1500 pagine all'ora. Puoi ridimensionare la tua distribuzione di {{site.data.keyword.cnc_short}} su IBM Cloud Private a diversi livelli a seconda dei tuoi requisiti di velocità effettiva. Contatta il tuo rappresentante delle vendite IBM per ulteriori informazioni sull'aumento della capacità di {{site.data.keyword.cnc_short}} su IBM Cloud Private.

{{site.data.keyword.cnc_short}} può essere ridimensionato su due dimensioni: _verticale_ e _orizzontale_.

 - Il ridimensionamento _verticale_ aumenta la velocità effettiva con cui viene analizzato un documento.
 - Il ridimensionamento _orizzontale_ aumenta il numero di documenti simultanei analizzati.

Entrambe le opzioni di ridimensionamento richiedono l'aumento del numero di VPC (Virtual Processor Core) utilizzati da {{site.data.keyword.cnc_short}}. Per informazioni sui VPC, consulta la [documentazione relativa alla licenza ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window} di IBM Cloud Private.

Le opzioni di ridimensionamento verticale includono quanto segue:

| Configurazione                             |Velocità approssimativa*         |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |1500 pagine all'ora (impostazione predefinita)   |
|`4Cores 20G 1 concurrent document (4 VPC)` |2500 pagine all'ora             |
|`8Cores 40G 1 concurrent document (8 VPC)` |3000 pagine all'ora             |

Le opzioni di ridimensionamento orizzontale includono quanto segue:

| Configurazione                               |Velocità approssimativa*         |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |3000 pagine all'ora             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |5000 pagine all'ora             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |6000 pagine all'ora             |

\***Nota**: le stime della velocità effettiva sono basate su test interni. La velocità effettiva in un ambiente di produzione dipende da più fattori, che includono altri carichi di lavoro sullo stesso cluster IBM Cloud Private, le latenze tra l'applicazione client e il cluster IBM Cloud Private e altre condizioni ambientali.

Per modificare la configurazione di distribuzione aggiungendo o rimuovendo i cluster {{site.data.keyword.cnc_short}}, completa la seguente procedura.

**Note**: se riduci una distribuzione di {{site.data.keyword.cnc_short}}, IBM Cloud Private recupera immediatamente le risorse rilasciate, riducendo così la capacità di elaborazione della distribuzione. Non ridurre una distribuzione che viene utilizzata, in particolare se la distribuzione si trova in un ambiente di produzione.
	
Allo stesso modo, se aumenti una distribuzione di {{site.data.keyword.cnc_short}}, IBM Cloud Private applica immediatamente le risorse richieste alla distribuzione, supponendo che le risorse siano disponibili. Se hai bisogno di più risorse rispetto a quelle fornite dall'installazione di IBM Cloud Private, contatta il tuo rappresentante delle vendite IBM in merito all'aumento della capacità della tua installazione di IBM Cloud Private.

  1. Accedi al tuo cluster IBM Cloud Private.

  1. Dall'icona di menu accanto all'angolo superiore a sinistra, seleziona **Workloads -> Deployments**.
  
  1. La console visualizza la pagina **Deployments**. Individua la distribuzione che vuoi ridimensionare nella tabella delle distribuzioni.
  
  1. Nella riga della tabella che elenca la tua distribuzione, fai clic sull'icona **Action** nell'ultima colonna della tabella, quindi fai clic su **Scale**.
  
  1. La console visualizza la finestra **Scale Deployment**. Nel campo **Instances**, immetti il numero di istanze desiderato o utilizza le frecce verso l'alto e il basso per selezionare un valore.
  
  1. Fai clic su **Scale Deployment**.
  
  1. La finestra **Scale Deployment** si chiude e la console torna a visualizzare la pagina **Deployments**. Trova la tua distribuzione e utilizza le colonne **Desired** e **Current** per verificare che alla distribuzione sia stato assegnato il numero di istanze da te richiesto.
  
  1. Facoltativamente, fai clic sul nome della distribuzione nella pagina **Deployments** per visualizzare i dettagli della distribuzione.
  
  

