
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

# Releaseinformationen
{: #release_notes}

Die Releaseinformationen enthalten Informationen zu Änderungen am Release des Service '{{site.data.keyword.cnc_long}}' auf {{site.data.keyword.BluOpenStackDed}}.

**Wichtig:** Diese Dokumentationsreihe gilt nur für den Service '{{site.data.keyword.cnc_short}}' auf {{site.data.keyword.BluOpenStackDed}}. Sie bezieht sich nicht auf andere Watson-Services, die auf der öffentlichen IBM Cloud verfügbar sind.

**Anmerkung:** Releaseinformationen, die sich auf alle {{site.data.keyword.BluOpenStackDed}}-Services beziehen, finden Sie unter [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv){: new_window}.

## Versionierung der Service-API
{: #api_versioning}

API-Anforderungen erfordern einen Versionsparameter, der ein Datum im Format `version=JJJJ-MM-TT` akzeptiert. Bei jeder rückwärtskompatiblen Änderung an der API wird jeweils eine neue untergeordnete Version der API freigegeben.

Senden Sie den Versionsparameter mit jeder API-Anforderung. Der Service verwendet die API-Version des von Ihnen angegebenen Datums oder die letzte Version vor diesem Datum. Verwenden Sie nicht standardmäßig das aktuelle Datum. Geben Sie stattdessen ein Datum an, das einer Version entspricht, die mit Ihrer Anwendung kompatibel ist, und ändern Sie es erst, wenn Ihre Anwendung für eine spätere Version bereit ist.

Die aktuelle Version ist `2018-03-23`.

## Änderungen
{: #changes}

Die folgenden neuen Features und Änderungen am Service sind verfügbar.

**Wichtig**: Bei der Versionsnummer, auf die in den folgenden Abschnitten Bezug genommen wird, handelt es sich um die Version des Helm-Diagramms für {{site.data.keyword.cnc_long}}, die das Sie in Ihrem {{site.data.keyword.BluOpenStackDed}}-Cluster bereitgestellt haben.

### 1.0.5, 2. August 2018
{: #105}

  - Die Syntaxanalyse der Tabelle, die unter [Informationen zum Ausgabeschema](/docs/services/compare-and-comply/schema.html#output_schema) und [Informationen zur Syntaxanalyse der Tabelle](/docs/services/compare-and-comply/tables.html#understanding_tables) beschrieben ist.


### 1.0.4, 5. Juli 2018
{: #ingress}

**Wichtig**: Aufgrund der in diesem Abschnitt beschriebenen Änderungen ist es nicht möglich, für ältere Versionen von {{site.data.keyword.cnc_short}} unter Verwendung der standardmäßigen Upgradeprozeduren von {{site.data.keyword.BluOpenStackDed}} ein Upgrade auf Version 1.0.4 durchzuführen. Stattdessen müssen Sie {{site.data.keyword.cnc_short}} wie folgt erneut bereitstellen:

1.  Entfernen Sie die vorhandene Bereitstellung von {{site.data.keyword.cnc_short}} wie in [Bereitstellung entfernen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window} beschrieben.

1.  Implementieren Sie {{site.data.keyword.cnc_short}} Version 1.0.4 oder höher wie in der Datei `README.md` des Helm-Diagramms für {{site.data.keyword.cnc_short}} und in [Service bereitstellen](/docs/services/compare-and-comply/deploy.html) beschrieben.

**Wichtig:** Die erneute Bereitstellung des Service zieht die folgenden Konsequenzen nach sich:

- Der Service steht in der Zeit zwischen dem Entfernen der vorherigen Instanz und dem Bereitstellen der aktuellen Instanz nicht zur Verfügung.
- Wenn Sie über Code verfügen, der API-Aufrufe vornimmt, bei denen Portnummern in die URLs der betreffenden Aufrufe eingebunden sind, müssen Sie die Portnummern entfernen. Wenn Sie an {{site.data.keyword.cnc_short}} Version 1.0.4 oder höher einen API-Aufruf richten, der eine Portnummer enthält, schlägt dieser Aufruf mit einem Fehler ähnlich dem folgenden fehl:
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**Wichtig:** Wenn Sie {{site.data.keyword.cnc_short}} in einer älteren Version als 1.0.4 ausführen, müssen Sie die Kennung `:{portnummer}` wie im folgenden Beispiel mit der IP-Adresse des {{site.data.keyword.BluOpenStackDed}}-Clusters angeben, wenn Sie Aufrufe an den Service durchführen:
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Zusätzliche Anmerkungen

-   Weitere Anweisungen zur Bereitstellung sind jetzt in [Service bereitstellen](/docs/services/compare-and-comply/deploy.html) enthalten.
-   Der Service '{{site.data.keyword.cnc_short}}' ist jetzt mit dem [Ingress-Controller ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window} von {{site.data.keyword.BluOpenStackDed}} integriert. Dank dieser Änderung ist es nun möglich, die API-Methoden des Service aufzurufen, ohne in den URLs der Aufrufe eine Portnummer angeben zu müssen. Der folgende API-Aufruf ist ein typisches Beispiel für Aufrufe vor der **Ingress**-Integration:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  Der funktional entsprechende Aufruf sieht in Version 1.0.4 oder höher wie folgt aus:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- Bei der Bereitstellung des Release 1.0.4 wird möglicherweise ein Fehler wie der folgende angezeigt:

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    Diesen Fehler können Sie problemlos ignorieren und schließen.

### 1.0.3, 25. Mai 2018

- Die Ausgabe der `parse`-Methode enthält nun das Array `attributes`. Nähere Informationen finden Sie in [Analyse überprüfen](/docs/services/compare-and-comply/getting-started.html#review_analysis) und [Attribute](/docs/services/compare-and-comply/parsing.html#attributes). Sie können die Informationen im Array `attributes` verwenden, um nach Dokumentelementen zu suchen, die sich auf bestimmte Standorte, auf Zeiten, Daten, Zeiträume oder Datumsbereiche oder auf Geldwerte und Einheiten beziehen.
- Jedes Objekt in den Arrays `types` und `categories` enthält ein Objekt vom Typ `provenance`. Das Objekt `provenance` verfügt über einen oder mehrere Schlüssel vom Typ `id`. Jeder `id`-Schlüssel besitzt einen Hashwert, den Sie an IBM senden können, um Feedback zu liefern oder Unterstützung zu erhalten.
- Verbesserungen bei der Genauigkeit des PDF-Parsings und der Leistung beim Parsing umfangreicher PDF-Dateien.
- Dieses Release ist auf {{site.data.keyword.BluOpenStackDed}} Version 2.1.0.2 oder höher verfügbar. Detaillierte Informationen zu den Anforderungen für {{site.data.keyword.BluOpenStackDed}} können Sie dem Katalogeintrag für den Service entnehmen.
- Für den Schlüssel `assurance` für die Verlässlichkeitsbewertung ist der Wert `Low` nicht mehr gültig.

### 1.0.2, 19. April 2018

- Erweiterung durch zusätzlich unterstützte Kategorien. Die jeweils aktuellste Liste der unterstützten Kategorien finden Sie in der [Dokumentation zu Kategorien](/docs/services/compare-and-comply/parsing.html#contract_categories).
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  Aktualisierung der Dokumentation einschließlich Korrekturen an den Beispielen in der [Einführung](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (GA-Release), 23.März 2018

Die folgenden Hinweise betreffen das Release zur allgemeinen Verfügbarkeit (GA-Release) des Service '{{site.data.keyword.cnc_long}}'.

- Das GA-Release ist nur auf {{site.data.keyword.BluOpenStackDed}} Version 2.1.0.2 oder höher verfügbar. Detaillierte Informationen zu den Anforderungen für {{site.data.keyword.BluOpenStackDed}} können Sie dem Katalogeintrag für den Service entnehmen.

## Allgemeine Hinweise
{: #general_notes}

- Die API von {{site.data.keyword.cnc_short}} auf {{site.data.keyword.BluOpenStackDed}} erfordert keine Berechtigung, wie es bei APIs auf der öffentlichen IBM Cloud der Fall ist.
 - PDFs muss in Textformat vorliegen, um geparst werden zu können. Gescannte Dokumente können nicht geparst werden, selbst wenn die Scans mit einem optischen Zeichenleser (OCR) verarbeitet wurden.

## Bekannte Probleme
{: #known_issues}

- Die maximale Größe einer PDF-Datei, die hochgeladen werden kann, ist auf 50 MB beschränkt.
- PDFs mit aktivierter Sicherheitsfunktion können nicht geparst werden.
- Dokumente mit einem Seitenlayout, das vom Standard abweicht (z. B. mit 2 oder 3 Spalten pro Seite), werden nicht fehlerfrei geparst.
