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

# Bereitstellungskonfigurationen verstehen
{: #configs}

Die standardmäßige Bereitstellungskonfiguration für {{site.data.keyword.cnc_short}} umfasst `2 Kerne, 10 G, 1 parallel verarbeitetes Dokument (2 VPCs)`. Diese Konfiguration ist in der Lage, ungefähr 1.500 Seiten pro Stunde zu verarbeiten. Sie können Ihre {{site.data.keyword.cnc_short}}-Bereitstellung auf IBM Cloud Private abhängig vom jeweils erforderlichen Durchsatz auf verschiedene Ebenen vertikal per Scale-up skalieren. Wenden Sie sich an Ihren IBM Vertriebsbeauftragten, um weitere Informationen zur Steigerung der Kapazität von {{site.data.keyword.cnc_short}} auf IBM Cloud Private zu erhalten.

Die Skalierung von {{site.data.keyword.cnc_short}} kann in zwei Richtungen erfolgen: _vertikal_ oder _horizontal_.

 - Bei der _vertikalen_ Skalierung (Scale-up) wird der Durchsatz erhöht, mit dem das Parsing eines Dokuments erfolgt.
 - Bei der _horizontalen_ Skalierung (Scale-out) wird die Zahl der Dokumente gesteigert, die gleichzeitig geparst werden.

Für beide Skalierungsoptionen muss die Zahl der von {{site.data.keyword.cnc_short}} verwendeten virtuellen Prozessorkerne (Virtual Processor Cores, VPCs) aufgestockt werden. Informationen zu virtuellen Prozessorkernen enthält die [Lizenzierungsdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window} für IBM Cloud Private.

Für die vertikale Skalierung gibt es die folgenden Optionen:

| Konfiguration                             |Ungefährer Durchsatz*         |
|:-----------------------------------------:|--------------------------------|
|`2 Kerne, 10 G, 1 parallel verarbeitetes Dokument (2 VPCs)` |1.500 Seiten pro Stunde (Standard)   |
|`4 Kerne, 20 G, 1 parallel verarbeitetes Dokument (4 VPCs)` |2.500 Seiten pro Stunde             |
|`8 Kerne, 40 G, 1 parallel verarbeitetes Dokument (8 VPCs)` |3.000 Seiten pro Stunde             |

Für die horizontale Skalierung gibt es die folgenden Optionen:

| Konfiguration                               |Ungefährer Durchsatz*         |
|:-------------------------------------------:|--------------------------------|
|`2 Kerne, 10 G, 2 parallel verarbeitete Dokumente (4 VPCs)`  |3.000 Seiten pro Stunde             |
|`4 Kerne, 20 G, 2 parallel verarbeitete Dokumente (8 VPCs)`  |5.000 Seiten pro Stunde             |
|`8 Kerne, 40 G, 2 parallel verarbeitete Dokumente (16 VPCs)` |6.000 Seiten pro Stunde             |

\***Hinweis**: Die Schätzwerte für den Durchsatz stützen sich auf interne Tests. Der Durchsatz in einer Produktionsumgebung hängt von mehreren Faktoren ab, z. B. anderen Arbeitslasten im selben IBM Cloud Private-Cluster, Latenzen zwischen der Clientanwendung und dem IBM Cloud Private-Cluster sowie anderen Umgebungsbedingungen.

Führen Sie zum Ändern der Bereitstellungskonfiguration durch Hinzufügen oder Entfernen von {{site.data.keyword.cnc_short}}-Clustern die folgenden Schritte aus.

**Hinweis**: Wenn Sie eine {{site.data.keyword.cnc_short}}-Bereitstellung per Scale-down nach unten skalieren, fordert IBM Cloud Private unverzüglich die freigegebenen Ressourcen zurück, wodurch die Verarbeitungskapazität der Bereitstellung verringert wird. Führen Sie für eine aktive Bereitstellung, die verwendet wird, keinesfalls ein Scale-down durch, insbesondere dann nicht, wenn sich die Bereitstellung in einer Produktionsumgebung befindet.
	
Ebenso wendet IBM Cloud Private beim Durchführen eines Scale-ups für eine {{site.data.keyword.cnc_short}}-Bereitstellung unverzüglich die angeforderten Ressourcen auf die Bereitstellung an, vorausgesetzt, dass diese Ressourcen verfügbar sind. Wenn Sie mehr Ressourcen benötigen, als Ihre IBM Cloud Private-Installation bereitstellen kann, wenden Sie sich hinsichtlich einer Steigerung der Kapazität Ihrer IBM Cloud Private-Installation an Ihren IBM Vertriebsbeauftragten.

  1. Melden Sie sich bei Ihrem IBM Cloud Private-Cluster an.

  1. Wählen Sie über das Menüsymbol nahe der linken oberen Ecke die Optionen **Arbeitslasten -> Bereitstellungen** aus.
  
  1. In der Konsole wird die Seite **Bereitstellungen** angezeigt. Suchen Sie in der Tabelle der Bereitstellungen diejenige Bereitstellung, die Sie skalieren wollen.
  
  1. Klicken Sie in der Tabellenzeile mit der gesuchten Bereitstellung in der letzten Spalte der Tabelle auf das Symbol **Aktion** und anschließend auf **Skalieren**.
  
  1. In der Konsole wird die Seite **Bereitstellung skalieren** angezeigt. Geben Sie im Feld **Instanzen** die Anzahl der gewünschten Instanzen ein oder wählen Sie den gewünschten Wert mit dem Aufwärts- bzw. Abwärtspfeil aus.
  
  1. Klicken Sie auf **Bereitstellung skalieren**.
  
  1. Das Fenster **Bereitstellung skalieren** wird geschlossen und die Konsole zeigt wieder die Seite **Bereitstellungen** an. Suchen Sie Ihre Bereitstellung und überprüfen Sie mit Hilfe der Spalten **Gewünscht** und **Aktuell**, ob die von Ihnen angeforderte Anzahl von Instanzen auch tatsächlich der Bereitstellung zugeordnet worden ist.
  
  1. Optional können Sie auf der Seite **Bereitstellungen** auf den Namen der entsprechenden Bereitstellung klicken, damit die zugehörigen Detailinformationen angezeigt werden.
  
  

