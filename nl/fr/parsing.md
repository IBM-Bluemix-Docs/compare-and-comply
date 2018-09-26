---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-27"

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

# Compréhension de l'analyse syntaxique de contrat
{: #contract_parsing}

{{site.data.keyword.cnc_short}} renvoie des contrats ayant fait l'objet d'une analyse syntaxique avec une analyse de chaque élément identifié.

Les sections suivantes expliquent de quelle manière l'analyse est fournie par l'objet JSON renvoyé.

## Types
{: #contract_types}

Le tableau `types` inclut un certain nombre d'objets, contenant chacun les clés `nature` et `party` dont les valeurs identifient un distique pour l'élément. Les tableaux suivants recensent les valeurs possibles pour les clés `nature` et `party` :

La valeur `nature` répertorie les types d'action requis par la phrase.

| `nature`         |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Definition`      |Cet élément permet de clarifier un terme, une relation ou d'autres objets similaires. Aucune action n'est requise pour satisfaire l'élément, et aucune partie prenante n'est concernée.|
|`Disclaimer`      |L'objet `party` de l'élément n'est pas tenu de respecter les termes spécifiés par l'élément mais il ne lui est pas interdit de le faire.|
|`Exclusion`       |L'objet `party` de l'élément ne respectera pas les termes spécifiés par l'élément.|
|`Obligation`      |L'objet `party` de l'élément est tenu de respecter les termes spécifiés par l'élément.|
|`Right`           |L'objet `party` de l'élément est assuré de recevoir les termes spécifiés par l'élément.|
|`Recommendation`  |L'objet `party` de l'élément n'est pas tenu de respecter les termes spécifiés par l'élément, mais il est fermement invité à le faire.|

## Parties prenantes
{: #contract_parties}

Le tableau `parties` spécifie les participants mentionnés dans le contrat. Chaque objet `party` identifié recense la partie prenante identifiée par son nom et est mis en correspondance avec un objet `role` qui classifie le rôle qui lui est associé. Les valeurs `role` pouvant être renvoyées pour des contrats sont notamment les suivantes :

| `role`           |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Buyer`           |Partie prenante chargée du paiement des biens ou des services mentionnés dans le contrat.|
|`End User`        |Partie prenante qui va interagir avec les biens ou les services fournis ; se distingue explicitement de l'objet role `Buyer`.|
|`None`            |Aucune partie prenante n'a été identifiée pour l'élément. Cette valeur est toujours associée à l'objet nature `Definition`.|
|`Supplier`        |Partie prenante chargée de la distribution des biens ou des services mentionnés dans le contrat.|

## Catégories
{: #contract_categories}

Le tableau `categories` définit le sujet traité dans la phrase. Les catégories actuellement prises en charge sont notamment les suivantes :

| `categories`     |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Amendments`      |Eléments qui spécifient les corrections apportées au contrat après qu'il a été signé ou les modifications apportées à un contrat standard. Cette catégorie inclut les éléments qui font référence à des modifications ou des corrections apportées à un contrat.|
|`Asset Use`       |Eléments qui font référence à des situations dans lesquelles une partie prenante doit utiliser les actifs (licences ou matériel) de l'autre partie prenante pour mener à bien ses tâches dans le cadre du contrat, y compris les autorisations et les restrictions comprises dans ce dernier.|
|`Assignments`     |Eléments qui décrivent la cession des droits et/ou des obligations à une partie prenante.|
|`Audits`          |Cette catégorie inclut les éléments faisant référence à la conservation des documents et pièces et au droit des parties prenantes d'inspecter les livres ou les enregistrements de l'autre partie prenante, ainsi que les éléments faisant référence au droit d'une partie prenante à inspecter ou examiner la conformité ou à l'obligation qui est faite à une partie prenante d'être disponible pour l'examen ou l'inspection de conformité.|
|`Business Continuity`|Catégorie restreinte englobant les éléments qui font référence à l'effet sur le contrat d'une vente, d'une fusion ou de toute autre modification substantielle apportée à l'une des parties prenantes.|
|`Communication`  |Eléments qui font référence aux obligations d'informer ou d'adresser une notification sur les méthodes de communication (acte ou processus d'échange d'informations), les représentants à contacter et les informations de contact. En outre, éléments qui font référence aux moyens acceptables mis en place pour l'échange d'informations entre les parties prenantes.|
|`Confidentiality` |Cette catégorie comprend les éléments qui décrivent la manière dont les parties prenantes peuvent utiliser les informations portées à leur connaissance au cours de l'exécution du contrat ; éléments qui traitent de la gestion des secrets commerciaux et éléments qui décrivent la non-divulgation des informations métier.|
|`Deliverables`    |Cette catégorie comprend les éléments qui spécifient l'article ou les articles donnés par une partie prenante à l'autre partie prenante en échange d'argent, ainsi que les éléments qui décrivent la préparation des livrables.|
|`Delivery`        |Eléments qui spécifient le moyen d'assurer le transfert de l'article ou des articles dans la catégorie `Delivrables` de la part d'une partie prenante pour une autre partie prenante. Par exemple, si une partie prenante vend des services d'accès ou de cloud, le moyen permettant d'obtenir cet accès fait partie de cette catégorie.|
|`Dispute Resolution`|Cette catégorie comprend les éléments qui traitent des dispositions relatives au règlement d'un différend qui survient entre les parties prenantes du contrat ; par exemple, le règlement peut prévoir une commission d'arbitrage. Les différends peuvent se régler par différentes méthodes autres que l'arbitrage, par exemple, des dommages irréparables, l'obtention d'une injonction ou une déclaration selon laquelle "Cela ne peut pas faire l'objet d'un recours collectif". Les exemples de différends incluent les différends d'ordre professionnel et les différends concernant la facturation. Cette catégorie inclut également les éléments qui spécifient le choix du droit applicable au contrat, par exemple, un pays ou une juridiction spécifique.|
|`Force Majeure`   |Eléments qui font référence à un événement perturbateur qui échappe au contrôle d'une partie prenante. Il s'agit d'une limitation de responsabilité spécifique.|
|`Indemnification` |Eléments qui spécifient la résolution de certains passifs. On parle de dédommagement lorsqu'il incombe à une partie prenante du contrat de régler le passif d'une autre partie prenante. L'existence d'un dédommagement implique qu'un passif a été contracté.|
|`Insurance`       |Eléments qui font référence à tout type de couverture d'assurance ou à des termes de couverture fournis par une partie prenante à l'autre partie prenante.|
|`Intellectual Property`|Les éléments se divisent en deux catégories : ceux qui utilisent des définitions formelles de brevet, de droits d'auteur et de marques, provenant généralement de statuts et ceux qui utilisent des concepts d'invention, de création et de savoir-faire plus larges. Des définitions peuvent aussi être fournies pour les droits de propriété intellectuelle, par exemple, le droit de poursuivre et de réclamer des dommages pour violation de ces droits de propriété intellectuelle. La catégorie `Intellectual Property` comprend des brevets, des droits de demander des brevets, des marques, des noms de marque, des marques déposées, des noms de domaine, des droits d'auteur et toutes les demandes et tous les enregistrements de ces schémas, modèles industriels, inventions, savoir-faire, secrets commerciaux, programmes informatiques et autre information confidentielle immatérielle. Pour le "code tiers", si une partie du code appartient à quelqu'un d'autre, la propriété intellectuelle doit être appelée.|
|`Liability`       |Eléments qui décrivent la méthode utilisée pour déterminer quand et comment les torts sont imputés à une partie prenante.|
|`Payment Terms & Billing`|Vaste catégorie qui inclut des éléments décrivant de quelle manière une partie prenante doit régler ou recevoir un montant, y compris les éléments suivants : éléments qui font référence au mode final de paiement dans le cadre du contrat ou à la manière dont une partie prenante règle le montant dû ; éléments qui détaillent comment et quand la partie prenante doit recevoir le règlement ou types d'articles que les parties prenantes devront régler ou pour lesquels ils seront facturés ; éléments qui font référence à tout type de paiement des frais, sauf ceux qui font référence à des prix ou des chiffres spécifiques et éléments qui font référence à la partie prenante qui reçoit les paiements dans le cadre du contrat.|
|`Pricing & Taxes` |Cette catégorie comprend les éléments qui décrivent le montant qu'une partie prenante doit payer ou recevoir. Cette catégorie inclut des éléments qui font référence aux taxes et à des montants spécifiques associés à des livrables individuels qui sont échangés, y compris les détails relatifs aux coûts d'un élément spécifique, ainsi que des éléments qui décrivent des chiffres ou des méthodes utilisés spécifiquement pour calculer les prix.|
|`Privacy`         | Eléments qui spécifient le traitement des informations personnelles, autrement dit, les informations privées relatives à une ou plusieurs personnes devant être protégées. Cette catégorie est un sous-ensemble très spécifique de la catégorie `Confidentiality` qui est plus vaste.|
|`Responsabilités`|Tâches annexes au contrat supervisées et contrôlées par une seule des deux parties prenantes. Les types d'élément qui appartiennent à cette catégorie varient en fonction de la nature du contrat.|
|`Safety and Security`|Eléments qui font référence à la sécurité physique ou à des protections de cybersécurité pour des données et des systèmes et éléments qui font référence à l'amélioration de la sécurité du ou des espaces de travail.|
|`Scope of Work`   |Cette catégorie définit ce qui figure et ce qui ne figure pas dans le contrat. Font généralement de cette catégorie les éléments qui indiquent aux parties prenantes de quelle manière une commande spécifique doit être définie. Les exemples incluent des discussions autour du "cahier des charges" ou des descriptions de normes de communication applicables.|
|`Subcontracts`    |Eléments qui font référence au recrutement de parties prenantes pour effectuer certaines tâches dans le cadre du contrat, et permissions, droits, restrictions et conséquences relatifs à ces tâches et qui en découlent.|
|`Term & Termination`|Eléments qui font référence à la durée du contrat, au planning et aux modalités de résiliation de contrat, ainsi que les conséquences de la résiliation, y compris les obligations qui s'appliquent au moment ou après la résiliation.|
|`Warranties`      |Eléments qui concernent spécifiquement les hypothèses sous-jacentes d'arrière-plan sur lesquelles les parties prenantes peuvent s'appuyer. Les conséquences d'une violation de garanties font également partie de cette catégorie.|

## Attributs
{: #attributes}

Le tableau `attributes` indique les attributs identifiés dans la phrase. Chaque objet du tableau inclut trois clés : `type` (type d'attribut issu du tableau suivant), `text` (texte applicable) et `attribute` (points de départ et de fin de l'attribut dans le document). Les attributs actuellement pris en charge sont notamment les suivants :

| `attributes`     |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Location`        |Emplacement géographique ou région.                         |
|`DateTime`        |Date, heure, plage de dates ou intervalle.                   |
|`Currency`        |Valeur et unités monétaires.                                  |

# Assurance
{: #assurance}

{{site.data.keyword.cnc_short}} fournit une notations d'assurance à chaque élément `type` ou `category` qu'il identifie. Il existe actuellement une valeur d'assurance, `High`, montrant de façon indéniable que la classification répertoriée est représentative du contenu.

## Provenance
{: #provenance}

Chaque objet des tableaux `types` et `categories` comprend un objet `provenance`. L'objet `provenance` comporte une ou plusieurs clés `id`. Chaque `id` est doté d'une valeur hachée que vous pouvez envoyer à IBM pour fournir des commentaires ou recevoir du support.

