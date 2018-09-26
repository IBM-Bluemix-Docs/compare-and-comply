---

copyright:
years: 2018
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

# Compréhension du schéma de sortie
{: #output_schema}

Une fois qu'un document a été analysé et enrichi par le service, ce dernier fournit la sortie JSON dans le schéma suivant.

```
{
    "document_text": string,
    "document_title": string,
    "elements": [
        {
          "sentence": { "begin": int, "end": int },
          "sentence_text": string,
          "types": [
              {
                "label": { "nature": string, "party": string },
                "assurance": "High" or "Low",
                "provenance": [
                    {
                      "id": string
                    }
                    ...
                ]
              }
              ...
          ],
"categories": [
	{
                "label": string,
                "assurance": "High" or "Low",
                "provenance": [
                   {
                     "id": string
                   }
                ]
              }
              ...
          ]
	  "attributes": [
              {
              	"type": string,
              	"text": string,
              	"attribute": {"begin": int, "end": int}
              }
          ]
        }
        ...
    ], 
    "tables": [
      {
        "table": {
          "begin": int,
          "end": int
        },
        "table_text": string,
        "section_title": {
          "begin": int,
          "end": int
        },
        "section_title_text": string,
        "table_headers" : [
          {
            "id": string,
            "cell": {
              "begin": int,
              "end": int
            },
            "cell_text": string,
            "row_index_min": int,
            "row_index_max": int,
            "column_index_min"  int,
            "column_index_max": int
          },
          ...
        ],
        "column_headers": [
         {
          "id": string,
          "cell": {
            "begin": int,
            "end": int
          },
          "cell_text": string,
          "row_index_min": int,
          "row_index_max": int,
          "column_index_min": int,
          "column_index_max": int
        },
        ...
      ],
      "row_headers": [
        {
            "id": string,	
            "cell": {
              "begin": int,
              "end": int
            },
            "cell_text": string,
            "row_index_min": int,
            "row_index_max": int,
            "column_index_min": int,
            "column_index_max": int
            },
            ...
          ],
          "body_cells": [
          {
            "id": string,
            "cell": {
              "begin": int,
              "end": int
            },
            "cell_text": string,
            "row_index_min": int,
            "row_index_max": int,
            "column_index_min": int,
            "column_index_max": int,
            "row_header_ids": [string],
            "row_header_texts": [string],
            "column_header_ids": [string],
          "column_header_texts": [string]
          },
        ...  
        ]
      },
      ...
    ]
}
```

Le schéma est organisée comme suit. 

 - `document_text` : texte intégral du document analysé au format HTML.
 - `document_title` : titre du document analysé. Si le service ne détecte pas de titre, cet élément a la valeur `null`.
 - `elements` : tableau des éléments du document détectés par le service.
    - `sentence` : emplacement de la phrase tel que défini par les index de début et de fin.
    - `sentence_text` : texte de la phrase.
    - `types` : tableau qui décrit ce qu'est l'élément et les parties prenantes concernées. Ce tableau comprend un ou plusieurs ensembles de clés `nature` (effet de la phrase sur la partie prenante `party` identifiée) et de clés `party` (partie prenante concernée par la phrase).
      - `label` : tableau des éléments `nature` et `party`.
        - `nature` : type d'action requis par la phrase. Les valeurs possibles sont `Definition`, `Disclaimer`, `Exclusion`, `Obligation` et `Right`.
        - `party` : chaîne qui identifie la partie prenante à qui la phrase s'applique.
      - `assurance` : certitude que le type détecté est applicable. Les valeurs possibles sont `High` et `Low`.
      - `provenance` : chaque objet des tableaux `types` et `categories` comprend un objet `provenance`. L'objet comporte une ou plusieurs clés `id`.
        - `id` : valeur hachée que vous pouvez envoyer à IBM pour fournir des commentaires ou recevoir du support.
    - `categories` : tableau qui recense les catégories fonctionnelles auxquelles la phrase appartient ; en d'autres termes, il s'agit du sujet traité dans la phrase.
      - `label` : catégorie à laquelle la phrase appartient. Voir [Catégories dans Compréhension de l'analyse syntaxique de contrat](/docs/services/compare-and-comply/parsing.html#contract_categories) pour la liste des catégories.
      - `assurance` : certitude que la catégorie détectée est applicable. Les valeurs possibles sont `High` et `Low`.
      - `provenance` : chaque objet des tableaux `types` et `categories` comprend un objet `provenance` qui inclut une ou plusieurs clés `id`.
        - `id` : valeur hachée que vous pouvez envoyer à IBM pour fournir des commentaires ou recevoir du support.
    - `attributes` : tableau d'éléments `type`, `text` et `attribute`.
      - `type` : type d'attribut. Les valeurs possibles sont `Location`, `DateTime` et `Currency`.
      - `text` : texte associé à l'attribut.
      - `attribute` : emplacement de l'attribut tel que défini par les index `begin` et `end`.


  - `tables` : tableau qui définit les tables identifiées par le service.
    - `table` : emplacement du tableau en cours tel que défini par les décalages `begin` et `end` dans le document d'entrée.
    - `table_text` : contenu textuel du tableau en cours issu du document d'entrée sans contenu de balisage associé.
    - `column_headers` : tableau de cellules de niveau colonne, toutes applicables en tant qu'en-tête à d'autres cellules de la même colonne, du tableau en cours. Chaque en-tête de colonne est défini en tant que collection des éléments suivants :
      - `id` : valeur de chaîne au format `columnHeader-x-y`, où `x` et `y` sont les décalages de début et de fin de cette cellule d'en-tête de colonne dans le document d'entrée.
      - `cell` : emplacement de cette cellule dans le tableau en cours tel que défini par les décalages `begin` et `end` dans le document d'entrée.
      - `cell_text` : contenu textuel de cette cellule issu du document d'entrée sans contenu de balisage associé. 
      - `row_index_min` : index de début de l'emplacement `row` de cette cellule dans le tableau en cours.
      - `row_index_max` : index de fin de l'emplacement `row` de cette cellule dans le tableau en cours.
      - `column_index_min` : index de début de l'emplacement `column` de cette cellule dans le tableau en cours.
      - `column_index_max` : index de fin de l'emplacement `column` de cette cellule dans le tableau en cours.
    - `row_headers` : tableau de cellules de niveau ligne, toutes applicables en tant qu'en-tête à d'autres cellules de la même ligne, du tableau en cours. Chaque en-tête de ligne est défini en tant que collection des éléments suivants :
      - `id` : valeur de chaîne au format `rowHeader-x-y`, où `x` et `y` sont les décalages de début et de fin de cette cellule d'en-tête de ligne dans le document d'entrée.
      - `cell` : emplacement de cette cellule dans le tableau en cours tel que défini par les décalages `begin` et `end` dans le document d'entrée.
      - `cell_text` : contenu textuel de cette cellule issu du document d'entrée sans contenu de balisage associé. 
      - `row_index_min` : index de début de l'emplacement `row` de cette cellule dans le tableau en cours.
      - `row_index_max` : index de fin de l'emplacement `row` de cette cellule dans le tableau en cours.
      - `column_index_min` : index de début de l'emplacement `column` de cette cellule dans le tableau en cours.
      - `column_index_max` : index de fin de l'emplacement `column` de cette cellule dans le tableau en cours.
    - `body_cells` : tableau de cellules qui ne sont ni des cellules d'en-tête de tableau, d'en-tête de colonne ou d'en-tête de ligne, du tableau en cours avec associations d'en-tête de colonne et de ligne correspondantes. Chaque cellule de corps est définie en tant que collection des éléments suivants :
      - `id` : valeur de chaîne au format `bodyCell-x-y`, où `x` et `y` sont les décalages de début et de fin de cette cellule de corps dans le document d'entrée.
      - `cell` : emplacement de cette cellule dans le tableau en cours tel que défini par les décalages `begin` et `end` dans le document d'entrée.
      - `cell_text` : contenu textuel de cette cellule issu du document d'entrée sans contenu de balisage associé. 
      - `row_index_min` : index de début de l'emplacement `row` de cette cellule dans le tableau en cours.
      - `row_index_max` : index de fin de l'emplacement `row` de cette cellule dans le tableau en cours.
      - `column_index_min` : index de début de l'emplacement `column` de cette cellule dans le tableau en cours.
      - `column_index_max` : index de fin de l'emplacement `column` de cette cellule dans le tableau en cours.
      - `row_header_ids` : tableau de valeurs, chacune étant la valeur `id` d'un en-tête de ligne applicable à cette cellule de corps.
      - `row_header_texts` : tableau de valeurs, chacune étant la valeur `cell_text` d'un en-tête de ligne applicable à cette cellule de corps.
      - `column_header_ids` : tableau de valeurs, chacune étant la valeur `id` d'un en-tête de colonne applicable à cette cellule de corps.
      - `column_header_texts` : tableau de valeurs, chacune étant la valeur `cell_text` d'un en-tête de colonne applicable à cette cellule de corps.
    - `section_title` : s'il est identifié, emplacement d'un titre de section contenant le tableau en cours, tel que défini par les décalages de début et de fin dans le document d'entrée d'origine. Vide si aucun titre de section n'est identifié.
    - `section_title_text` : s'il est identifié, contenu textuel du titre de section contenant le tableau en cours, sans contenu de balisage associé. Vide si aucun titre de section n'est identifié.
    - `table_headers` : tableau de cellules de niveau tableau applicables en tant qu'en-tête à toutes les autres cellules du tableau en cours. Chaque en-tête du tableau est défini en tant que collection des éléments suivants :
      - `id` : valeur de chaîne au format `tableHeader-x-y` où `x` et `y` sont les décalages de début et de fin de la valeur de cellule dans le document d'entrée d'origine.
      - `cell` : emplacement de la cellule d'en-tête de table dans le tableau en cours tel que défini par les décalages de début et de fin dans le document d'entrée d'origine.
      - `cell_text` : contenu textuel de cette cellule issu du document d'entrée sans contenu de balisage associé. 
      - `row_index_min` : index de début de cet emplacement de ligne de cellule dans le tableau en cours.
      - `row_index_max` : index de fin de cet emplacement de ligne de cellule dans le tableau en cours.
      - `column_index_min` : index de début de l'emplacement `column` de cette cellule dans le tableau en cours.
      - `column_index_max` : index de fin de cet emplacement de colonne de cellule dans le tableau en cours.
      
**Remarque :**
  - Les valeurs d'index de ligne et de colonne sont à base zéro, de sorte qu'elles commencent à `0`.
  - Plusieurs valeurs des tableaux d'éléments `row_header_ids` et `row_header_texts` indiquent une hiérarchie des en-têtes de ligne.
  - Plusieurs valeurs des tableaux d'éléments `column_header_ids` et `column_header_texts` indiquent une hiérarchie des en-têtes de colonne.
