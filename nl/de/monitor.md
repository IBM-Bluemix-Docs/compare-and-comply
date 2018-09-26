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

# Überwachungsdashboards installieren, konfigurieren und verwenden
{: #monitor}

Mithilfe der Überwachungsdashboards von IBM Cloud Private können Sie den Status von {{site.data.keyword.cnc_short}} überwachen. Die Überwachungsdashboards verwenden Grafana, Kibana und Prometheus, um detaillierte, anpassbare Informationen zu Ihrer {{site.data.keyword.cnc_short}}-Instanz anzuzeigen.

Weitere Informationen zum Überwachen Ihres {{site.data.keyword.BluOpenStackDed}}-Clusters finden Sie im Knowledge Center unter [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.

Bevor Sie die Dashboards verwenden, müssen Sie sie wie in den folgenden Schritten beschrieben generieren und installieren.

## Schritt 1: Dashboardvorlagen herunterladen, extrahieren und wiedergeben
{: #render}

Führen Sie die folgenden Schritte aus, um die Dashboardvorlagen für die Installation vorzubereiten.

1. Laden Sie das {{site.data.keyword.cnc_short}}-Image von Passport Advantage (PPA) herunter. Die Datei liegt als komprimierte TAR-Datei vor, deren Benennung dem Muster `ibm-watson-compare-comply-prod-1.0.5.tar.gz` entspricht. Die Datei enthält die Dashboardvorlagen sowie ein `Bash`-Script für die Wiedergabe der Dashboards aus den Vorlagen.

1. Dekomprimieren und erweitern Sie die TAR-Datei:
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. Wechseln Sie im extrahierten Verzeichnis in das Verzeichnis `charts`:
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. Dekomprimieren und erweitern Sie die komprimierte TAR-Datei im Verzeichnis `charts`:
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. Wechseln Sie in das Verzeichnis `dashboard`. Dieses Verzeichnis enthält Vorlagen für Alerts, für die Protokollierung und für Metriken sowie ein `Bash`-Script zum Generieren von Dashboards
aus Vorlagen.
  ```
  $ cd ibm-watson-compare-comply-prod/dashboard
  
  $ tree
    .
  ├── alerts.json.tpl
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json.tpl
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directories, 5 files
  ```

1. Führen Sie das Script `render-dashboards.sh` aus, damit die Vorlagen wiedergegeben werden. Für das Script gibt es die folgenden Ausführungsoptionen:
  
    - `-v`, `--version {diagrammversion}`: Gibt die Diagrammversion an, zum Beispiel `1.0.5`.
    - `-h`, `--help`: Gibt Hilfe für Befehle aus und wird dann beendet.
    - `-r`, `--release {releasename}`: Der Name des Helm-Release.
    - `-n`, `--namespace {namensbereich}`: Der Namensbereich der Bereitstellung. Der Standardnamensbereich lautet `default`.

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

Die JSON-Dateien der Dashboards werden im Verzeichnis `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard` generiert.

  ```
  $ tree
    .
  ├── alerts.json
  ├── alerts.json.tpl
  ├── external-process-logging.json
  ├── external-process-logging.json.tpl
  ├── frontend-logging.json
  ├── frontend-logging.json.tpl
  ├── metrics.json
  ├── metrics.json.tpl
  └── render-dashboards.sh

  0 directories, 9 files
  ```

## Schritt 2: Vorlagen importieren
{: #import-tpls}

Nachdem Sie die Dashboards wiedergegeben haben, importieren Sie mindestens eines wie in den folgenden Abschnitten beschrieben:

  - [Metrikdashboard importieren](metrics.html#import)
  - [Protokollierungsdashboards importieren](logging.html#import)
  - [Alertdashboard importieren](alerts.html#import)

## Schritt 3: Dashboards anzeigen und anpassen
{: #use-dboards}

Nachdem Sie die ausgewählten Dashboards importiert haben, können Sie sie wie in den folgenden Abschnitten beschrieben anzeigen und anpassen:

  - [Metrikdashboard anzeigen](metrics.html#view)
  - [Protokollierungsdashboards anzeigen](logging.html#view)
  - [Alertregeln hinzufügen](alerts.html#add)
