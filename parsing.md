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
|`Amendments`      |Elements that specify changes to the contract after it has been signed, or alterations to a standard contract. Elements referring to alterations to a contract and changes to agreements also fall into this category.|
|`Asset Use`       |Elements that refer to situations in which one party must use the assets (licenses or equipment) of the other party in conducting their duties under the agreement, including permissions and restrictions thereon.|
|`Assignments`     |Elements that describe the transfer of rights, obligations, or both to a third party.|
|`Audits`          |This category includes elements referring to record-keeping and the right of parties to inspect the books or records of the other party, and elements referring to either the right of a party to inspect or review compliance, or requirements that a party be available for inspection or compliance review.|
|`Business Continuity`|A narrow category for elements that reference the effect of a sale, merger, or other substantial change to one of the parties to the agreement.|
|`Communication`  |Elements referring to requirements to notify or provide notice, details about communication methods (the act or process of exchanging information), contact representatives, and contact information. Additionally, elements referring to acceptable means of exchanging information between parties.|
|`Confidentiality` |This category includes elements describing how the parties can use information that they learn in the course of completing the contract; elements that discuss maintaining trade secrets; and elements describing the nondisclosure of business information.|
|`Deliverables`    |This category includes elements specifying the item or items one party gives to the other party in exchange for money, and elements describing the preparation of deliverables.|
|`Delivery`        |Elements that specify the means of transferring the item or items in the `Deliverables` category from one party to another. For example, if one party is selling access or cloud services, the means of getting that access falls into this category.|
|`Dispute Resolution`|This category includes elements that discuss provisions for settling any dispute arising between contracting parties; for example, settlement by a defined procedure such as an arbitration panel. Disputes can be settled by various methods other than arbitration; for example, irreparable harm, obtaining an injunction, or a statement to the effect of "You may not pursue this as a class action". Examples of disputes include labor disputes and disputes about invoices or billing. The category also includes elements that specify the contract's governing law or choice of law, such as a particular country or jurisdiction.|
|`Force Majeure`   |Elements that refer to disruptive events outside a party's control. It is a specific limitation of liability.|
|`Indemnification` |Elements that specify the remediation of certain liabilities. Indemnification is when one party of the contract becomes responsible for paying for another party's liability. The fact of indemnification implies that a liability has occurred.|
|`Insurance`       |Elements referring to any kind of insurance coverage or terms of coverage provided by one party to the other party.|
|`Intellectual Property`|Examples tend to fall into two categories: those using formal definitions of patents, copyrights, and trademarks, typically taken from statutes; and those using broader concepts of inventions, authorship, and know-how. Definitions can also be provided for intellectual property rights, such as the right to sue and claim damages for violation of such property rights. `Intellectual Property` includes patents, rights to apply for patents, trademarks, trade names, service marks, domain names, copyrights, and all applications and registration of such worldwide, schematics, industrial models, inventions, know-how, trade secrets, computer software programs, and other intangible proprietary information. For "Third Party Code", if any part of the code belongs to someone else, IP needs to be called out.|
|`Liability`       |Elements that describe the method for determining when and how fault attaches to any party.|
|`Payment Terms & Billing`|A broad category that includes elements that describe how a party pays or gets paid, including the following: elements that refer to the final mode of payment under the contract or how one party is paying; elements detailing how and when the party is to be be paid or the types of items the parties will be paying or billed for; elements referring to any kind of fee payment, except those that refer to specific prices or figures; and elements referring to the party that receives the payments under the contract.|
|`Pricing & Taxes` |This category includes elements that describe what a party pays or gets paid. The category includes elements that refer to taxes and specific amounts associated with individual deliverables that are exchanged, including details about the costs of a particular element, and elements that describe specific figures or methods for calculating prices.|
|`Privacy`         | Elements that specify the treatment of personal information; that is, private information about a specific individual or individuals that needs to be protected. The category is a very specific subset of the broader `Confidentiality` category.|
|`Responsibilities`|Tasks ancillary to the agreement, over which only one of the parties has oversight and control. The types of elements that fall into this category differ depending on the nature of the contract.|
|`Safety and Security`|Elements referring to physical safety or cybersecurity protections for data and systems, as well as elements referring to enhancing safety of the workplace or workplaces.|
|`Scope of Work`   |This category defines what is in the contract versus what is not in the contract. Typically, elements that tell the parties how a particular order is to be defined fall into this category. Examples include discussions of "statements of work" or descriptions of applicable communications standards.|
|`Subcontracts`    |Elements referring to the hiring of third parties to perform certain duties under the contract, and the permissions, rights, restrictions, and consequences thereto and arising therefrom.|
|`Term & Termination`|Elements referring to duration of the contract, the schedule and terms of contract termination, and any consequences of termination, including any obligations that apply at or after termination.|
|`Warranties`      |Elements that refer specifically to background, underlying assumptions that the parties can rely on. Consequences of breaching warranties also fall under this category.|

## Attributes
{: #attributes}

The `attributes` array specifies any attributes identified in the sentence. Each object in the array includes three keys: `type` (the type of attribute from the following table), `text` (the applicable text), and `attribute` (the start and end points of the attribute in the document). Currently supported attributes include:

| `attributes`     |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`Location`        |A geographical location or region.                         |
|`DateTime`        |A date, time, date range, or time range.                   |
|`Currency`        |Monetary value and units.                                  |

# Assurance
{: #assurance}

{{site.data.keyword.cnc_short}} gives an assurance rating to each `type` or `category` element it identifies. Assurance values include the following:

| `assurance`      |Description                                                |
|:----------------:|-----------------------------------------------------------|
|`High`            |There is significant evidence that the listed classification is representative of the content.|
|`Low`             |There is some evidence to support the listed classification, but it might need further review to confirm.|

## Provenance
{: #provenance}

Each object in the `types` and `categories` arrays includes a `provenance` object. The `provenance` object has one or more `id` keys. Each `id` key has a hashed value that you can send to IBM to provide feedback or receive support.

