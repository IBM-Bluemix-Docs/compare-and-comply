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

# Understanding contract parsing
{: #contract_parsing}

{{site.data.keyword.cnc_short}} returns parsed contracts with an analysis of each identified element.

The following sections describe how the returned JSON provides the analysis.

## Types
{: #contract_types}

The `types` array includes a number of objects, each of which contains `nature` and `party` keys whose values identify a couplet for the element. The following tables list the possible values of the `nature` and `party` keys.

The `nature` value lists the types of action the sentence requires.

| `nature`         |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Definition`      |This element adds clarity for a term, relationship, or similar. No action is required to fulfill the element, nor is any party affected.|
|`Disclaimer`      |The `party` in the element is not obligated to fulfill the terms specified by the element but is not prohibited from doing so.|
|`Exclusion`       |The `party` in the element will not fulfill the terms specified by the element.|
|`Obligation`      |The `party` in the element is required to fulfill the terms specified by the element.|
|`Right`           |The `party` in the element is guaranteed to receive the terms specified by the element.|
|`Recommendation`  |The `party` in the element is not obligated, but is strongly advised, to fulfill the terms specified by the element.|

## Parties
{: #contract_parties}

The `parties` array specifies the participants listed in the contract. Each identified `party` object lists the identified party by name and is matched with a `role` object that classifies the role of the `party` object. The values of `role` that can be returned for contracts include:

| `role`           |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Buyer`           |The party responsible for paying for the goods or services listed in the contract.|
|`End User`        |The party who will interact with the provided goods or services, explicitly distinguished from the `Buyer`.|
|`None`            |No party was identified for the element. This value is always paired with the `Definition` nature.|
|`Supplier`        |The party responsible for providing the goods or services listed in the contract.|

## Categories
{: #contract_categories}

The `categories` array defines the the subject matter of the sentence. Currently supported categories include:

| `categories`     |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Amendments`      | A modification of the original contract that stipulates the conditions for changing the terms.|
|`Assignments`     |A description of how to transfer rights held by one party to another.|
|`Audits`          | This clause allows for the buyer to examine or inspect the supplier of the services being provided.|
|`Business Continuity`| Describes how an organization will continue delivery of work at predefined levels, in the event of a disruptive incident.|
|`Confidentiality` |A description of how confidential or private information will be handled, such as who can share what and how.|
|`Definitions`     |A section that defines a term used in the document.|
|`Deliverables`    |A list of the goods or services to be delivered at the end of a piece of work.|
|`Delivery`        |Details of the specific schedule or process needed to complete a project.|
|`Dispute Resolution`|Provisions for any dispute arising among contracting parties, and how disputes are to be handled.|
|`Force Majeure`   |A clause that frees both parties from liability in case of a disruptive event.|
|`Indemnification` |A description of the remedies or consequences if the terms of the contract are breached.|
|`Insurance`       |A description of the level of insurance coverage a supplier must carry.|
|`Intellectual Property`|A clause that relates to patents, copyrights, and trademarks. More generally it can relate to invention, authorship, or know-how.|
|`Liability`       |A description of obligations and limitations for which each party is responsible.|
|`Miscellaneous`   |Sections of interest that do not fit into other categories.|
|`Payment Terms & Billing`|A description of payments due and the payment schedule.|
|`Pricing & Taxes` |A description of how the prices are made up and how taxes are to be applied.|
|`Responsibilities`|A description of each party's responsibilities.|
|`Scope of Work`   |A description of what is to be achieved by the parties, as detailed in the statement of work.|
|`Subcontracts`    |Information related to any third parties involved to fulfill a requirement.|
|`Term & Termination`|The time over which something will happen, and the conditions under which it can end.|
|`Warranties`      |Guarantee by a supplier of how a product will work.|

# Assurance
{: #assurance}

{{site.data.keyword.cnc_short}} gives an assurance rating to each `type` or `category` element it identifies. Possible assurance values include:

| `assurance`      |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`High`            |There is significant evidence that the listed classification is representative of the content.|
|`Low`             |There is some evidence to support the listed classification, but it might need further review to confirm.|

An accessible version of this page is available at: [https://www.ibm.com/watson/developercloud/compare-and-comply/doc/parsing.html](https://www.ibm.com/watson/developercloud/compare-and-comply/doc/parsing.html)
