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

# Implementando o serviço
{: #install}

Antes de instalar o gráfico Helm do {{site.data.keyword.cnc_long}} conforme descrito no arquivo `README.md` do gráfico Helm, pode ser necessário executar etapas de implementação adicionais.

## Valores do sistema padrão
{: #sys_vals}

Essas instruções usam os valores de sistema padrão a seguir. Os valores para seu sistema podem ser diferentes. Verifique com seu administrador do IBM Cloud Private.

 - Nome do cluster e número da porta do IBM Cloud Private:
  `mycluster.icp:8500`
 	
 - Namespace do IBM Cloud Private:
  `default`

## Concluindo a implementação
{: #deploy}

Execute as etapas a seguir para concluir a implementação do {{site.data.keyword.cnc_short}}:

### Etapa 1: conceder acesso ao Docker
{: #step1}

  1. Conceda acesso do Docker ao registro do IBM Cloud Private.
  
  	Se você estiver executando o Microsoft Windows ou o Apple macOS ou OS X, execute as etapas a seguir:

 	 1. Clique no ícone **Docker**.
 	 1. Abra o menu  ** Preferências **  do Docker.
 	 1. Clique na guia  ** Daemon ** .
 	 1. No campo **Registros inseguros**, inclua o valor `mycluster.icp:8500` ou o valor para sua instalação do IBM Cloud Private.
 	 
 ![Configurar o docker](images/docker.png)
 	 
 	Se você estiver executando o Linux, execute as etapas a seguir:
 	
 	1. Localize o arquivo  ` daemon.json ` . Por padrão, esse arquivo está localizado em `/etc/docker/daemon.json`. Se o arquivo não existir, crie-o no local padrão. Abra o arquivo em um editor de texto e verifique ou inclua o JSON a seguir.
 	
 	  ```json
 	  {
 	    "insecure-registries": ["mycluster.icp:8500"]
 	  }
 	  ```
 	  
 	1. Execute os comandos a seguir:
 	  ```bash
 	  recarregamento do daemon systemctl
 	  ```
 	  {: pre}
 	  
 	  ```bash
 	  systemctl restart docker
 	  ``` 
 	  {: pre}

### Etapa 2: fazer download das CLIs do IBM Cloud Private e do IBM Cloud
{: #step2}

  1. Faça download e instale a CLI do Helm para cada versão do IBM Cloud Private que você está usando. Consulte a [Documentação do IBM Cloud Private ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/icp_cli.html){: new_window} para obter detalhes.
  
  1. Faça download e instale a CLI do IBM Cloud. Consulte a [Documentação do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html#download_install){: new_window} para obter detalhes.
  
  1. Faça download e instale o plug-in `bx` da CLI do IBM Cloud. Consulte a [Documentação do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/cli/reference/bluemix_cli/extend_cli.html#plug-ins){: new_window} para obter detalhes.
 	
### Etapa 3: fazer download do arquivo de pacote
{: #step3}
 	
Faça download do arquivo de pacote `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz` por meio do [Passport Advantage![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/software/passportadvantage/){: new_window} para sua estação de trabalho local. Esse arquivo contém a imagem inteira do serviço do {{site.data.keyword.cnc_short}}.
  
#### Edite o arquivo do pacote, se necessário
{: edit-pkg-file}

**Importante**: execute o procedimento nesta etapa *somente* se as condições a seguir forem verdadeiras. Se uma ou ambas as condições forem falsas, continue com a [Etapa 4](#step4).

 - Esta etapa se aplica somente ao {{site.data.keyword.cnc_short}} versão 1.0.3 e anterior. Se você estiver instalando o {{site.data.keyword.cnc_short}} versão 1.0.4 ou mais recente, não execute o procedimento nesta etapa.

 - É necessário seguir o procedimento nesta etapa somente quando seu cluster do IBM Cloud Private não está conectado à Internet externa. Se o cluster do IBM Cloud Private tiver acesso à rede para o site do [Passport Advantage![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/software/passportadvantage/){: new_window}, será possível instalar e implementar o {{site.data.keyword.cnc_short}} versão 1.0.3 sem nenhuma modificação no arquivo do pacote.

**Nota**: se você executar o procedimento nesta etapa, poderá haver problemas com upgrades e retrocessos do {{site.data.keyword.cnc_short}} versão 1.0.3. 
 
  1. Em um shell do `bash` ou em um ambiente semelhante, como o Cygwin, extraia o arquivo `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz`:
  
 	```bash
 	cd {path_to_file}
 	```
 	{: pre}
 	
 	```bash
 	tar -xvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz
 	```
 	{: pre}
 	
 	O arquivo extraído inclui um diretório chamado `charts` que contém um arquivo chamado `ibm-watson-compare-comply-prod-1.0.3.tgz`, que é uma versão compactada do gráfico Helm para o {{site.data.keyword.cnc_short}}.
 	
  1. Mude para o diretório `charts` e extraia o arquivo `ibm-watson-compare-prod-1.0.3.tgz`:
  
 	```bash
 	cd charts
 	```
 	{: pre}
 	
 	```
 	tar -xvzf ibm-watson-compare-compley-prod-1.0.3.tgz
 	```
 	{: pre}
 	
 	O arquivo extraído inclui uma série de diretórios e arquivos, incluindo um diretório de nível superior chamado `ibm-watson-compare-UNK-prod`, que contém um diretório chamado `templates`.
 	
  1. Mude para o diretório  ` ibm-watson-compare-UNK-prod/templates ` :
  
 	```bash
 	cd ibm-watson-compare-comply-prod/templates
 	```
 	{: pre}

  1. Localize o arquivo `deployment.yaml` no diretório `ibm-watson-compare-UNK-prod/templates` e abra-o em um editor de texto.
 
  1. Edite o arquivo  ` deployment.yaml `  conforme a seguir:
  
 	** Original **
 	```
 	- name: {{ .Chart.Name }}
 		image: "hyc-icpcontent-docker-local.artifactory.swg-devops.com/cncdev/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	
 	** Atualizado **
 	```
 	- name: {{ .Chart.Name }}
 		image: "mycluster.icp:8500/default/cnc-frontend:v2.3.12"
 		imagePullPolicy: Always
 	```
 	Salve e feche o arquivo  ` deployment.yaml ` .
 	
  1. Retorne ao diretório `charts` e recompacte o arquivo `ibm-watson-compare-comply-prod-1.0.3.tgz`:
  
 	```bash
 	cd ../..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf ibm-watson-compare-compley-prod-1.0.3.tgz ibm-watson-compare-UNK-prod
 	```
 	{: pre}
 	
  1. Retorne ao diretório de nível superior e recompacte o arquivo `IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz`:
  
 	```bash
 	cd ..
 	```
 	{: pre}
 	
 	```bash
 	tar -cvzf IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz charts images manifest.json manifest.yaml
 	```
 	{: pre}
 	
 	 Para referência, a visualização `tree` do conteúdo do arquivo `IBM_WTSN_COMPARE_AN_COMPL_ELEM_ELEM_CL.tar.tar.tar.gz` descompactado é a seguinte. Os arquivos envolvidos neste procedimento são chamados com o `<-----` na coluna da direita.
 	 
 	 ```bash
 	 cd {path_to_archive_file}
 	 ```
 	 {: pre}
 	 
 	 ```bash
 	 tree
 	 
	 .
	 ├── IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz          <-----
     ├── charts                                            <-----
     | UNK -- ibm-watson-compare-UNK-prod
     │   │   ├── Chart.yaml
     | | UNK -- LICENSE
     | | UNK -- LICENSES
     | | | UNK -- LICENSE-Product
     | | UNK -- README.md
     | | UNK -- cnc-banner.png
     | | UNK -- dashboard
     | | | UNK -- alerts.json.tpl
     │   │   │   ├── external-process-logging.json.tpl
     | | | UNK -- frontend-logging.json.tpl
     | | | UNK -- metrics.json.tpl
     | | | UNK -- render-dashboards.sh
     │   │   ├── templates                                 <-----
     | | | UNK -- NOTES.txt
     | | | UNK -- _helpers.tpl
     | | | UNK -- _sch-chart-config.tpl
     | | | UNK -- configmap.yaml
     │   │   │   ├── deployment.yaml                       <-----
     | | | UNK -- ingress.yaml
     | | | UNK -- metrics-service.yaml
     │   │   │   ├── sch-2.6.0
     | | | | UNK -- _config.tpl
     | | | | UNK -- _metadata.tpl
     | | | | UNK -- _names.tpl
     | | | | UNK -- _utils.tpl
     | | | | UNK -- _version.tpl
     | | | UNK -- service.yaml
     | | | UNK -- tests
     │   │   │       └── service-test.yaml
     | | UNK -- values-metadata.yaml
     | | UNK -- values.yaml
     │   └── ibm-watson-compare-comply-prod-1.0.3.tgz      <-----
     Imagens -- imagens
     | UNK -- 304735eab4e4df55b236f9835104cb1058bc50cb5ff7f4ee7d719cb56d2a718b.tar.gz
     │   ├── af80995a4fcfcee364f4f72ba043bf9e73dcafe7fcac384e1cd06bf1c7a52c1f.tar.gz
     | UNK -- ccec10461710cd625cbbacc0f5d5d837a0cc5baf132e0fc39ed42d8264d25062.tar.gz
     UNK -- manifest.json
     └── manifest.yaml 
 	 ```
 	 {: pre}
   
### Etapa 4: preparar um terminal
{: #step4}
 	
Prepare um shell `bash` ou equivalente a ser usado com sua instalação privada do IBM Cloud Private executando os comandos a seguir no shell:
  
  ```bash
  bx pr login -a https://mycluster.icp: 8443--skip-ssl-validation
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

  **Nota**: no primeiro comando na lista precedente, o número da porta `8443` na URL `https://mycluster.icp:8443` representa o valor padrão para a comunicação com o serviço do {{site.data.keyword.cnc_short}} no IBM Cloud Private.

### Etapa 5: criar um segredo de registro de imagem e uma conta de serviço de correção
{: #step5}

Execute os comandos a seguir para ativar todos os componentes necessários para acessar o registro interno do IBM Cloud Private:

```bash
kubectl create secret docker-registry $secret_name --docker-server=mycluster.icp:8500 --docker-username=$username --docker-password=$password --docker-email=admin@admin.com --namespace=default
```
{: pre}

```bash
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "$secret_name"}]}' --namespace=default
```
{: pre}

em que:
  - ` $username `  e  ` $password `  variam de acordo com o cluster. Pergunte ao administrador.
  - `$secret_name` é uma sequência que você especifica [para permitir que os contêineres do Docker se comuniquem com segurança ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/engine/swarm/secrets/){: new_window}.

### Etapa 6: fazer upload do arquivo de pacote para o registro de imagem interna do IBM Cloud Private
{: #step6}

No terminal que você preparou na [Etapa 4](#step4), execute o comando a seguir para fazer upload do arquivo de pacote do Passport Advantage:

```bash
bx pr load-ppa-archive --archive {path_to}/IBM_WTSN_COMPARE_AN_COMPL_ELEM_CL.tar.gz --clustername mycluster.icp --namespace default 
```
{: pre}

### Etapa 7: concluir a instalação

Retorne à página **Catálogo** em seu cluster do IBM Cloud Private, localize e selecione a entrada `ibm-watson-compare-UNK-prod` e clique em **Configurar** para continuar com a instalação.
