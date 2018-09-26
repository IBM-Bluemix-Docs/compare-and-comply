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

# Usando a criação de
{: #logging}

## Importando os painéis de criação de log

Para importar os painéis de criação de log para o {{site.data.keyword.cnc_short}} no IBM
Cloud Private, execute as etapas a seguir.

  1. Assegure-se de que tenha extraído e gerado os painéis de criação de log, conforme descrito na [Etapa 1: Fazer download, extrair e renderizar os modelos de painel](/docs/services/compare-and-comply/monitor.html#monitor).

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
`external-process-logging.json` que foram gerados na Etapa 6 do procedimento anterior. Quando solicitado, clique em **Sim, sobrescrever tudo**.
![Prompt Sim, sobrescrever tudo](images/overwrite-all.png)

  1. Os painéis aparecem na guia **Procuras**.
![Guia Painéis nas procuras](images/searches-tab.png)

## Visualizando os Painéis de criação de log
{: #view}

Para visualizar os painéis de criação de log, execute as etapas a seguir.

  1. Navegue para a guia  ** Descobrir ** .

  1. Clique em **Abrir** próximo ao lado superior direito da interface do Kibana.

  1. Selecione o painel que você deseja visualizar. Há dois painéis de criação de log, um para o log de
serviço e outro para o log do processo externo.
![Visualizar painéis de criação de log](images/kibana-dboards.png)

É possível mudar facilmente o intervalo de tempo e a frequência de atualização automática:
![Mudar o intervalo de tempo e a taxa de atualização](images/log-dboard-change.png)

