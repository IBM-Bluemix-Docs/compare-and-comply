---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-02"

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

# Einführung
{: #getting_started}

In diesem kurzen Lernprogramm wird {{site.data.keyword.cnc_long}} auf IBM Cloud Private vorgestellt. Außerdem wird erläutert, wie ein Vertrag geparst wird, um Komponententeile, ihre Gattung, die betroffenen Parteien und alle angegebenen Kategorien zu ermitteln.

## Vorbereitende Schritte
{: #before-you-begin}

Bevor Sie den {{site.data.keyword.cnc_short}}-Service verwenden können, müssen Sie die Befehlszeilenschnittstelle (CLI) für IBM Cloud Private installieren und sich bei Ihrem IBM Cloud Private-Cluster wie in [IBM Cloud Private-CLI installieren](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html) beschrieben anmelden.
 
## Schritt 1: Inhalt angeben
{: #identify_content}

Geben Sie geeignete Dokument zum Analysieren an. {{site.data.keyword.cnc_short}} wurde konzipiert, um Vertragsdokumente <!-- and regulatory -->zu analysieren, die den folgenden Kriterien entsprechen:

- Die Dateien, die analysiert werden sollen, liegen in PDF-Format vor.
- Der PDF-Inhalt liegt in Textformat vor. Gescannte Dokumente können nicht geparst werden, selbst wenn die Scans mit einem optischen Zeichenleser (OCR) verarbeitet wurden.

  **Hinweis:** Sie können ermitteln, ob es sich um eine PDF-Datei in Textformat handelt, indem Sie das Dokument in einem PDF-Viewer öffnen und dann ein einzelnes Wort mit dem Tool für die **Textauswahl** auswählen. Sollte es nicht möglich sein, im Dokument ein einzelnes Wort auszuwählen, kann die Datei nicht geparst werden.

- Dateien haben eine Größe von maximal 50 MB.
- Für sichere PDF-Dateien, bei denen zum Öffnen ein Kennwort erforderlich ist, und für kennwortgeschützte PDF-Dateien, die nur mit einem Kennwort bearbeitet werden können, kann kein Parsing durchgeführt werden.

## Schritt 2: Parsing für einen Vertrag durchführen
{: #parse_contract}

Verwenden Sie in einer `Bash`-Shell oder einer funktional entsprechenden Umgebung wie Cygwin die Methode `POST /v1/parse`, um den Vertrag zu parsen. Ersetzen Sie dabei `{ICP-IP-adresse}` durch die IP-Adresse für Ihren IBM Cloud Private-Cluster. Ersetzen Sie `{PDF-datei}` durch den Pfad zu der PDF-Datei, die geparst werden soll.

```bash
curl -k -X POST -F 'file=@{PDF-datei};type=application/pdf' https://{ICP-IP-adresse}/api/v1/parse?version=2018-03-23
```
{: pre}

**Wichtig:** Wenn Sie {{site.data.keyword.cnc_short}} in einer älteren Version als 1.0.4 ausführen, müssen Sie die Kennung `:{portnummer}` wie folgt mit der IP-Adresse des IBM Cloud Private-Clusters angeben, wenn Sie Aufrufe an den Service durchführen:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP-IP-adresse}:{portnummer}/api/v1/parse?version=2018-03-23
```
{: pre}

Beispiel:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

Detaillierte Informationen hierzu enthalten die [Releaseinformationen zu **Ingress**](/docs/services/compare-and-comply/relnotes.html#ingress).

Die Methode gibt ein JSON-Objekt mit folgendem Inhalt zurück:

 - HTML-Konvertierung der PDF-Quellendatei.
 - Extrahierter Dokumenttitel.
 - Array von `elements`, die die einzelnen Komponenten des Vertrags definieren.
 - Array von `parties`, die Entitäten definieren, die als Vertragspartner angegeben wurden.
 
**Wichtig**: Die API von {{site.data.keyword.cnc_short}} auf IBM Cloud Private erfordert keine Berechtigung, wie es bei APIs auf der öffentlichen IBM Cloud der Fall ist.

## Schritt 3: Analyse überprüfen
{: #review_analysis}

Jedes Objekt im `elements`-Array beschreibt ein Element des Vertrags, das von {{site.data.keyword.cnc_short}} ermittelt wurde. Folgender Code stellt ein typisches Element dar:

```
{
  "sentence_text": "If the parties are unable to agree on the License Price within thirty (30) days after Richmond Enterprises provides the written notice of its exercise of the Optional Patent License to Savage Narwhal Studios in Section 2.3(a), such FMV Dispute shall, at either party's request, be resolved solely and exclusively by final, binding and confidential arbitration to be filed and the decision rendered in New York, New York (with hearings at the request of either party to be held in San Francisco, California or other mutually agreeable place convenient for the parties) in accordance with the Commercial Arbitration Rules of the American Arbitration Association (\" AAA \"), including as supplemented by the Procedures for Large, Complex Commercial Disputes.",
  "attributes": [
	{
      "type": "Location",
		"text": "New York",
		"attribute": {
        "begin": 58372,
			"end": 58380
		}
    },
    {
      "type": "Location",
		"text": "New York",
		"attribute": {
        "begin": 58382,
			"end": 58390
		}
    },
    {
      "type": "Location",
		"text": "San Francisco",
		"attribute": {
        "begin": 58451,
			"end": 58464
		}
    },
    {
      "type": "Location",
		"text": "California",
		"attribute": {
        "begin": 58466,
			"end": 58476
		}
    }
  ],
"categories": [
	{
      "label": "Communication",
		"assurance": "High",
		"provenance": [
			{
          "id": "C7xhbsepUodh09zmJdUXSvYZCdixx00wFyCZuAnTujok="
			}
      ]
    },
    {
      "label": "Dispute Resolution",
		"assurance": "High",
		"provenance": [
			{
          "id": "Ck8vgUWOj41OutOOLJ38b2Q7jOj3F30ABGaGLKKxppFA="
        },
        {
          "id": "CdPeg8mAxM5YIsdpzzaluDg7jOj3F30ABGaGLKKxppFA="
			}
      ]
    },
    {
      "label": "Intellectual Property",
		"assurance": "High",
		"provenance": [
			{
          "id": "Cor/mgcf1UE/zmsKm68M6+a9LSRCpcKe8EWCUdwsjrgs="
			}
      ]
    }
  ],
"types": [
	{
      "label": {
        "nature": "Obligation",
			"party": "All Parties"
      },
		"assurance": "High",
		"provenance": [
			{
          "id": "NNpSqaNkY2zHtxI6Zh78NaZccVNtutrQxISkzdnaVjx0="
        },
        {
          "id": "PlyERkjg5is36RpFjVUFXp69eDmGmCxLCXRs1sDMDUCo="
			}
      ]
    }
  ],
"sentence": {
    "begin": 57998,
	"end": 58952
  }
}
```

Das Element umfasst fünf wichtige Abschnitte:
  - `sentence_text`: Der Text, der analysiert wurde.
  - `attributes`: Ein Array, das ein oder mehr Attribute für das Element auflistet. Gegenwärtig werden vom Array `attributes` die Elemente `Location` (vom Element referenzierter geografische/r Standort oder Region), `DateTime` (vom Element angegebene/r/s Datum, Zeit, Datumsbereich oder Zeitraum) und `Currency` (Geldwerte und Einheiten) unterstützt. 
  - `categories`: Ein Array, das die funktionalen Kategorien auflistet, in die der erkannte Satz fällt, anders ausgedrückt also das Thema oder der Gegenstand des Satzes.
  - `types`: Ein Array, das beschreibt, was das Element ist und worauf es sich auswirkt. Es besteht aus einem oder mehreren Sätzen von `nature`-Schlüsseln (d. h. der Auswirkung des Satzes auf die angegebene `party`) und `party`-Schlüsseln (d. h. den vom Satz Betroffenen).
  - `sentence`: Ein Objekt, das beschreibt, wo das Element im konvertierten HTML-Dokument gefunden wurde. Es enthält einen Zeichenwert `start` und einen Zeichenwert `end`.

**Hinweis**: Manche Sätze gehören keinem Typ bzw. keiner Kategorie an. In diesem Fall gibt der Service die Arrays `types` und `categories` als leere Objekte zurück.

**Hinweis:** Manche Sätze decken mehrere Themen ab. In diesem Fall gibt der Service mehrere Sätze der Objekte `types` und `categories` zurück.

**Hinweis**: Manche Sätze enthalten keine erkennbaren Attribute. In diesem Fall gibt der Service das Array `attributes` als leere Objekte zurück.

Zusätzlich werden alle ermittelten Vertragspartner im Array `parties` definiert. Das Array `parties` befindet sich in der JSON-Ausgabe unter dem Array `elements`.

```json
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

Das Array `parties` umfasst zwei wichtige Abschnitte:

  - `party`: Der Text, der im Dokument als Vertragspartner ermittelt wurde.
  - `role`: Die Rolle des erkannten Vertragspartners. Die Rollen wechseln je nach Unterdomäne. Eine Liste der möglichen Rollen enthält die [Dokumentation zur angegebenen Unterdomäne](/docs/services/compare-and-comply/parsing.html#contract_parties). Vertragspartner, für die keine bestimmte Rolle erkannt werden kann, werden mit dem Wert `unknown` aufgelistet.

## Nächste Schritte
{: #next_steps}

Sie haben erfolgreich einen Vertrag geparst, um die Gattung, Vertragspartner und Kategorien der einzelnen Komponenten des Dokuments zu ermitteln. Anhand der Analyse können Sie den geparsten Vertrag rasch verstehen und umsetzen. Als Nächstes sind die folgenden Schritte erforderlich:

 - [Parsing von Verträgen verstehen](/docs/services/compare-and-comply/parsing.html#contract_parsing)
 - [Informationen zum Ausgabeschema](/docs/services/compare-and-comply/schema.html#output_schema) und zur [Tabellenanalyse](/docs/services/compare-and-comply/tables.html#understanding_tables).


