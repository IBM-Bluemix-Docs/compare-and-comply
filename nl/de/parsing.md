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

# Parsing von Verträgen verstehen
{: #contract_parsing}

{{site.data.keyword.cnc_short}} gibt geparste Verträge mit einer Analyse jedes erkannten Elements zurück.

In den folgenden Abschnitten wird beschrieben, wie die zurückgegebene JSON die Analyse bereitstellt.

## Types
{: #contract_types}

Das Array `types` umfasst eine Reihe von Objekten, von denen jedes Schlüssel vom Typ `nature` und `party` enthält, deren Werte ein Paar für das Element angeben. In den folgenden Tabellen sind die gültigen Werte für die Schlüssel `nature` und `party` aufgeführt.

Der Wert für `nature` listet die Arten von Maßnahmen auf, die der Satz erfordert.

| `nature`         |Beschreibung                                               |
|:----------------:|-----------------------------------------------------------|
|`Definition`      |Dieses Element sorgt bei einem Begriff, einer Beziehung oder Ähnlichem für mehr Klarheit. Es sind keine Maßnahmen erforderlich, um das Element zu erfüllen, und es ist auch keine der Parteien betroffen.|
|`Disclaimer`      |Die im Element mit `party` genannte Partei ist nicht verpflichtet, die durch das Element angegebenen Bedingungen zu erfüllen, doch ist es ihr auch nicht untersagt.|
|`Exclusion`       |Die im Element mit `party` genannte Partei wird die durch das Element angegebenen Bedingungen nicht erfüllen.|
|`Obligation`      |Die im Element mit `party` genannte Partei ist verpflichtet, die durch das Element angegebenen Bedingungen erfüllen.|
|`Right`           |Der im Element mit `party` genannten Partei werden die durch das Element angegebenen Bedingungen garantiert zugesichert.|
|`Recommendation`  |Die im Element mit `party` genannte Partei ist nicht verpflichtet, die durch das Element angegebenen Bedingungen zu erfüllen, doch die Erfüllung dieser Bedingungen wird dringend empfohlen.|

## Parties
{: #contract_parties}

Das Array `parties` gibt die im Vertrag aufgeführten Teilnehmer an. Jedes erkannte Objekt vom Typ `party` listet die erkannte Partei namentlich auf und wird mit einem Objekt vom Typ `role` abgeglichen, das die Rolle des Objekts `party` klassifiziert. Bei Verträgen können für Objekte vom Typ `role` die folgenden Werte zurückgegeben werden:

| `role`           |Beschreibung                                               |
|:----------------:|-----------------------------------------------------------|
|`Buyer`           |Die Partei, die für die Bezahlung der im Vertrag aufgeführten Waren oder Services verantwortlich ist.|
|`End User`        |Die Partei, die mit den gelieferten Waren oder Services interagieren wird und sich explizit von der Rolle `Buyer` unterscheidet.|
|`None`            |Für das Element wurde keine Partei ermittelt. Dieser Wert wird stets mit der Gattung `Definition` paarweise kombiniert.|
|`Supplier`        |Die Partei, die für die Bereitstellung der im Vertrag aufgeführten Waren oder Services verantwortlich ist.|

## Categories
{: #contract_categories}

Das Array `categories` definiert das Thema bzw. den Gegenstand des Satzes. Gegenwärtig werden die folgenden Kategorien unterstützt:

| `categories`     |Beschreibung                                               |
|:----------------:|-----------------------------------------------------------|
|`Amendments`      |Elemente, die Änderungen am Vertrag nach seiner Unterzeichnung oder Änderungen an einem Standardvertrag angeben. Elemente, die sich auf Änderungen eines Vertrags und Änderungen von Vereinbarungen beziehen, gehören ebenfalls zu dieser Kategorie.|
|`Asset Use`       |Elemente, die sich auf Situationen beziehen, in denen eine Partei bei der Erfüllung ihrer Pflichten im Rahmen der Vereinbarung die Assets (Lizenzen oder Geräte) der anderen Partei einschließlich diesbezüglicher Genehmigungen und Einschränkungen verwenden muss.|
|`Assignments`     |Elemente, die die Übertragung von Rechten, von Pflichten oder von Beidem auf einen Dritten beschreiben.|
|`Audits`          |Diese Kategorie umfasst Elemente, die sich auf die Datenhaltung und das Recht der Parteien auf Einsichtnahme in die Geschäftsbücher oder auf Akteneinsicht der anderen Partei beziehen, sowie Elemente, die sich entweder auf das Recht einer Partei zur Untersuchung oder Überprüfung der Compliance beziehen, oder auf Anforderungen, dass sich eine Partei zur Untersuchung oder Überprüfung der Compliance zur Verfügung stellen muss.|
|`Business Continuity`|Eine eng umschriebene Kategorie für Elemente, die auf die Auswirkungen durch den Verkauf, die Fusion oder eine andere wesentliche Änderung bei einer der Parteien der Vereinbarung verweisen.|
|`Communication`  |Elemente, die sich auf Anforderungen zur Benachrichtigung oder Inkenntnissetzung, Details zu Kommunikationsmethoden (d. h. den Informationsaustausch selbst oder den Prozess des Informationsaustauschs), Ansprechpartner und Kontaktinformationen beziehen. Außerdem sind auch Elemente enthalten, die sich auf zulässige Mittel zum Austausch von Informationen zwischen den Parteien beziehen.|
|`Confidentiality` |Diese Kategorie enthält Elemente, die beschreiben, wie die Parteien die Informationen nutzen können, die sie im Zuge der Vertragsabwicklung erhalten, Elemente, die die Bewahrung von Geschäftsgeheimnissen erörtern und Elemente, die die Geheimhaltung von Geschäftsinformationen beschreiben.|
|`Deliverables`    |Diese Kategorie enthält Elemente, die den Gegenstand oder die Gegenstände beschreiben, die eine Partei der Gegenpartei im Gegenzug für Geld bietet, sowie Elemente, die die Vorbereitung der Liefergegenstände beschreiben.|
|`Delivery`        |Diese Kategorie enthält Elemente, die angeben, wie der Gegenstand oder die Gegenstände in der Kategorie `Deliverables` von einer Partei an eine andere zu übertragen sind. Wenn eine Partei zum Beispiel Zugriffsservices oder Cloud-Services verkauft, zählt die Methode zum Erhalten dieses Zugriffs zu dieser Kategorie.|
|`Dispute Resolution`|Diese Kategorie enthält Elemente, die Bestimmungen zur Beilegung von Streitigkeiten zwischen Vertragsparteien erörtern, beispielsweise die Beilegung durch ein festgelegtes Verfahren wie etwa eine Schiedsinstanz. Streitigkeiten können auf mehrere andere Arten als Schiedsverfahren beigelegt werden, nicht wieder gutzumachender Schaden zum Beispiel durch Erwirken einer einstweiligen Verfügung oder eine Erklärung mit der Kernaussage „Hiergegen dürfen Sie keine Sammelklage erheben“. Beispiele für Streitigkeiten umfassen Arbeitskonflikte und Meinungsverschiedenheiten bei Rechnungen oder der Rechnungsstellung. Ferner enthält die Kategorie auch Elemente, die das geltende Recht oder die Rechtswahl des Vertrags angeben, wie z. B. ein bestimmtes Land oder eine bestimmte Gerichtsbarkeit.|
|`Force Majeure`   |Elemente, die sich auf störende Ereignisse jenseits der Einflussnahme einer Partei beziehen. Hierbei handelt es sich um eine spezifische Haftungsbeschränkung.|
|`Indemnification` |Elemente, die die Behebung gewisser Verbindlichkeiten angeben. Bei einer Entschädigung ist eine Partei des Vertrages für die Bezahlung des Haftungsanspruchs einer anderen Partei verantwortlich. Die Tatsache einer Entschädigung bedeutet, dass ein Haftungsanspruch entstanden ist.|
|`Insurance`       |Elemente, die sich auf jegliche Art von Versicherungsschutz oder Versicherungsbedingungen beziehen, die eine Partei der anderen Partei bietet.|
|`Intellectual Property`|Beispiele lassen sich tendenziell zumeist in zwei Kategorien einteilen: solche, die formale Definitionen in Form von Patenten, Urheberrechten und Marken verwenden, die normalerweise Gesetzesbestimmungen entnommen sind, und solche, die allgemeiner gefasste Konzepte in Form von Erfindungen, Autorenschaft und Know-how verwenden. Definitionen können auch für Rechte an geistigem Eigentum angegeben werden, wie das Recht, Klage zu erheben und bei Verletzung solcher Eigentumsrechte Schadenersatzforderungen geltend zu machen. `Intellectual Property` umfasst Patente, die Rechte zur Beantragung von Patenten, Marken, Handelsnamen, Servicemarken, Domänennamen, Urheberrechte und weltweit alle Anmeldungen und Registrierungen dergleichen, Schemata, Industriemodelle, Erfindungen, Know-how, Geschäftsgeheimnisse, Computersoftwareprogramme und andere immaterielle urheberrechtlich geschützte Informationen. Bei Code anderer Anbieter (Third Party Code) muss, falls irgendein Teil des Codes jemand anders gehört, die IP nannt werden.|
|`Liability`       |Elemente, die beschreiben, mit welcher Methode festgestellt wird, wann und wie eine Partei ein Verschulden trifft.|
|`Payment Terms & Billing`|Eine breit gefasste Kategorie, die Elemente enthält, die beschreiben, wie eine Partei Zahlungen leistet oder erhält, einschließlich Folgendem: Elemente, die sich auf die endgültige Zahlungsart im Rahmen des Vertrags beziehen oder darauf, wie eine Partei zahlt, Elemente, die im Detail angeben, wann und wie die Partei Zahlungen zu erhalten hat oder welche Arten von Gegenständen die Parteien bezahlen oder in Rechnung stellen werden, Elemente, die sich auf jegliche Art der Zahlung von Gebühren beziehen, doch mit Ausnahme solcher, die sich auf spezifische Preise oder Zahlen beziehen, und Elemente, die sich auf die Partei beziehen, die die Zahlungen gemäß dem Vertrag erhält.|
|`Pricing & Taxes` |Diese Kategorie enthält Elemente, die beschreiben, welche Bezahlung eine Partei erhält oder zu leisten hat. Die Kategorie umfasst Elemente, die sich auf Steuern und spezifische Beträge beziehen, die mit einzelnen erbrachten Liefergegenständen verbunden sind, die ausgetauscht werden, und schließt Detailangaben zu den Kosten eines bestimmten Elements sowie Elemente ein, die spezifische Zahlen oder Methoden zur Berechnung von Preisen beschreiben.|
|`Privacy`         |Elemente, die die Behandlung von personenbezogenen Informationen angeben, d. h. von vertraulichen Daten zu einer oder mehreren Einzelpersonen, die geschützt werden müssen. Diese Kategorie ist eine sehr spezifische Untergruppe der breiter gefassten Kategorie `Confidentiality`.|
|`Responsibilities`|Die Vertragsvereinbarung ergänzende Tasks, über die nur eine der Parteien die Aufsicht und die Kontrolle hat. Welche Arten von Elementen diese Kategorie umfasst, hängt jeweils von der Art des Vertrags ab.|
|`Safety and Security`|Elemente, die sich auf physische Sicherheitsmaßnahmen oder auf Schutzeinrichtungen zur Wahrung der Cybersicherheit von Daten und Systemen beziehen, sowie Elemente, die sich auf die Verbesserung der Sicherheit am Arbeitsplatz oder an den Arbeitsplätzen beziehen.|
|`Scope of Work`   |Diese Kategorie definiert, was im Vertrag steht und was nicht im Vertrag behandelt wird. Normalerweise gehören Elemente, die den Parteien mitteilen, wie ein bestimmter Auftrag definiert werden soll, in diese Kategorie. Beispiele hierfür umfassen Erörterungen von Leistungsbeschreibungen oder Beschreibungen der anzuwendenden Kommunikationsstandards.|
|`Subcontracts`    |Elemente, die sich auf die Beauftragung von anderen Anbietern beziehen, um bestimmte Pflichten im Rahmen des Vertrags zu erfüllen, und die diesbezüglichen sowie daraus hervorgehenden Berechtigungen, Rechte, Einschränkungen und Konsequenzen.|
|`Term & Termination`|Elemente, die sich auf die Dauer des Vertrags, den Zeitplan und die Bedingungen der Vertragsbeendigung beziehen, sowie jegliche Folgen der Beendigung einschließlich aller Verpflichtungen, die bei oder nach der Beendigung zutreffen.|
|`Warranties`      |Elemente, die sich spezifisch auf den Hintergrund beziehen, sowie zugrunde liegende Annahmen, auf die sich die Parteien verlassen können. Zu dieser Kategorie gehören auch die Folgen von Verletzungen der Gewährleistung.|

## Attributes
{: #attributes}

Das Array `attributes` gibt alle im Satz erkannten Attribute an. Jedes Objekt im Array enthält drei Schlüssel: `type` (der Typ des Attributs aus der folgenden Tabelle), `text` (der zutreffende Text) und `attribute` (Anfangs- und Endpunkt des Attributs im Dokument). Gegenwärtig werden die folgenden Attribute unterstützt:

| `attributes`     |Beschreibung                                               |
|:----------------:|-----------------------------------------------------------|
|`Location`        |Ein Standort oder eine Region.                             |
|`DateTime`        |Ein Datum, eine Zeit, ein Datumsbereich oder ein Zeitraum. |
|`Currency`        |Geldwert und Einheiten.                                    |

# Assurance
{: #assurance}

{{site.data.keyword.cnc_short}} erteilt jedem von ihm erkannten Element vom Typ `type` oder `category` eine Verlässlichkeitsbewertung. Gegenwärtig gibt es lediglich einen Verlässlichkeitswert, nämlich `High`, der anzeigt, dass es deutliche Hinweise dafür gibt, dass die aufgelistete Klassifizierung den Inhalt repräsentiert.

## Provenance
{: #provenance}

Jedes Objekt in den Arrays `types` und `categories` enthält ein Objekt vom Typ `provenance`. Das Objekt `provenance` verfügt über einen oder mehrere Schlüssel vom Typ `id`. Jeder `id`-Schlüssel besitzt einen Hashwert, den Sie an IBM senden können, um Feedback zu liefern oder Unterstützung zu erhalten.

