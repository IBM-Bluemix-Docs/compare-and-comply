
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

# Notes sur l'édition
{: #release_notes}

Les notes sur l'édition fournissent des informations sur les modifications apportées à l'édition du service {{site.data.keyword.cnc_long}} sur {{site.data.keyword.BluOpenStackDed}}.

**Important :** cette documentation s'applique uniquement au service {{site.data.keyword.cnc_short}} sur {{site.data.keyword.BluOpenStackDed}}. Elle ne s'applique pas aux autres services Watson disponibles sur IBM Cloud Public.

**Remarque :** pour les notes sur l'édition qui affectent tous les services {{site.data.keyword.BluOpenStackDed}}, voir [https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/known_issues.html#issues_nlv){: new_window}.

## Gestion des versions d'API du service
{: #api_versioning}

Les demandes d'API nécessitent un paramètre de version qui prend une date au format `version=AAAA-MM-JJ`. Chaque fois que nous modifions l'API de manière irréversible, nous publions une nouvelle version mineure de l'API.

Envoyez le paramètre de version avec chaque demande d'API. Le service utilise la version d'API correspondant à la date que vous spécifiez ou la toute dernière version avant cette date. N'utilisez pas la valeur en cours par défaut. A la place, spécifiez une date qui correspond à une version compatible avec votre application et ne la modifiez pas tant que votre application n'est pas prête à recevoir une version ultérieure.

La version en cours est `2018-03-23`.

## Modifications
{: #changes}

Les nouvelles fonctions et modifications apportées au service et décrites ci-après sont disponibles.

**Important** : le numéro de version référencé dans les sections suivantes correspond à la version de la charte {{site.data.keyword.cnc_long}} Helm que vous avez déployée sur votre cluster {{site.data.keyword.BluOpenStackDed}}.

### 1.0.5, 2 août 2018
{: #105}

  - Ajout de l'analyse syntaxique des tables, décrit dans [Compréhension du schéma de sortie](/docs/services/compare-and-comply/schema.html#output_schema) et [Compréhension de l'analyse syntaxique des tables](/docs/services/compare-and-comply/tables.html#understanding_tables).


### 1.0.4, 5 juillet 2018
{: #ingress}

**Important** : en raison des modifications décrites dans cette section, vous ne pouvez pas effectuer une mise à niveau vers la version 1.0.4 à partir des versions précédentes de {{site.data.keyword.cnc_short}} à l'aide des procédures de mise à niveau d'{{site.data.keyword.BluOpenStackDed}} standard. Vous devez redéployer {{site.data.keyword.cnc_short}} comme suit :

1.  Retirez le déploiement {{site.data.keyword.cnc_short}} existant comme indiqué dans la rubrique [Retrait d'un déploiement ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}.

1.  Déployez la version 1.0.4 ou ultérieure de {{site.data.keyword.cnc_short}} comme indiqué dans le fichier `README.md` de la charge Helm {{site.data.keyword.cnc_short}} et dans la rubrique [Déploiement du service](/docs/services/compare-and-comply/deploy.html).

**Important :** le redéploiement du service a les conséquences suivantes :

- Le service n'est pas disponible entre le retrait de l'instance précédente et le déploiement de l'instance en cours.
- Si votre code établit des appels API comportant des numéros de port dans les URL des appels, vous devez retirer les numéros de port. Si vous passez un appel API comprenant un numéro de port vers la version 1.0.4 ou ultérieure de {{site.data.keyword.cnc_short}}, l'appel échoue avec un message d'erreur semblable au message suivant :
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**Important :** si vous exécutez une version de {{site.data.keyword.cnc_short}} antérieure à la version 1.0.4, vous devez inclure l'indicateur `:{port_number}` avec l'adresse IP du cluster {{site.data.keyword.BluOpenStackDed}} lorsque vous passez des appels vers le service, comme dans l'exemple suivant :
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Notes supplémentaires

-   Des instructions de déploiement supplémentaires sont désormais fournies dans la rubrique [Déploiement du service](/docs/services/compare-and-comply/deploy.html).
-   Le service {{site.data.keyword.cnc_short}} est désormais intégré au [contrôleur Ingress d'{{site.data.keyword.BluOpenStackDed}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window}. Cette modification vous permet d'appeler les méthodes API du service sans spécifier de numéro de port dans les URL des appels. Voici un appel API typique qui précède l'intégration d'**ingress** :

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  L'appel équivalent dans la version 1.0.4 ou ultérieure est le suivant :

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- Lorsque vous déployez l'édition 1.0.4, il se peut que le message d'erreur suivant s'affiche :

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    Vous pouvez ignorer ce message d'erreur et fermer la fenêtre.

### 1.0.3, 25 mai 2018

- La sortie de la méthode `parse` comprend désormais le tableau `attributes`. Pour plus d'informations, voir les sections [Passer en revue l'analyse](/docs/services/compare-and-comply/getting-started.html#review_analysis) et [Attributs](/docs/services/compare-and-comply/parsing.html#attributes). Vous pouvez utiliser les informations du tableau `attributes` pour rechercher des éléments de document qui font référence à des emplacements, des heures, des dates, des intervalles, des plages de dates et des valeurs et des unités monétaires spécifiques.
- Chaque objet contenu dans les tableaux `types` et `categories` inclut un objet `provenance`. L'objet `provenance` comporte une ou plusieurs clés `id`. Chaque `id` est doté d'une valeur hachée que vous pouvez envoyer à IBM pour fournir des commentaires ou recevoir du support.
- Améliorations apportées à l'exactitude d'analyse syntaxique de PDF et aux performances de l'analyse syntaxique de fichiers PDF volumineux.
- Cette édition est disponible sur {{site.data.keyword.BluOpenStackDed}} version 2.1.0.2 ou ultérieure. Pour plus d'informations sur les exigences relatives à {{site.data.keyword.BluOpenStackDed}}, voir l'entrée de catalogue correspondant au service.
- La valeur `Low` ne fait plus partie des valeurs possibles pour la clé `assurance`.

### 1.0.2, 19 avril 2018

- Ajout de catégories prises en charge. Pour obtenir la liste la plus récente des catégories prises en charge, voir la rubrique [Tableau categories](/docs/services/compare-and-comply/parsing.html#contract_categories).
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  Mises à jour de la documentation, y compris des corrections apportées aux exemples dans la rubrique [Mise en route](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (disponibilité générale), 23 mars 2018

Les remarques suivantes s'appliquent à la disponibilité générale du service {{site.data.keyword.cnc_long}} :

- L'édition de disponibilité générale est disponible uniquement sur la version 2.1.0.2 ou ultérieure d'{{site.data.keyword.BluOpenStackDed}}. Pour plus d'informations sur les exigences relatives à {{site.data.keyword.BluOpenStackDed}}, voir l'entrée de catalogue correspondant au service.

## Remarques générales
{: #general_notes}

- L'API {{site.data.keyword.cnc_short}} sur {{site.data.keyword.BluOpenStackDed}} ne requiert pas d'autorisation contrairement aux API sur IBM Cloud Public.
 - Les PDF doivent être au format texte pour pouvoir faire l'objet d'une analyse syntaxique. Les documents ayant été numérisés ne peuvent pas faire l'objet d'une analyse syntaxique, même si les numérisations ont été traitées par un lecteur optique de caractères.

## Problèmes connus
{: #known_issues}

- La taille maximale d'un fichier PDF pouvant être téléchargé est de 50 Mo.
- Les PDF pour lesquels la sécurité est activée ne peuvent pas faire l'objet d'une analyse syntaxique.
- L'analyse syntaxique des documents avec des présentations de page non standard (par exemple, 2 ou 3 colonnes par page) ne s'effectue pas correctement.
