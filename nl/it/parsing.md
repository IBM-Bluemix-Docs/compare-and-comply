---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-27"

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

# Descrizione dell'analisi del contratto
{: #contract_parsing}

{{site.data.keyword.cnc_short}} restituisce i contratti analizzati con un'analisi di ciascun elemento identificato.

Le seguenti sezioni descrivono in che modo il JSON restituito fornisce l'analisi.

## Tipi
{: #contract_types}

L'array `types` include una serie di oggetti, ognuno dei quali contiene le chiavi `nature` e `party` i cui valori identificano un distico per l'elemento. Le seguenti tabelle elencano i possibili valori delle chiavi `nature` e `party`.

Il valore `nature` elenca i tipi di azione richiesti dalla frase.

| `nature`         |Descrizione                                                |
|:----------------:|-----------------------------------------------------------|
|`Definition`      |Questo elemento aggiunge chiarezza per un termine, una relazione o simili. Non è richiesta alcuna azione per soddisfare l'elemento, né alcuna parte viene interessata.|
|`Disclaimer`      |La `parte` nell'elemento non è obbligata a soddisfare i termini specificati dall'elemento ma non le viene proibito di farlo.|
|`Exclusion`       |La `parte` nell'elemento non soddisferà i termini specificati dall'elemento.|
|`Obligation`      |La `parte` nell'elemento è obbligata a soddisfare i termini specificati dall'elemento.|
|`Right`           |La `parte` nell'elemento ha garanzia di ricevere i termini specificati dall'elemento.|
|`Recommendation`  |La `parte` nell'elemento non è obbligata, ma è fortemente invitata, a soddisfare i termini specificati dall'elemento.|

## Parti
{: #contract_parties}

L'array `parties` specifica i partecipanti elencati nel contratto. Ogni oggetto `party` identificato elenca la parte identificata in base al nome ed è abbinato a un oggetto `role` che classifica il ruolo dell'oggetto `party`. I valori di `role` che possono essere restituiti per i contratti includono:

| `role`           |Descrizione                                                |
|:----------------:|-----------------------------------------------------------|
|`Buyer`           |La parte responsabile del pagamento dei beni o dei servizi elencati nel contratto.|
|`End User`        |La parte che interagirà con i beni o servizi i forniti, esplicitamente distinta dal `Buyer`.|
|`None`            |Non è stata identificata alcuna parte per l'elemento. Questo valore è sempre associato alla natura di `Definition`.|
|`Supplier`        |La parte responsabile della fornitura dei beni o dei servizi elencati nel contratto.|

## Categorie
{: #contract_categories}

L'array `categories` definisce l'oggetto della frase. Le categorie attualmente supportate includono:

| `categories`     |Descrizione                                                |
|:----------------:|-----------------------------------------------------------|
|`Amendments`      |Elementi che specificano le modifiche al contratto dopo la firma o le modifiche a un contratto standard. Gli elementi relativi alle modifiche di un contratto e alle modifiche degli accordi rientrano anche in questa categoria.|
|`Asset Use`       |Elementi che si riferiscono a situazioni in cui una parte deve utilizzare le risorse (licenze o attrezzature) dell'altra parte nello svolgimento delle proprie funzioni in base all'accordo, incluse le autorizzazioni e le restrizioni in merito.|
|`Assignments`     |Elementi che descrivono il trasferimento di diritti, obblighi o entrambi a una terza parte.|
|`Audits`          |Questa categoria include elementi che si riferiscono alla conservazione delle registrazioni e al diritto delle parti di ispezionare i libri o i registri dell'altra parte e gli elementi che si riferiscono al diritto di una parte di ispezionare o verificare la conformità o ai requisiti che una parte sia disponibile per l'ispezione o la revisione della conformità.|
|`Business Continuity`|Una categoria ristretta per gli elementi che si riferiscono all'effetto di una vendita, di una fusione o di altre modifiche sostanziali a una delle parti dell'accordo.|
|`Communication`  |Elementi che si riferiscono ai requisiti di notifica o preavviso, dettagli sui metodi di comunicazione (l'atto o il processo di scambio di informazioni), i rappresentanti di contatto e le informazioni di contatto. Inoltre, elementi che si riferiscono a mezzi accettabili per lo scambio di informazioni tra le parti.|
|`Confidentiality` |Questa categoria include elementi che descrivono come le parti possono utilizzare le informazioni che apprendono nel corso del completamento del contratto, elementi che discutono sulla conservazione dei segreti commerciali ed elementi che descrivono la non divulgazione delle informazioni aziendali.|
|`Deliverables`    |Questa categoria include elementi che specificano l'articolo o gli articoli che una parte fornisce all'altra parte in cambio di denaro e gli elementi che descrivono la preparazione dei prodotti distribuibili.|
|`Delivery`        |Elementi che specificano i mezzi per trasferire l'articolo o gli articoli della categoria `Deliverables` da una parte all'altra. Ad esempio, se una parte sta vendendo servizi di accesso o cloud, i mezzi per ottenere tale accesso rientrano in questa categoria.|
|`Dispute Resolution`|Questa categoria comprende elementi che trattano le disposizioni per la risoluzione di eventuali controversie tra le parti contraenti; ad esempio, il regolamento tramite una procedura definita come un collegio arbitrale. Le controversie possono essere risolte con vari metodi diversi dall'arbitrato; ad esempio, danno irreparabile, ottenimento di un'ingiunzione o una dichiarazione dall'effetto di "Non perseguibile come class action". Esempi di controversie comprendono controversie di lavoro e controversie relative a fatture o fatturazione. La categoria comprende anche elementi che specificano la legge applicabile al contratto o la scelta della legge, come una particolare nazione o giurisdizione.|
|`Force Majeure`   |Elementi che si riferiscono a eventi dirompenti fuori dal controllo di una parte. Si tratta di una limitazione specifica della responsabilità.|
|`Indemnification` |Elementi che specificano la correzione di alcune responsabilità. L'indennizzo è quando una parte del contratto diventa responsabile del pagamento della responsabilità di un'altra parte. Il fatto di indennizzo implica che si è verificata una responsabilità.|
|`Insurance`       |Elementi che si riferiscono a qualsiasi tipo di copertura assicurativa o termini di copertura forniti da una parte all'altra parte.|
|`Intellectual Property`|Gli esempi tendono a rientrare in due categorie: quelle che usano definizioni formali di brevetti, diritti d'autore e marchi, tipicamente tratti dagli statuti e quelle che usano concetti più ampi di invenzioni, paternità e know-how. Le definizioni possono anche essere fornite per i diritti di proprietà intellettuale, come il diritto di agire e richiedere il risarcimento dei danni per violazione di tali diritti di proprietà. `Intellectual Property` include brevetti, diritti di richiesta di brevetti, marchi, nomi commerciali, marchi di servizio, nomi di dominio, diritti d'autore e tutte le applicazioni e registrazioni di tali schemi, modelli industriali, invenzioni, know-how, segreti commerciali, programmi software per computer a livello a mondiale e altre informazioni proprietarie intangibili. Per "Codice di terze parti", se una qualsiasi parte del codice appartiene a qualcun altro, è necessario richiamare l'IP.|
|`Liability`       |Elementi che descrivono il metodo per determinare quando e come l'errore è collegato a una qualsiasi parte.|
|`Payment Terms & Billing`|Una categoria ampia che include elementi che descrivono il modo in cui una parte paga o viene pagata, inclusi i seguenti elementi: elementi che si riferiscono alla modalità finale di pagamento in base al contratto o al modo in cui una parte sta pagando; elementi che specificano come e quando deve essere pagata la parte o il tipo di articoli che le parti pagheranno o fattureranno; elementi che si riferiscono a qualsiasi tipo di pagamento tariffario, ad eccezione di quelli che si riferiscono a prezzi o numeri specifici ed elementi che si riferiscono alla parte che riceve i pagamenti in base al contratto.|
|`Pricing & Taxes` |Questa categoria include elementi che descrivono ciò che una parte paga o per cosa viene pagata. La categoria comprende elementi che si riferiscono a imposte e importi specifici associati a singoli prodotti distribuibili che vengono scambiati, inclusi dettagli sui costi di un particolare elemento e gli elementi che descrivono cifre o metodi specifici per il calcolo dei prezzi.|
|`Privacy`         | Elementi che specificano il trattamento delle informazioni personali, ossia le informazioni private su uno o più specifici individui che devono essere protette. La categoria è un sottoinsieme molto specifico della più ampia categoria `Confidentiality`.|
|`Responsibilities`|Attività complementari all'accordo, su cui solo una delle parti ha supervisione e controllo. I tipi di elementi che rientrano in questa categoria variano a seconda della natura del contratto.|
|`Safety and Security`|Elementi che si riferiscono alla sicurezza fisica o alle protezioni di sicurezza informatica per dati e sistemi, nonché elementi che si riferiscono al miglioramento della sicurezza del luogo di lavoro.|
|`Scope of Work`   |Questa categoria definisce ciò che è presente nel contratto rispetto a ciò che non è nel contratto. In genere, gli elementi che indicano alle parti in che modo deve essere definito un determinato ordine rientrano in questa categoria. Alcuni esempi includono discussioni sulle "dichiarazioni di lavoro" o descrizioni degli standard di comunicazione applicabili.|
|`Subcontracts`    |Elementi che si riferiscono alle assunzioni di terzi per adempiere a determinate mansioni previste dal contratto, nonché i permessi, i diritti, le restrizioni e le conseguenze che ne derivano.|
|`Term & Termination`|Elementi che si riferiscono alla durata del contratto, alla pianificazione e ai termini di risoluzione del contratto e tutte le conseguenze della risoluzione, compresi eventuali obblighi applicabili al termine o dopo la risoluzione.|
|`Warranties`      |Elementi che si riferiscono specificamente alle ipotesi di fondo su cui le parti possono fare affidamento. In questa categoria rientrano anche le conseguenze della violazione delle garanzie.|

## Attributi
{: #attributes}

L'array `attributes` specifica gli attributi identificati nella frase. Ogni oggetto nell'array include tre chiavi: `type` (il tipo di attributo dalla seguente tabella), `text` (il testo applicabile) e `attribute` (i punti di inizio e di fine dell'attributo nel documento). Gli attributi attualmente supportati includono:

| `attributes`     |Descrizione                                                |
|:----------------:|-----------------------------------------------------------|
|`Location`        |Una posizione geografica o regione.                         |
|`DateTime`        |Una data, un'ora, un intervallo di date o un intervallo di ore.                   |
|`Currency`        |Valore monetario e unità.                                  |

# Garanzia
{: #assurance}

{{site.data.keyword.cnc_short}} fornisce una valutazione di affidabilità per ciascun elemento `type` o `category` che identifica. Attualmente esiste un valore di garanzia, `High`, che indica che ci sono prove significative che la classificazione elencata è rappresentativa del contenuto.

## Provenienza
{: #provenance}

Ogni oggetto presente negli array `types` e `categories` include un oggetto `provenance`. L'oggetto `provenance` ha una o più chiavi `id`. Ogni chiave `id` ha un valore di hash che puoi inviare a IBM per fornire un feedback o ricevere supporto.

