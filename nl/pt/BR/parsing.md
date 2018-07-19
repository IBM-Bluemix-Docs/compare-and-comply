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

# Entendo a análise sintática do contrato
{: #contract_parsing}

O {{site.data.keyword.cnc_short}} retorna contratos analisados com uma análise de cada elemento
identificado.

As seções a seguir descrevem como o JSON retornado fornece a análise.

## Tipos
{: #contract_types}

A matriz `types` inclui uma série de objetos, cada um dos quais contendo chaves
`nature` e `party` cujos valores identificam um couplet para o elemento. As
tabelas a seguir listam os valores possíveis das chaves `nature` e `party`.

O valor `nature` lista os tipos de ação que a sentença requer.

| `nature`         |Descrição                                                |
|:----------------:|-----------------------------------------------------------|
|`Definition`      |Este elemento inclui clareza para um termo, relacionamento ou semelhante. Nenhuma ação é necessária para cumprir o elemento, nem qualquer parte afetada.|
|`Disclaimer`      |A `party` no elemento não é obrigada a cumprir os
termos especificados pelo elemento, mas não é proibida de fazê-lo.|
|`Exclusion`       |A `party` no elemento não cumprirá os termos
especificados pelo elemento.|
|`Obligation`      |A `party` no elemento é obrigada a cumprir os
termos especificados pelo elemento.|
|`Right`           |A `party` no elemento tem a garantia de que receberá os
termos especificados pelo elemento.|
|`Recommendation`  |A `party` no elemento não é obrigada, mas é
altamente aconselhada a cumprir os termos especificados pelo elemento.|

## Partes
{: #contract_parties}

A matriz `parties` especifica os participantes listados no contrato. Cada objeto
`party` identificado lista a parte identificada por nome e é correspondido com um objeto
`role` que classifica a função do objeto `party`.
Os valores de `role` que podem ser retornados para os contratos incluem:

| `role`           |Descrição                                                |
|:----------------:|-----------------------------------------------------------|
|`Buyer`           |A parte responsável pelo pagamento dos bens ou serviços listados no
contrato.|
|`End User`        |A parte que interagirá com os bens ou serviços fornecidos, distinguidos
explicitamente do `Buyer`.|
|`None`            |Nenhuma parte foi identificada para o elemento. Esse valor está sempre
associado à natureza `Definition`.|
|`Supplier`        |A parte responsável pelo fornecimento dos bens ou serviços listados no
contrato.|

## Categorias
{: #contract_categories}

A matriz `categories` define o assunto da sentença. As categorias atualmente suportadas
incluem:

| `categories`     |Descrição                                                |
|:----------------:|-----------------------------------------------------------|
|`Amendments`      |Elementos que especificam mudanças no contrato após ele ter sido assinado
ou alterações em um contrato padrão. Os elementos que se referem a alterações em um contrato e a mudanças de acordos também se enquadram nesta categoria.|
|`Asset Use`       |Elementos que se referem a situações em que uma parte deve usar os ativos
(licenças ou equipamento) da outra parte na realização de suas obrigações sob o contrato, incluindo permissões
e restrições.|
|`Assignments`     |Elementos que descrevem a transferência de direitos, obrigações ou
ambos para um terceiro.|
|`Audits`          |Esta categoria inclui elementos que se referem à manutenção do registro e
ao direito das partes de inspecionar os livros ou registros da outra parte e os elementos que se referem ao
direito de uma parte para inspecionar ou revisar a conformidade ou os requisitos que uma parte está disponível
para inspeção ou revisão de conformidade.|
|`Business Continuity`|Uma categoria estreita de elementos que referenciam o efeito de uma
venda, fusão ou outra mudança substancial em uma das partes para o acordo.|
|`Communication`  |Elementos que se referem aos requisitos para notificar ou fornecer aviso,
aos detalhes sobre métodos de comunicação (o ato ou o processo de troca de informações), aos representantes de
contato e às informações de contato. Além disso, os elementos que se referem aos meios aceitáveis de troca de
informações entre as partes.|
|`Confidentiality` |Esta categoria inclui elementos que descrevem como as partes podem usar
informações que elas aprendem durante a vigência do contrato, elementos que discutem a manutenção de
segredos comerciais e elementos que descrevem a não divulgação de informações de negócios.|
|`Deliverables`    |Esta categoria inclui elementos que especificam um ou mais itens que uma
parte fornece para a outra parte em troca de dinheiro e elementos que descrevem a preparação de distribuíveis.|
|`Delivery`        |Elementos que especificam os meios de transferência de um ou mais itens na
categoria `Deliverables` de uma parte para outra. Por exemplo, se uma parte estiver vendendo
serviços de acesso ou de nuvem, os meios de obtenção desse acesso se enquadram nesta categoria.|
|`Dispute Resolution`|Esta categoria inclui elementos que discutem provisões para resolução
de qualquer litígio entre partes contratantes; por exemplo, a quitação por um procedimento definido, como um
painel de arbitragem. Os litígios podem ser resolvidos por vários métodos diferentes de arbitragem; por
exemplo, danos irreparáveis, obtenção de uma liminar ou uma instrução para o efeito de "Você não pode
perseguir isso como uma ação de classe". Exemplos de litígios incluem litígios de mão de obra e
litígios sobre faturas ou faturamento. A categoria também inclui elementos que especificam a lei ou a escolha
de lei que rege o contrato, como um país ou jurisdição específica.|
|`Force Majeure`   |Elementos que se referem a eventos perturbadores fora do controle de uma
parte. Trata-se de uma limitação específica da responsabilidade.|
|`Indemnification` |Elementos que especificam a correção de determinados passivos. Indenização é quando uma parte do contrato se torna responsável por pagar a responsabilidade da outra parte. O
fato da indenização implica que uma indenização ocorreu.|
|`Insurance`       |Elementos que se referem a qualquer tipo de cobertura de seguro ou termos
de cobertura fornecidos por uma parte para a outra parte.|
|`Intellectual Property`|Exemplos tendem a se enquadrar em duas categorias: as que usam
definições formais de patentes, copyrights e marcas comerciais, normalmente obtidas dos estatutos e aquelas
que usam conceitos mais amplos de invenções, autoria e know-how. Definições também podem ser fornecidas para
direitos de propriedade intelectual, como o direito de processar e reclamar danos por violação de tais
direitos de propriedade. A `Intellectual Property` inclui patentes, direitos a serem
aplicados a patentes, marcas comerciais, marcas de serviço, nomes de domínios, copyrights e todos os
aplicativos e registro de tais modelos esquemáticos industriais mundiais, invenções, know-how, segredos
comerciais, programas de software de computador e outras informações de propriedade intangíveis. Para "Código
de Terceiros", se qualquer parte do código pertencer a outra pessoa, o IP precisará ser chamado.|
|`Liability`       |Elementos que descrevem o método para determinar quando e como a falha se
conecta a qualquer parte.|
|`Payment Terms & Billing`|Uma categoria ampla que inclui elementos que descrevem como uma
parte paga ou recebe pagamento, incluindo os elementos a seguir: elementos que se referem ao modo final de
pagamento sob o contrato ou o método de pagamento que uma parte utiliza; elementos que detalham como e quando a
parte deve ser paga ou os tipos de itens pelos quais as partes pagarão ou serão faturadas; elementos que se
referem a qualquer tipo de pagamento de taxa, exceto aqueles que se referem a preços ou figuras específicos e
os elementos que se referem à parte que recebe os pagamentos sob contrato.|
|`Pricing & Taxes` |Esta categoria inclui elementos que descrevem o que uma parte está
pagando e pelo que ela está recebendo. A categoria inclui elementos que se referem a impostos e quantias específicas associadas a
distribuíveis individuais que são trocados, incluindo detalhes sobre os custos de um elemento específico e
elementos que descrevem figuras ou métodos específicos para o cálculo de preços.|
|`Privacy`         | Elementos que especificam o tratamento de informações pessoais; ou seja,
informações privadas sobre um ou mais indivíduos específicos que precisam ser protegidos. A categoria é um
subconjunto muito específico da categoria `Confidentiality` mais ampla.|
|`Responsabilidades`|Tarefas auxiliares ao acordo, sobre as quais somente uma das partes tem
supervisão e controle. Os tipos de elementos que se enquadram nesta categoria diferem dependendo da natureza
do contrato.|
|`Safety and Security`|Elementos que se referem à segurança física ou às proteções de
segurança cibernética para dados e sistemas, bem como elementos que se referem à melhoria da segurança de
um ou mais locais de trabalho.|
|`Scope of Work`   |Esta categoria define o que consta no contrato versus o que não consta no
contrato. Geralmente, os elementos que informam às partes como uma ordem específica deve ser definida se
enquadram nessa categoria. Exemplos incluem discussões sobre "declarações de trabalho" ou descrições de normas
de comunicação aplicáveis.|
|`Subcontracts`    |Elementos que se referem à contratação de terceiros para executar
determinas obrigações sob o contrato e as permissões, os direitos, as restrições e as consequências inerentes
e decorrentes.|
|`Term & Termination`|Elementos que se referem à duração do contrato, ao planejamento e aos
termos da rescisão do contrato e às consequências da rescisão, incluindo quaisquer obrigações que se
apliquem à rescisão ou após ela.|
|`Warranties`      |Elementos que se referem especificamente às suposições subjacentes e de
segundo plano das quais as partes podem depender. As consequências de garantias de violação também se
enquadram nesta categoria.|

## Atributos
{: #attributes}

A matriz `attributes` especifica quaisquer atributos identificados na sentença. Cada
objeto na matriz inclui três chaves: `type` (o tipo de atributo da tabela a seguir),
`text` (o texto aplicável) e o `attribute` (os pontos de início e de
término do atributo no documento). Os atributos atualmente suportados incluem:

| `attributes`     |Descrição                                                |
|:----------------:|-----------------------------------------------------------|
|`Location`        |Uma localização geográfica ou região.                         |
|`DateTime`        |Uma data, hora, intervalo de data ou intervalo de tempo.      |
|`Currency`        |Valor monetário e unidades.                                  |

# Garantia
{: #assurance}

O {{site.data.keyword.cnc_short}} fornece uma classificação de garantia para cada elemento
`type` ou `category` que ele identifica. Há atualmente um valor de garantia,
`High`, que indica que há evidência significativa de que a classificação listada é
representativa do conteúdo.

## Procedência
{: #provenance}

Cada objeto nas matrizes `types` e `categories` inclui um objeto `provenance`. O objeto `provenance` tem uma ou mais chaves de `id`. Cada chave de `id` tem um valor com hash que você pode enviar para a IBM para fornecer feedback ou receber suporte.

