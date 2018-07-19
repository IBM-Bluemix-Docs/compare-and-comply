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

# Distribuzione del servizio
{: #install}

Prima di installare il grafico Helm {{site.data.keyword.cnc_long}} come descritto nel relativo file `README.md`, potresti dover eseguire ulteriori passi di distribuzione.

## Valori di sistema predefiniti
{: #sys_vals}

Queste istruzioni utilizzano i seguenti valori di sistema predefiniti. I valori per il tuo sistema potrebbero essere diversi. Consulta il tuo amministratore di IBM Cloud Private.

 - Nome cluster e numero di porta di IBM Cloud Private:
 	`mycluster.icp:8500`
 	
 - Spazio dei nomi di IBM Cloud Private:
 	`default`

## Completamento della distribuzione
{: #deploy}

Effettua la seguente procedura per completare la distribuzione di {{site.data.keyword.cnc_short}}:

### Passo 1: concedi l'accesso Docker
{: #step1}

  1. Concedi l'accesso Docker al registro IBM Cloud Private.
  
  	Se utilizzi Microsoft Windows, Apple macOS o OS X, completa la seguente procedura:

 	 1. Fai clic sull'icona **Docker**.
 	 1. Apri il menu **Preferences** di Docker.
 	 1. Fai clic sulla scheda **Daemon**.
 	 1. Nel campo **Insecure registries**, aggiungi il valore `mycluster.icp:8500` o il valore per la tua installazione di IBM Cloud Private.
 	 
 	 ![Configura Docker](images/docker.png)
 	 
 	Se utilizzi Linux, completa la seguente procedura:
 	
 	1. Individua il file `daemon.json`. Per impostazione predefinita, questo file si trova in `/etc/docker/daemon.json`. Se il file non esiste, crealo nella posizione predefinita. Apri il file in un editor di testo e verifica o aggiungi il seguente JSON.
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. Immetti i seguenti comandi:
 	  ```bash
 	  systemctl daemon reload
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### Passo 2: scarica le CLI di IBM Cloud Private e IBM Cloud
{: #step2}

  1. Scarica e installa la CLI Helm per ogni versione di IBM Cloud Private che stai utilizzando. Per i dettagli, consulta la [documentazione di IBM Cloud Private ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window}.
  
  1. Scarica e installa la CLI di IBM Cloud. Per i dettagli, consulta la [documentazione di IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window}.
  
  1. Scarica e installa il plug-in `bx` della CLI IBM Cloud. Per i dettagli, consulta la [documentazione di IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window}.
 	
### Passo 3: scarica il file di pacchetto
{: #step3}
 	
Scarica il file di pacchetto `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` da [Passport Advantage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/software/passportadvantage/){: new_window} nella tua workstation locale. Questo file contiene l'intera immagine del servizio {{site.data.keyword.cnc_short}}.
  
#### Modifica il file di pacchetto secondo necessità
{: edit-pkg-file}

**Importante**: esegui la procedura in questo passo *solo* se le seguenti condizioni sono vere. Se una o entrambe le condizioni sono false, procedi al [Passo 4](#step4).

 - Questo passo si applica solo a {{site.data.keyword.cnc_short}} versione 1.0.3 e precedenti. Se stai installando {{site.data.keyword.cnc_short}} versione 1.0.4 o successive, non eseguire la procedura in questo passo.

 - Devi seguire la procedura in questo passo solo se il tuo cluster IBM Cloud Private non è connesso all'Internet esterno. Se il tuo cluster IBM Cloud Private dispone dell'accesso di rete al sito [Passport Advantage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/software/passportadvantage/){: new_window}, puoi installare e distribuire {{site.data.keyword.cnc_short}} versione 1.0.3 senza alcuna modifica al file di pacchetto.

**Nota**: se esegui la procedura in questo passo, potresti riscontrare dei problemi con gli aggiornamenti e i rollback di {{site.data.keyword.cnc_short}} versione 1.0.3. 
 
  1. In una shell `bash` o in un ambiente simile come Cygwin, estrai il file `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz`:
  
 	```bash
 	cd {path_to_file}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	Il file estratto include una directory denominata `charts` che contiene un file denominato `ibm-watson-compare-comply-prod-1.0.3.tgz`, che è una versione compressa del grafico Helm per {{site.data.keyword.cnc_short}}.
 	
  1. Passa alla directory `charts` ed estrai il file `ibm-watson-compare-comply-prod-1.0.3.tgz`:
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-comply-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	Il file estratto include una serie di directory e file, inclusa una directory di livello superiore denominata `ibm-watson-compare-comply-prod` che contiene una directory denominata `templates`.
 	
  1. Passa alla directory `ibm-watson-compare-comply-prod/templates`:
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. Individua il file `deployment.yaml` nella directory `ibm-watson-compare-comply-prod/templates` e aprilo in un editor di testo.
 
  1. Modifica il file `deployment.yaml` nel seguente modo:
  
 	**Originale**
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	**Aggiornato**
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	Salva e chiudi il file `deployment.yaml`.
 	
  1. Ritorna alla directory `charts` e riassembla il file `ibm-watson-compare-comply-prod-1.0.3.tgz`:
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-comply-prod-1.0.3.tgz ibm-watson-compare-comply-prod
 	```
 	{: pre}
 	
  1. Ritorna alla directory di livello superiore e riassembla il file `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz`:
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 Per riferimento, la vista ad `albero` dei contenuti del file `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` non compresso è la seguente. I file coinvolti in questa procedura sono richiamati con `<-----` nella colonna a destra.
 	 
 	 ```bash
 	 cd {path_to_archive_file}
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
   
### Passo 4: prepara un terminale
{: #step4}
 	
Prepara una shell `bash` o un equivalente da utilizzare con la tua installazione di IBM Cloud Private immettendo i seguenti comandi nella shell:
  
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

  **Nota**: nel primo comando dell'elenco precedente, il numero di porta `8443` nell'URL `https://mycluster.icp:8443` rappresenta il valore predefinito per la comunicazione con il servizio {{site.data.keyword.cnc_short}} su IBM Cloud Private.

### Passo 5: crea un account del servizio di patch e un segreto del registro di immagini
{: #step5}

Immetti i seguenti comandi per consentire a tutti i componenti richiesti di accedere al registro interno di IBM Cloud Private:

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

dove:
  - `$username` e `$password` variano in base al cluster. Rivolgiti al tuo amministratore.
  - `$secret_name` è una stringa che specifichi [per consentire ai contenitori Docker di comunicare in modo sicuro ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.docker.com/engine/swarm/secrets/){: new_window}.

### Passo 6: carica il file di pacchetto nel registro di immagini interno di IBM Cloud Private
{: #step6}

Nel terminale che hai preparato nel [Passo 4](#step4), immetti il seguente comando per caricare il file di pacchetto Passport Advantage:

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: pre}

### Passo 7: completa l'installazione

Ritorna alla pagina **Catalog** nel tuo cluster IBM Cloud Private, individua e seleziona la voce `ibm-watson-compare-comply-prod` e fai clic su **Configure** per procedere con l'installazione.
