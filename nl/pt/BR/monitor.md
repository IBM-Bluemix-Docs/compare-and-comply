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

# Instalando, configurando e usando os painéis de monitoramento
{: #monitor}

É possível monitorar o status do {{site.data.keyword.cnc_short}} usando os painéis de monitoramento do IBM Cloud Private. Os painéis de monitoramento usam o Grafana, o Kibana e o Prometheus para exibir as informações detalhadas e customizáveis sobre a instância do {{site.data.keyword.cnc_short}}.

Para obter informações sobre como monitorar o cluster {{site.data.keyword.BluOpenStackDed}}, consulte
[https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html![Ícone de link externo](../../icons/launch-glyph.svg "Íconede link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_metrics/monitoring_service.html){: new_window}.


Antes de usar os painéis, é necessário gerá-los e instalá-los conforme descrito nas etapas a seguir.

## Etapa 1: Fazer download, extrair e renderizar os modelos de painel
{: #render}

Execute as etapas a seguir para preparar os modelos de painel para a instalação.

1. Faça download da imagem do {{site.data.keyword.cnc_short}} no Passport Advantage (PPA). O arquivo é um arquivo tar
compactado com um nome semelhante a `ibm-watson-compare-comply-prod-1.0.5.tar.gz`. O arquivo inclui os modelos de
painel e um script `bash` para renderizar os painéis por meio dos modelos.

1. Descompacte e expanda o arquivo tar:
  ```bash
  mkdir ibm-watson-compare-comply-prod-1.0.5 && tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tar.gz -C ibm-watson-compare-comply-prod-1.0.5
  ``` 
  {: pre}

1. Mude para o diretório `charts` no diretório extraído:
  ```bash
  cd ibm-watson-compare-comply-prod-1.0.5/charts
  ```
  {: pre}

1. Descompacte e expanda o arquivo tar compactado no diretório `charts`:
  ```bash
  tar -xvzf ibm-watson-compare-comply-prod-1.0.5.tgz
  ```
  {: pre}

1. Mude para o diretório  ` dashboard ` . Ele inclui os modelos para os alertas, a criação de log e as
métricas, assim como um script `bash` para gerar os painéis dos modelos.
  ```
  $cd ibm-watson-compare-UNK-prod/dashboard
  
  $árvore.
  UNK -- alerts.json.tpl
  UNK -- external-process-logging.json.tpl
  UNK -- frontend-logging.json.tpl
  UNK -- metrics.json.tpl
  UNK -- render-dashboards.sh

  0 diretórios, 5 arquivos
  ```

1. Executar o script `render-dashboards.sh` para renderizar os modelos. As opções para o script incluem:
  
    - `-v`, `--version {chart_version}`: a versão do gráfico, por exemplo, `1.0.5`.
    - `-h`, `--help`: ajuda do comando Imprimir e saída.
    - `-r`, `--release {release_name}`: o nome da liberação do Helm.
    - `-n`, `--namespace {namespace}`: o espaço de nomes da implementação. O namespace padrão é `default`.

  ```bash
  ./render-dashboards.sh -v 1.0.5 -r my-test-release -n default
  ```
  {: pre}

Os arquivos JSON do painel são gerados no `ibm-watson-compare-comply-prod-1.0.5/charts/ibm-watson-compare-comply-prod/dashboard`.

  ```
  $árvore.
  UNK -- alerts.json
  UNK -- alerts.json.tpl
  ├── external-process-logging.json
  UNK -- external-process-logging.json.tpl
  UNK -- frontend-logging.json
  UNK -- frontend-logging.json.tpl
  UNK -- metrics.json
  UNK -- metrics.json.tpl
  UNK -- render-dashboards.sh

  0 diretórios, 9 arquivos
  ```

## Etapa 2: importar os modelos
{: #import-tpls}

Depois de renderizar os painéis, importe um ou mais deles, conforme descrito nas seções a seguir:

  - [Importando o painel de métricas](metrics.html#import)
  - [Importando os painéis de criação de log](logging.html#import)
  - [ Importando o painel de alertas ](alerts.html#import)

## Etapa 3: Visualizar e Customizar Painéis
{: #use-dboards}

Depois de importar os painéis selecionados, visualize-os e customize-os conforme descrito nas seções a seguir:

  - [Visualizando o painel de métricas](metrics.html#view)
  - [Visualizando os Painéis de criação de log](logging.html#view)
  - [ Incluindo regras de alerta ](alerts.html#add)
