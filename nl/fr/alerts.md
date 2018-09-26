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

# Utilisation des alertes de surveillance
{: #alerts}

Une fois le tableau de bord des alertes importé, vous pouvez définir des alertes Prometheus pour votre instance {{site.data.keyword.cnc_short}} comme indiqué dans les sections suivantes.

## Importation du tableau de bord des alertes et ajout de règles d'alerte
{: #import}

Pour importer le tableau de bord des alertes et ajouter des règles d'alerte au tableau de bord, procédez comme suit :

  1. Vérifiez que vous avez extrait et généré les tableaux de bord des alertes comme indiqué dans [Etape 1 : Télécharger, extraire et afficher le rendu des modèles de tableau de bord](/docs/services/compare-and-comply/monitor.html#monitor).

  1. Connectez-vous à votre cluster IBM Cloud Private.

  1. A partir de l'icône de menu située dans l'angle supérieur gauche, sélectionnez **Configuration -> Objets ConfigMap**.
      ![Icône de menu IBM Cloud Private](images/icp-menu.png) <br />
      ![Menu Configuration -> Objets ConfigMap](images/configmaps.png)

  1. La page **Objets ConfigMap** s'ouvre avec un tableau d'objets Configmap. Dans le tableau, repérez la ligne `alert-rules`. Dans la colonne **Action** de la ligne `alert-rules`, cliquez sur l'icône de menu et sélectionnez **Editer**.
     ![Edition de la ligne alert-rules](images/configmaps-page.png)

  1. Ouvrez le fichier `.../ibm-watson-compare-comply-prod-1.0.0/charts/ibm-watson-compare-comply-prod/dashboard/alerts.json` dans un éditeur de texte et copiez la ligne commençant par `cnc.rules`.

  1. La fenêtre **Editer ConfigMap** s'ouvre. Dans l'objet `data`, ajoutez une virgule à la fin de la dernière ligne de l'objet, puis collez dedans la ligne `cnc.rules` que vous avez copiée à l'étape précédente. <br />
     ![Edition de l'objet ConfigMap](images/edit-configmap.png)

  1. Cliquez sur **Soumettre** dans la fenêtre **Editer ConfigMap**.

## Affichage des règles d'alerte

Pour afficher la liste de règles d'alerte, procédez comme suit :

  1. Accédez au tableau de bord Prometheus sur votre cluster IBM Cloud Private. Le tableau de bord Prometheus se trouve à l'adresse `https://{ICP_cluster_IP_address}:{ICP_cluster_port}/prometheus`.

  1. Cliquez sur l'onglet **Alertes**. Le tableau de bord Prometheus affiche une liste de toutes les règles d'alerte et le nombre d'alertes actives pour chacune d'elles. <br />
    ![Alertes Prometheus](images/prometheus-dboard.png)

## Ajout de notifications d'alerte

Vous pouvez ajouter des notifications d'alerte pour un certain nombre de systèmes de pagination, y compris Slack, PagerDuty, HipChat, les courriers électroniques, etc. Prometheus fournit la prise en charge des notifications comme indiqué sur les sites suivants :

 - [Configuration des alertes Prometheus ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://prometheus.io/docs/alerting/configuration/){: new_window}
 - [Exemples de notification Prometheus ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://prometheus.io/docs/alerting/notification_examples/){: new_window}

Pour créer un récepteur de notification pour {{site.data.keyword.cnc_short}} sur IBM Cloud Private, procédez comme suit :
{: #create-notification-receiver}

  1. Connectez-vous à votre cluster IBM Cloud Private.

  1. A partir de l'icône de menu située dans l'angle supérieur gauche, sélectionnez **Configuration -> Objets ConfigMap**. <br />
      ![Icône de menu IBM Cloud Private](images/icp-menu.png) <br />
      ![Menu Configuration -> Objets ConfigMap](images/configmaps.png)

  1. La page **Objets ConfigMap** s'ouvre avec un tableau d'objets Configmap. Dans le tableau, repérez la ligne `monitoring-prometheus-alertmanager`. Dans la colonne **Action** de la ligne `monitoring-prometheus-alertmanager`, cliquez sur l'icône de menu et sélectionnez **Editer**.

  1. La fenêtre **Editer ConfigMap** s'ouvre. Dans l'objet `data`, entrez les nouvelles configurations de récepteur.
     ![Edition de l'objet ConfigMap](images/prom-alert-edit.png)

  1. Cliquez sur **Soumettre** dans la fenêtre **Editer ConfigMap**.

### Exemples

Pour créer une notification Slack, procédez comme suit :

  1. Vérifiez que le canal Slack cible existe. S'il n'existe pas, créez-le. Pour plus d'informations, voir la [documentation Slack sur la création d'un canal ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://get.slack.help/hc/en-us/articles/201402297-Create-a-channel){: new_window}.

  1. Procurez-vous ou créez le webhook pour le canal Slack. Pour plus d'informations, voir la [documentation Slack pour les webHooks ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack){: new_window}.

  1. Ouvrez le Configmap `monitoring-prometheus-alertmanager` dans l'éditeur de ConfigMap, comme illustré dans la rubrique [Ajout de notifications d'alerte](#create-notification-receiver).

  1. Mettez à jour l'objet `data` dans le ConfigMap comme suit :
    ```
    "data": {
      "alertmanager.yml": "global: \n  slack_api_url: '{WebHook_URL_for_Slack_channel}' \nreceivers: \n  - name: default-receiver \n    slack_configs: \n    - channel: '#{Slack_channel}' \n      send_resolved: true \nroute: \n  receiver: default-receiver \n  routes: \n  - match: \n    severity: critical \n   receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. Dans la fenêtre **Editer ConfigMap**, cliquez sur **Soumettre**.

Pour créer une notification PagerDuty, procédez comme suit :

  1. Vérifiez que le service PagerDuty existe. S'il n'existe pas, créez-le. Pour plus d'informations, voir la [documentation PagerDuty![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://v2.developer.pagerduty.com/docs){: new_window}.

  1. Obtenez la clé d'intégration PagerDuty en ajoutant l'intégration Prometheus. Pour plus d'informations, voir la [documentation de l'API PagerDuty![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://v2.developer.pagerduty.com/docs/events-api){: new_window}.

  1. Ouvrez le Configmap `monitoring-prometheus-alertmanager` dans l'éditeur de ConfigMap, comme illustré dans la rubrique [Ajout de notifications d'alerte](#create-notification-receiver).

  1. Mettez à jour l'objet `data` dans le ConfigMap comme suit :
    ```
    "data": {
      "alertmanager.yml": "global:\nreceivers:\n  - name: default-receiver\n    pagerduty_configs:\n    - service_key: ' {PagerDuty_integration_key}'\nroute:\n  receiver: default-receiver\n  routes:\n  - match:\n      severity: critical\n    receiver: default-receiver"
    }
    ```
    {: codeblock}

  1. Dans la fenêtre **Editer ConfigMap**, cliquez sur **Soumettre**.
  
