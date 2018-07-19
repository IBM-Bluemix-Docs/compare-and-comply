---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-22"

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

# Mise en route
{: #getting_started}

Ce court tutoriel a pour objectif de vous présenter {{site.data.keyword.cnc_long}} sur IBM Cloud Private et de passer en revue la procédure consistant à faire l'analyse syntaxique d'un contrat pour identifier les pièces qui le constituent, leur nature, les parties prenantes concernées et les catégories identifiées. 

## Avant de commencer
{: #before-you-begin}

Avant de pouvoir utiliser le service {{site.data.keyword.cnc_short}}, vous devez installer l'interface de ligne de commande IBM Cloud Private et vous connecter à votre cluster IBM Cloud Private en procédant comme indiqué dans la rubrique [Installation de l'interface de ligne de commande IBM Cloud Private](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html).
 
## Etape 1 : Identifier le contenu
{: #identify_content}

Identifiez les documents à analyser. {{site.data.keyword.cnc_short}} a été conçu pour analyser des documents contractuels <!-- and regulatory -->répondant aux critères suivants : 

- Les fichiers à analyser doivent être au format PDF.
- Le contenu PDF doit être sous forme de texte. Les documents ayant été numérisés ne peuvent pas faire l'objet d'une analyse syntaxique, même si les numérisations ont été traitées par un lecteur optique de caractères. 

  **Remarque :** vous pouvez identifier un PDF au format texte en ouvrant le document dans un afficheur PDF et en utilisant l'outil de **sélection de texte** pour sélectionner un seul mot. Si vous ne pouvez pas sélectionner un seul mot dans le document, le fichier ne peut pas faire l'objet d'une analyse syntaxique. 

- La taille des fichiers ne doit pas dépasser 50 Mo.
- Les PDF sécurisés (mot de passe requis pour les ouvrir) et les PDF dont l'édition est restreinte (mot de passe requis pour les éditer) ne peuvent pas être analysés.

## Etape 2 : Faire une analyse syntaxique d'un contrat
{: #parse_contract}

Dans un interpréteur de commandes `bash` ou un environnement équivalent, tel que Cygwin, utilisez la méthode `POST /v1/parse` pour faire une analyse syntaxique de votre contrat. Remplacez `{ICP_IP_address}` par l'adresse IP de votre cluster IBM Cloud Private. Remplacez `{PDF_file}` par le chemin d'accès au fichier PDF qui doit faire l'objet d'une analyse syntaxique. 

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**Important :** si vous exécutez une version de {{site.data.keyword.cnc_short}} antérieure à la version 1.0.4, vous devez inclure l'indicateur `:{port_number}` avec l'adresse IP du cluster IBM Cloud Private lorsque vous passez des appels vers le service, comme suit :

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

Par exemple :

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

Pour plus d'informations, voir la [note sur l'édition****](/docs/services/compare-and-comply/relnotes.html#ingress). 

La méthode renvoie un objet JSON contenant les éléments suivants :

 - Une conversion HTML du fichier PDF source. 
 - Le titre du document extrait. 
 - Un tableau `elements` dans lequel sont définis les éléments constitutifs du contrat. 
 - Un tableau `parties` dans lequel sont définies les entités ayant été identifiées comme parties prenantes. 
 
**Important** : l'API {{site.data.keyword.cnc_short}} sur IBM Cloud Private ne requiert pas d'autorisation contrairement aux API sur IBM Cloud Public. 

## Etape 3 : Passer en revue l'analyse
{: #review_analysis}

Chaque objet du tableau `elements` décrit un élément du contrat ayant été identifié par {{site.data.keyword.cnc_short}}. Le code suivant représente un élément typique :

```
{
  "sentence_text": "If the parties are unable to agree on the License Price within thirty (30) days after Richmond Enterprises provides the written notice of its exercise of the Optional Patent License to Savage Narwhal Studios in Section 2.3(a), such FMV Dispute shall, at either party's request, be resolved solely and exclusively by final, binding and confidential arbitration to be filed and the decision rendered in New York, New York (with hearings at the request of either party to be held in San Francisco, California or other mutually agreeable place convenient for the parties) in accordance with the Commercial Arbitration Rules of the American Arbitration Association (\" AAA \"), including as supplemented by the Procedures for Large, Complex Commercial Disputes.",
  "attributes": [
	{
		"type": "Location",
		"text": "New York",
		"attribute": {
			"begin": 58372,
			"end": 58380
		}
	},
	{
		"type": "Location",
		"text": "New York",
		"attribute": {
			"begin": 58382,
			"end": 58390
		}
	},
	{
		"type": "Location",
		"text": "San Francisco",
		"attribute": {
			"begin": 58451,
			"end": 58464
		}
	},
	{
		"type": "Location",
		"text": "California",
		"attribute": {
			"begin": 58466,
			"end": 58476
		}
	}
],
"categories": [
	{
		"label": "Communication",
		"assurance": "High",
		"provenance": [
			{
				"id": "C7xhbsepUodh09zmJdUXSvYZCdixx00wFyCZuAnTujok="
			}
		]
	},
	{
		"label": "Dispute Resolution",
		"assurance": "High",
		"provenance": [
			{
				"id": "Ck8vgUWOj41OutOOLJ38b2Q7jOj3F30ABGaGLKKxppFA="
			},
			{
				"id": "CdPeg8mAxM5YIsdpzzaluDg7jOj3F30ABGaGLKKxppFA="
			}
		]
	},
	{
		"label": "Intellectual Property",
		"assurance": "High",
		"provenance": [
			{
				"id": "Cor/mgcf1UE/zmsKm68M6+a9LSRCpcKe8EWCUdwsjrgs="
			}
		]
	}
],
"types": [
	{
		"label": {
			"nature": "Obligation",
			"party": "All Parties"
		},
		"assurance": "High",
		"provenance": [
			{
				"id": "NNpSqaNkY2zHtxI6Zh78NaZccVNtutrQxISkzdnaVjx0="
			},
			{
				"id": "PlyERkjg5is36RpFjVUFXp69eDmGmCxLCXRs1sDMDUCo="
			}
		]
	}
],
"sentence": {
	"begin": 57998,
	"end": 58952
	}
}
```

Le tableau element comprend cinq sections importantes :
 - `sentence_text` : texte qui a été analysé. 
 - `attributes` : tableau qui recense un ou plusieurs attributs de l'élément. Les objets actuellement pris en charge dans le tableau`attributes` sont notamment `Location` (emplacement géographique ou région référencé par l'élément), `DateTime` (date, heure, plage de dates ou intervalle spécifié par l'élément) et `Currency` (valeurs et unités monétaires). 
 - `categories` : tableau qui recense les catégories fonctionnelles auxquelles la phrase identifiée appartient ; en d'autres termes, il s'agit du sujet traité dans la phrase. 
 - `types` : tableau qui décrit ce qu'est l'élément et les parties prenantes concernées. Ce tableau comprend un ou plusieurs ensembles de clés `nature` (effet de la phrase sur la partie prenante `party` identifiée) et de clés `party` (partie prenante concernée par la phrase). 
 - `sentence` : objet qui décrit à quel endroit l'élément a été trouvé dans le document HTML converti. Il contient une valeur de caractère `start` et une valeur de caractère `end`. 

**Remarque** : certaines phrases ne sont associées à aucun type ou n'appartiennent à aucune catégorie. Dans ce cas, le service renvoie les tableaux `types` et `categories` sous forme d'objets vides. 

**Remarque :** certains phrases traitent de plusieurs sujets. Dans ce cas, le service renvoie plusieurs ensembles d'objets `types` et`categories`. 

**Remarque** : certaines phrases ne contiennent aucun attribut identifiable. Dans ce cas, le service renvoie le tableau `attributes` sous forme d'objets vides. 

De plus, les parties prenantes identifiées sont définies dans le tableau `parties`. Le tableau `parties` figure après le tableau `elements` dans la sortie JSON. 

```
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

Le tableau `parties` comprend deux sections importantes :

 - `party` : texte identifié en tant que partie prenante dans le document.
 - `role` : rôle de la partie prenante identifiée. Les rôles varient en fonction du sous-domaine. Voir [la documentation sur le sous-domaine spécifié pour connaître les rôles possibles](/docs/services/compare-and-comply/parsing.html#contract_parties). Les parties prenantes qui ne peuvent pas être identifiées comme ayant un rôle spécifique sont répertoriées avec la valeur `unknown`. 

## Etapes suivantes
{: #next_steps}

Vous avez soumis un contrat à une analyse syntaxique afin d'identifier la nature, les parties prenantes et les catégories des éléments constitutifs du document. Vous pouvez utiliser cette analyse syntaxique pour comprendre et faire exécuter rapidement le contrat analysé. Les étapes suivantes consistent à :

 - Comprendre les types et les catégories
 - Passer en revue les options d'analyse syntaxique


