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

# Metriken verwenden
{: #metrics}

Mithilfe des Überwachungsdashboards von IBM Cloud Private können Sie den Status von {{site.data.keyword.cnc_short}} überwachen. Das Überwachungsdashboard verwendet Grafana für Metriken, Prometheus für Alerts und Kibana für die Protokollierung, um detaillierte Informationen zu Ihrer Instanz von {{site.data.keyword.cnc_short}} darzustellen.

## Metrikdashboard importieren

Führen Sie zum Importieren des Metrikdashboards für {{site.data.keyword.cnc_short}} in IBM Cloud Private die folgenden Schritte aus.

  1. Stellen Sie sicher, dass Sie die Metrikdashboards extrahiert und generiert haben, wie im Abschnitt [Schritt 1: Dashboardvorlagen herunterladen, extrahieren und wiedergeben](/docs/services/compare-and-comply/monitor.html#monitor) beschrieben.

  1. Melden Sie sich bei Ihrem IBM Cloud Private-Cluster an.

  1. Wählen Sie über das Menüsymbol in der linken oberen Ecke die Optionen **Plattform -> Überwachung** aus. <br />
      ![Symbol für das Menü 'IBM Cloud Private'](images/icp-menu.png) <br />
      ![Menü 'Plattform -> Überwachung'](images/icp-monitoring.png)

  1. Klicken Sie im Bereich der linken oberen Ecke der Grafana-Schnittstelle auf **Home**. <br />
      ![Symbol 'Home'](images/icp-home.png)

  1. Klicken Sie auf **Dashboard importieren**.
      ![Symbol 'Dashboard importieren'](images/import-dboard.png)

  1. Wählen Sie die Datei `metrics.json` aus, die in der vorherigen Prozedur in Schritt 6 generiert wurde, und klicken Sie dann auf **JSON-Datei hochladen**. <br />
      ![Datei 'metrics.json' hochladen](images/metrics-json.png)

  1. Wählen Sie **Prometheus** als Datenquelle aus und klicken Sie dann auf **Importieren**.
       ![Prometheus auswählen](images/prometheus.png)

## Metrikdashboard anzeigen
{: #view}

Das Metrikdashboard ähnelt der folgenden Darstellung:
![Metrikdashboard](images/metrics-dboard.png)

Sie können den Zeitraum und die Häufigkeit der automatischen Aktualisierung der Anzeige ohne großen Aufwand ändern:
![Zeitbereich und Aktualisierungsrate ändern](images/dboard-change.png)

## Metrikdashboard bearbeiten

Sie können das Metrikdashboard bearbeiten oder durch Ausführen der folgenden Schritte ein neues Metrikdashboard erstellen.

  1. Wählen Sie über das Menüsymbol in der linken oberen Ecke die Optionen **Plattform -> Überwachung** aus, um auf die Benutzerschnittstelle von Grafana zuzugreifen.

  1. Klicken Sie im Bereich der linken oberen Ecke der Grafana-Schnittstelle auf **Home** und anschließend auf **+ Neues Dashboard**.

  1. Wählen Sie aus, welcher Anzeigetyp hinzugefügt werden soll, beispielsweise **Diagramm** oder **Tabelle**.

  1. Klicken Sie auf die Anzeigenüberschrift und dann auf **Bearbeiten**. Die standardmäßige Anzeigenüberschrift lautet `Panel title`.

  1. Legen Sie auf der Registerkarte **Allgemein** die Überschrift für die Anzeige, eine Beschreibung und die Größe der Anzeige fest. Beachten Sie, dass 12 Einheiten der vollen Breite eines Browserfensters entsprechen.

  1. Verwenden Sie die Registerkarte **Metriken**, um Abfragen zu erstellen, die Daten aus Prometheus darstellen.

    1. Wenn Sie mit der Abfragesprache vertraut sind, können Sie die Abfrage direkt formulieren. Andernfalls verwenden Sie das Feld **Metriksuche**, um eine Auswahl bei den Metriken zu treffen, die gegenwärtig bei Prometheus gemeldet werden.

    1. Die Ergebnisse der Abfragen werden in Echtzeit in der neuen Dashboardanzeige angezeigt.

    1. Zu einer einzigen Anzeige können mehrere Abfragen hinzugefügt werden. Sie können beispielsweise Lese- und Schreiboperationen in demselben Diagramm oder die Gesamtzahl der Besuche wie auch der Besucher in derselben Tabelle anzeigen lassen.
        
