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

# Service bereitstellen
{: #install}

Bevor Sie das Helmdiagramm für {{site.data.keyword.cnc_long}} wie in der Datei `README.md` des Helmdiagramms beschrieben installieren, müssen Sie möglicherweise noch einige zusätzliche Schritte für die Bereitstellung ausführen.

## Standardsystemwerte
{: #sys_vals}

In diesen Anweisungen werden die nachfolgend aufgeführten Standardsystemwerte verwendet. Möglicherweise weichen die Werte für Ihr spezielles System von den hier genannten Werten ab. Halten Sie diesbezüglich Rücksprache mit Ihrem IBM Cloud Private-Administrator.

 - Name und Portnummer des IBM Cloud Private-Clusters:
 	`mycluster.icp:8500`
 	
 - IBM Cloud Private-Namensbereich:
 	`default`

## Bereitstellung abschließen
{: #deploy}

Führen Sie zum Abschließen der {{site.data.keyword.cnc_short}}-Bereitstellung die folgenden Schritte aus:

### Schritt 1: Docker Zugriff erteilen
{: #step1}

  1. Erteilen Sie Docker Zugriff auf die IBM Cloud Private-Registry.
  
  	Wenn Sie Microsoft Windows oder Apple Mac OS bzw. OS X ausführen, führen Sie die folgenden Schritte aus:

 	 1. Klicken Sie auf das Symbol für **Docker**.
 	 1. Öffnen Sie das Docker-Menü **Vorgaben**.
 	 1. Klicken Sie auf die Registerkarte **Dämon**.
 	 1. Fügen Sie im Feld **Unsichere Registrys** den Wert `mycluster.icp:8500` oder den Wert für Ihre IBM Cloud Private-Installation hinzu.
 	 
 	 ![Docker konfigurieren](images/docker.png)
 	 
 	Wenn Sie Linux verwenden, führen Sie die folgenden Schritte aus:
 	
 	1. Suchen Sie die Datei `daemon.json`. Standardmäßig befindet sich diese Datei im Verzeichnis `/etc/docker/daemon.json`. Sollte diese Datei nicht vorhanden sein, erstellen Sie sie an ihrer Standardposition. Öffnen Sie die Datei in einem Texteditor und überprüfen Sie den folgenden JSON-Code oder fügen Sie diesen bei Bedarf hinzu.
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. Führen Sie die folgenden Befehle aus:
 	  ```bash
 	  systemctl daemon reload
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### Schritt 2: Befehlszeilenschnittstellen für IBM Cloud Private und IBM Cloud herunterladen
{: #step2}

  1. Laden Sie die Helm-Befehlszeilenschnittstelle (Command Line Interface, CLI) für jede der von Ihnen verwendeten Version von IBM Cloud Private herunter und installieren Sie sie. Detaillierte Informationen hierzu enthält die [Dokumentation zu IBM Cloud Private ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window}.
  
  1. Laden Sie die Befehlszeilenschnittstelle (CLI) für IBM Cloud herunter und installieren Sie sie. Detaillierte Informationen hierzu enthält die [Dokumentation zu IBM Cloud ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window}.
  
  1. Laden Sie das Plug-in `bx` für die IBM Cloud-Befehlszeilenschnittstelle (CLI) herunter und installieren Sie es. Detaillierte Informationen hierzu enthält die [Dokumentation zu IBM Cloud ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window}.
 	
### Schritt 3: Laden Sie die Paketdatei herunter
{: #step3}
 	
Laden Sie die Paketdatei `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` von [Passport Advantage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-01.ibm.com/software/passportadvantage/){: new_window} auf Ihre lokale Workstation herunter. Diese Datei enthält das vollständige Image für den {{site.data.keyword.cnc_short}}-Service.
  
#### Bearbeiten Sie die Paketdatei bei Bedarf.
{: edit-pkg-file}

**Wichtig**: Führen Sie die Prozedur in diesem Schritt *nur dann* aus, wenn die beiden folgenden Bedingungen zutreffen. Sollte nur eine oder sogar keine dieser Bedingungen zutreffen, fahren Sie mit [Schritt 4](#step4) fort.

 - Dieser Schritt gilt nur für {{site.data.keyword.cnc_short}} bis einschließlich Version 1.0.3. Wenn Sie {{site.data.keyword.cnc_short}} Version 1.0.4 oder höher installieren, führen Sie die in diesem Schritt beschriebene Prozedur nicht aus.

 - Sie müssen gemäß der in diesem Schritt erläuterten Prozedur nur dann vorgehen, wenn Ihr IBM Cloud Private-Cluster nicht mit dem externen Internet verbunden ist. Wenn Ihr IBM Cloud Private-Cluster Zugriff auf die Site von [Passport Advantage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-01.ibm.com/software/passportadvantage/){: new_window} hat, können Sie Version 1.0.3 von {{site.data.keyword.cnc_short}} installieren und bereitstellen, ohne dass Sie hierzu Änderungen an der Paketdatei vornehmen müssen.

**Hinweis**: Wenn Sie die Prozedur in diesem Schritt ausführen, können Probleme mit Upgrades und Rollbacks von {{site.data.keyword.cnc_short}} Version 1.0.3 auftreten. 
 
  1. Extrahieren Sie die Datei `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` in einer `Bash`-Shell oder einer ähnlichen Umgebung:
  
 	```bash
 	cd {pfad_zur_datei}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	Die extrahierte Datei enthält ein Verzeichnis namens `charts` mit einer Datei namens `ibm-watson-compare-comply-prod-1.0.3.tgz`. Bei dieser Datei handelt es sich um die komprimierte Version des Helm-Diagramms für {{site.data.keyword.cnc_short}}.
 	
  1. Wechseln Sie in das Verzeichnis `charts` und extrahieren Sie die Datei `ibm-watson-compare-comply-prod-1.0.3.tgz`:
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	Die extrahierte Datei umfasst eine Reihe von Verzeichnissen und Dateien, unter anderem das Basisverzeichnis `ibm-watson-compare-comply-prod`, das ein Verzeichnis namens `templates` enthält.
 	
  1. Wechseln Sie in das Verzeichnis `ibm-watson-compare-comply-prod/templates`:
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. Suchen Sie im Verzeichnis `ibm-watson-compare-comply-prod/templates` die Datei `deployment.yaml` und öffnen Sie diese in einem Texteditor.
 
  1. Bearbeiten Sie die Datei `deployment.yaml` wie folgt:
  
 	**Ursprünglicher Inhalt**
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	**Aktualisierter Inhalt**
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	Speichern und schließen Sie die Datei `deployment.yaml`.
 	
  1. Kehren Sie zum Verzeichnis `charts` zurück und packen Sie die Datei `ibm-watson-compare-comply-prod-1.0.3.tgz` erneut:
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
 	{: pre}
 	
  1. Kehren Sie zum Basisverzeichnis zurück und packen Sie die Datei `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` erneut:
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 Zum Vergleich sieht die `tree`-Ansicht (Baumstrukturansicht) des Inhalts der dekomprimierten Datei `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` wie folgt aus. Die an dieser Prozedur beteiligten Dateien sind in der rechten Spalte mit `<-----` gekennzeichnet.
 	 
 	 ```bash
 	 cd {pfad_zur_archivdatei}
 	 ```
 	 {: pre}
 	 
 	 ```bash
 	 tree
 	 
	 .
	 ├── IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz          <-----
     ├── charts                                            <-----
     │   ├── ibm-watson-compare-comply-prod
     │   │   ├── Chart.yaml
     │   │   ├── LICENSE
     │   │   ├── LICENSES
     │   │   │   └── LICENSE-Product
     │   │   ├── README.md
     │   │   ├── cnc-banner.png
     │   │   ├── dashboard
     │   │   │   ├── alerts.json.tpl
     │   │   │   ├── external-process-logging.json.tpl
     │   │   │   ├── frontend-logging.json.tpl
     │   │   │   ├── metrics.json.tpl
     │   │   │   └── render-dashboards.sh
     │   │   ├── templates                                 <-----
     │   │   │   ├── NOTES.txt
     │   │   │   ├── _helpers.tpl
     │   │   │   ├── _sch-chart-config.tpl
     │   │   │   ├── configmap.yaml
     │   │   │   ├── deployment.yaml                       <-----
     │   │   │   ├── ingress.yaml
     │   │   │   ├── metrics-service.yaml
     │   │   │   ├── sch-2.6.0
     │   │   │   │   ├── _config.tpl
     │   │   │   │   ├── _metadata.tpl
     │   │   │   │   ├── _names.tpl
     │   │   │   │   ├── _utils.tpl
     │   │   │   │   └── _version.tpl
     │   │   │   ├── service.yaml
     │   │   │   └── tests
     │   │   │       └── service-test.yaml
     │   │   ├── values-metadata.yaml
     │   │   └── values.yaml
     │   └── ibm-watson-compare-comply-prod-1.0.3.tgz      <-----
     ├── images
     │   ├── 304735eab4e4df55b236f9835104cb1058bc50cb5ff7f4ee7d719cb56d2a718b.tar.gz
     │   ├── af80995a4fcfcee364f4f72ba043bf9e73dcafe7fcac384e1cd06bf1c7a52c1f.tar.gz
     │   └── ccec10461710cd625cbbacc0f5d5d837a0cc5baf132e0fc39ed42d8264d25062.tar.gz
     ├── manifest.json
     └── manifest.yaml 
 	 ```
 	 {: pre}
   
### Schritt 4: Terminal vorbereiten
{: #step4}
 	
Bereiten Sie eine `Bash`-Shell oder eine vergleichbare Shell für die Verwendung mit Ihrer IBM Cloud Private-Installation vor, indem Sie die folgenden Befehle in der Shell ausführen:
  
  ```bash
  bx pr login -a https://mycluster.icp:8443 --skip-ssl-validation
  ```
  {: pre}
  
  ```bash
  bx pr cluster-config mycluster
  ```
  {: pre}
  
  ```bash
  docker login mycluster.icp:8500
  ```
  {: pre}

  **Hinweis**: Im ersten Befehl in der vorherigen Liste stellt die Portnummer `8443` in der URL `https://mycluster.icp:8443` den Standardwert für die Kommunikation mit dem {{site.data.keyword.cnc_short}}-Service in IBM Cloud Private dar.

### Schritt 5: Geheimen Schlüssel für Image-Registry und Patch-Servicekonto erstellen
{: #step5}

Führen Sie die folgenden Befehle aus, um den Zugriff auf die interne Registry von IBM Cloud Private durch alle erforderlichen Komponenten zu ermöglichen:

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

Hierbei gilt Folgendes:
  - Die Werte für `$username` und `$password` hängen von dem jeweiligen Cluster ab. Wenden Sie sich diesbezüglich an Ihren Administrator.
  - `$secret_name` ist eine Zeichenfolge, die Sie angeben, um [Docker-Containern die sichere Kommunikation zu ermöglichen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.docker.com/engine/swarm/secrets/){: new_window}.

### Schritt 6: Paketdatei in interne Image-Registry von IBM Cloud Private hochladen
{: #step6}

Führen Sie in dem in [Schritt 4](#step4) vorbereiteten Terminal den folgenden Befehl aus, um die Passport Advantage-Paketdatei hochzuladen:

```bash
bx pr load-ppa-archive --archive {pfad_zur_archivdatei}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default
```
{: pre}

### Schritt 7: Installation abschließen

Kehren Sie in Ihrem IBM Cloud Private-Cluster zur Seite **Katalog** zurück, suchen Sie den Eintrag `ibm-watson-compare-comply-prod` und wählen Sie ihn aus und klicken Sie dann auf **Konfigurieren**, um mit der Installation fortzufahren.
