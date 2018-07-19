---

copyright:
years: 2017, 2018
lastupdated: "2018-03-23"

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

# Protokollierung verwenden
{: #using-logging}

## Protokollierungsdashboards installieren und ausführen

Führen Sie zum Installieren des Protokollierungsdashboards für {{site.data.keyword.cnc_short}} die folgenden Schritte aus.

  1. Laden Sie die Passport Advantage-Datei (PPA-Datei) für {{site.data.keyword.cnc_short}} herunter. Diese Datei liegt als komprimierte TAR-Datei vor, deren Benennung dem Muster `ibm-watson-compare-comply-prod-1.0.0.tar.gz` entspricht. Die Datei enthält die Vorlagen für die Protokollierungsdashboards sowie ein `Bash`-Script für die Wiedergabe der Dashboards aus den Vorlagen.

  1. Dekomprimieren und erweitern Sie die PPA-Datei:
    ```bash
    $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
    ```
    {: codeblock}

  1. Wechseln Sie im extrahierten Verzeichnis in das Verzeichnis `charts`:
    ```bash
    $ cd ibm-watson-compare-comply-prod-1.0.0/charts
    ```

  1. Dekomprimieren und erweitern Sie die komprimierte TAR-Datei im Verzeichnis `charts`:
    ```bash
    $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
    ```

  1. Wechseln Sie in das Verzeichnis `dashboard`. Dieses Verzeichnis enthält Vorlagen für Metriken und für die Protokollierung sowie ein Bash-Script zum Generieren von Dashboards aus Vorlagen.

    ```bash
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
  
    -  `-v, --version {diagrammversion}`: Gibt die Diagrammversion an, zum Beispiel `1.0.0`.
    -  `-h, --help`: Gibt Hilfe für Befehle aus und wird dann beendet.
    -  `-r, --release {releasename}`: Der Name des Helm-Release.
    -  `-n, --namespace {namensbereich}`: Der Namensbereich der Bereitstellung. Der Standardnamensbereich lautet `default`.

    ```bash
    $ ./render-dashboards.sh -v 1.0.0 -r my-test-release -n default
    Die JSON-Dashboarddateien werden unter dem folgenden Verzeichnis generiert: /Users/{benutzer}/Downloads/ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard.

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

## Protokollierungsdashboards importieren

Führen Sie zum Importieren der Protokollierungsdashboards für {{site.data.keyword.cnc_short}} in IBM Cloud Private die folgenden Schritte aus.

  1. Melden Sie sich bei Ihrem IBM Cloud Private-Cluster an.

  1. Wählen Sie über das Menüsymbol in der linken oberen Ecke die Optionen **Plattform -> Protokollierung** aus. <br />
    ![Symbol für das Menü 'IBM Cloud Private'](images/icp-menu.png) <br />
    ![Menü 'Plattform -> Protokollierung'](images/icp-logging.png)

  1. Klicken Sie auf der linken Seite der Kibana-Schnittstelle auf **Verwaltung**.<br />
    ![Kibana-Schnittstelle](images/kibana.png)

  1. Wählen Sie die Registerkarte **Gespeicherte Objekte** aus.
    ![Registerkarte 'Gespeicherte Objekte'](images/saved-obj.png)

  1. Wählen Sie die Registerkarte **Suchvorgänge** aus und klicken Sie auf **Importieren**.
    !['Importieren' auf Registerkarte 'Suchvorgänge'](images/searches-import.png)

  1. Importieren Sie nacheinander die Dateien `frontend-logging.json` und `external-process-logging.json`, die in der vorherigen Prozedur in Schritt 6 generiert wurden. Wenn eine entsprechende Eingabeaufforderung angezeigt wird, klicken Sie auf **Ja, alle überschreiben**.
     ![Eingabeaufforderung 'Ja, alle überschreiben'](images/overwrite-all.png)

  1. Die Dashboards werden auf der Registerkarte **Suchvorgänge** angezeigt.
     ![Dashboards auf der Registerkarte 'Suchvorgänge'](images/searches-tab.png)

## Protokollierungsdashboards anzeigen

Führen Sie zum Anzeigen der Protokollierungsdashboards die folgenden Schritte aus.

  1. Navigieren Sie zu der Registerkarte **Erkennen**.

  1. Klicken Sie im Bereich oben rechts der Kibana-Schnittstelle auf **Öffnen**.

  1. Wählen Sie das Dashboard aus, das angezeigt werden soll. Es gibt zwei Protokollierungsdashboards: eines für die Protokollierung des Serviceprotokolls und eines für die Protokollierung externer Prozesse.
    ![Dashboards für die Protokollierung anzeigen](images/kibana-dboards.png)

Sie können den Zeitraum und die Häufigkeit der automatischen Aktualisierung der Anzeige ohne großen Aufwand ändern:
  ![Zeitbereich und Aktualisierungsrate ändern](images/log-dboard-change.png)

