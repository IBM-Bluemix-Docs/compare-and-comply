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

# Usando a criação de
{: #using-logging}

## Instalando e executando os painéis de criação de log

Para instalar o painel de criação de log para o {{site.data.keyword.cnc_short}}, execute as
etapas a seguir.

  1. Faça download do arquivo do Passport Advantage (PPA) para o {{site.data.keyword.cnc_short}}. O arquivo é um arquivo tar compactado com um nome semelhante a `ibm-watson-compare-comply-prod-1.0.0.tar.gz`. O arquivo inclui os modelos de painel de criação de log e um script `bash` para renderizar os
painéis por meio dos modelos.

  1. Descompacte e expanda o arquivo PPA:
    ```bash
    $ mkdir ibm-watson-compare-comply-prod-1.0.0 && tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tar.gz -C ibm-watson-compare-comply-prod-1.0.0
    ```
    {: codeblock}

  1. Mude para o diretório `charts` no diretório extraído:
    ```bash
    $ cd ibm-watson-compare-comply-prod-1.0.0/charts
    ```

  1. Descompacte e expanda o arquivo tar compactado no diretório `charts`:
    ```bash
    $ tar -xvzf ibm-watson-compare-comply-prod-1.0.0.tgz
    ```

  1. Mude para o diretório  ` dashboard ` . Inclui modelos para métricas e criação de log e um script de bash para gerar painéis dos modelos.

    ```bash
    $ cd ibm-watson-compare-comply-prod/dashboard

    $árvore.
    UNK -- alerts.json.tpl
    UNK -- external-process-logging.json.tpl
    UNK -- frontend-logging.json.tpl
    UNK -- metrics.json.tpl
    UNK -- render-dashboards.sh

    0 diretórios, 5 arquivos
    ```

  1. Executar o script `render-dashboards.sh` para renderizar os modelos. As opções para o script incluem:
  
    -  `-v, --version {chart_version}`: a versão do diagrama, por exemplo, `1.0.0`.
    -  `-h, --help`: imprimir a ajuda do comando e sair.
    -  `-r, --release {release_name}`: o nome da liberação do Helm.
    -  `-n, --namespace {namespace}`: o espaço de nomes da implementação. O namespace padrão é `default`.

    ```bash
    $ ./render-dashboards.sh -v 1.0.0 -r my-test-release -n default
    The dashboard JSON files are generated under /Users/{user}/Downloads/ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard.

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

## Importando os painéis de criação de log

Para importar os painéis de criação de log para o {{site.data.keyword.cnc_short}} no IBM
Cloud Private, execute as etapas a seguir.

  1. Efetue login em seu cluster do IBM Cloud Private.

  1. No ícone Menu no canto superior esquerdo, selecione **Plataforma -> Criação de log**. <br />
    ![Ícone de Menu do IBM Cloud Private](images/icp-menu.png) <br />
    ![Platform -> Logging menu](images/icp-logging.png)

  1. Clique em **Gerenciamento** no lado esquerdo da interface do Kibana. <br />
    ![Kibana interface](images/kibana.png)

  1. Selecione a guia **Objetos salvos**.
![Guia Objetos salvos](images/saved-obj.png)

  1. Selecione a guia **Procuras** e clique em **Importar**.
![Guia Importar das procuras](images/searches-import.png)

  1. Importe individualmente os arquivos `frontend-logging.json` e
`external-process-logging.json` que foram gerados na Etapa 6 do procedimento anterior.
Quando solicitado, clique em **Sim, sobrescrever tudo**.
![Prompt Sim, sobrescrever tudo](images/overwrite-all.png)

  1. Os painéis aparecem na guia **Procuras**.
![Guia Painéis nas procuras](images/searches-tab.png)

## Visualizando os Painéis de criação de log

Para visualizar os painéis de criação de log, execute as etapas a seguir.

  1. Navegue para a guia  ** Descobrir ** .

  1. Clique em **Abrir** próximo ao lado superior direito da interface do Kibana.

  1. Selecione o painel que você deseja visualizar. Há dois painéis de criação de log, um para o log de
serviço e outro para o log do processo externo.
![Visualizar painéis de criação de log](images/kibana-dboards.png)

É possível mudar facilmente o intervalo de tempo e a frequência de atualização automática:
![Mudar o intervalo de tempo e a taxa de atualização](images/log-dboard-change.png)

