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

# Usando alertas de monitoramento
{: #alerts}

É possível configurar os alertas do Prometheus para a instância do {{site.data.keyword.cnc_short}} depois de importar
o painel de alertas, conforme descrito nas seções a seguir.

## Importando o painel de alertas e incluindo as regras de alerta
{: #import}

Para importar o painel de alertas e incluir as regras de alerta no painel, execute as etapas a seguir.

  1. Assegure-se de que tenha extraído e gerado os painéis de alertas, conforme descrito na
[Etapa 1: Fazer download, extrair e renderizar os modelos de
painel](/docs/services/compare-and-comply/monitor.html#monitor).

  1. Efetue login em seu cluster do ICP.

  1. No ícone Menu no canto superior esquerdo, selecione **Configuração -> ConfigMaps**.
![Ícone de Menu do IBM Cloud Private](images/icp-menu.png) <br />
      ![Configuration -> ConfigMaps menu](images/configmaps.png)

  1. A página **ConfigMaps** é aberta para exibir uma tabela de configmaps. Na tabela, localize a linha rotulada `alert-rules`. Na coluna **Ação** da linha `alert-rules`, clique no ícone de menu e selecione **Editar**. ![Editar alert-rules](images/configmaps-page.png)

  1. Abra o arquivo `.../ibm-watson-compare-UNK-prod-prod-1.0.0/charts/ibm-watson-compare-UNK-prod/dashboard/alerts.json` em um editor de texto e copie a linha que começa com `cnc.rules`.

  1. A janela  ** Editar ConfigMap **  é aberta. No objeto `data`, inclua uma vírgula no término da última linha do objeto e, em seguida, cole na linha `cnc.rules` que você copiou na etapa anterior. <br />
     ![Editar o ConfigMap](images/edit-configmap.png)

  1. Clique em **Enviar** na janela **Editar ConfigMap**.

## Visualizando regras de alerta

Para visualizar a lista de regras de alerta, execute as etapas a seguir.

  1. Navegue para o painel Prometheus em seu cluster do IBM Cloud Private. O painel Prometheus está localizado em `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus`.

  1. Clique na guia **Alertas**. O painel Prometheus exibe uma lista de todas as regras de alerta e o número de alertas ativos para cada uma delas. <br />
    ![Alertas do Prometheus](images/prometheus-dboard.png)

## Incluindo notificações de alerta

É possível incluir notificações de alerta para vários sistemas de paginação, incluindo Slack, PagerDuty,
HipChat, e-mail e outros. O Prometheus fornece suporte para notificações, conforme documentado nos sites a
seguir:

 - [Documentação da configuração de alertas do Prometheus ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Documentação de Exemplos de Notificação do Prometheus ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

Para criar um receptor de notificação para o {{site.data.keyword.cnc_short}} no IBM Cloud
Private, execute as etapas a seguir.
{: #create-notification-receiver}

  1. Efetue login em seu cluster do ICP.

  1. No ícone Menu no canto superior esquerdo, selecione **Configuração -> ConfigMaps**. <br />
      ![Ícone de Menu do IBM Cloud Private](images/icp-menu.png) <br />
      ![Configuration -> ConfigMaps menu](images/configmaps.png)

  1. A página **ConfigMaps** é aberta para exibir uma tabela de configmaps. Na tabela, localize a linha rotulada `monitoring-prometheus-alertmanager`. Na coluna **Ação** da linha `monitoring-prometheus-alertmanager`, clique no ícone de menu e selecione **Editar**.

  1. A janela  ** Editar ConfigMap **  é aberta. No objeto `data`, insira as novas configurações do receptor. ![Editar o ConfigMap](images/prom-alert-edit.png)

  1. Clique em **Enviar** na janela **Editar ConfigMap**.

### Exemplos

Para criar uma notificação do Slack, execute as etapas a seguir.

  1. Verifique se o canal Slack de destino existe. Se não existir, crie-o. Consulte a [Documentação do Slack para criar um canal ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window} para obter detalhes.

  1. Obtenha ou crie o WebHook para o canal do Slack. Consulte a [Documentação do Slack para o WebHooks ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window} para obter detalhes.

  1. Abra o ConfigMap `monitoring-prometheus-alertmanager` no editor de ConfigMap, conforme descrito em [Incluindo notificações de alerta](#create-notification-receiver).

  1. Atualize o objeto `data` no ConfigMap, conforme a seguir:
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. Na janela **Editar ConfigMap**, clique em **Enviar**.

Para criar uma notificação do PagerDuty, execute as etapas a seguir.

  1. Verifique se o serviço PagerDuty existe. Se não existir, crie-o. Consulte a [Documentação do PagerDuty ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://v2.developer.pagerduty.com/docs){: new_window} para obter detalhes.

  1. Obtenha a chave de integração do PagerDuty ao incluir a integração do Prometheus. Consulte a [Documentação da API do PagerDuty ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://v2.developer.pagerduty.com/docs/events-api){: new_window} para obter detalhes.

  1. Abra o ConfigMap `monitoring-prometheus-alertmanager` no editor de ConfigMap, conforme descrito em [Incluindo notificações de alerta](#create-notification-receiver).

  1. Atualize o objeto `data` no ConfigMap, conforme a seguir:
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. Na janela **Editar ConfigMap**, clique em **Enviar**.
  
