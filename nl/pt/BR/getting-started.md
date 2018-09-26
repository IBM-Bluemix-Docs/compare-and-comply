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

# Informações de introdução
{: #getting_started}

Neste breve tutorial, introduzimos o {{site.data.keyword.cnc_long}} no IBM Cloud Private e
percorremos o processo de análise sintática de um contrato para identificar partes do componente, a natureza
delas, as partes afetadas e quaisquer categorias identificadas.

## Antes de iniciar
{: #before-you-begin}

Para poder usar o serviço do {{site.data.keyword.cnc_short}}, deve-se instalar a CLI
do IBM Cloud Private e efetuar login no cluster do IBM Cloud Private, conforme descrito em
[Instalando
a CLI do IBM Cloud Private](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_cluster/install_cli.html).
 
## Etapa 1: Identificar conteúdo
{: #identify_content}

Identifique os documentos apropriados para analisar. O {{site.data.keyword.cnc_short}} foi
projetado para analisar os documentos do contrato<!-- and regulatory --> que
atendem aos critérios a seguir:

- Os arquivos a serem analisados estão no formato PDF.
- O conteúdo do PDF está no formato de texto. Documentos que foram varridos não podem ser analisados, mesmo que as varreduras tenham sido processadas por um leitor de caracteres ópticos (OCR).

  **Nota:** é possível identificar um PDF que está no texto abrindo o documento em
um visualizador PDF e usando a ferramenta **Seleção de texto** para selecionar uma
palavra única. Se não for possível selecionar uma única palavra no documento, o arquivo não poderá ser
analisado.

- Arquivos não são maiores que 50 MB em tamanho.
- PDFs protegidos (com uma senha para abrir) e PDFs de edição restrita (com uma senha para edição) não podem ser analisados.

## Etapa 2: Analisar um contrato
{: #parse_contract}

Em um shell `bash` ou em um ambiente equivalente, como o Cygwin, use o método
`POST/v1/parse` para analisar seu contrato. Substitua `{ICP_IP_address}`
pelo endereço IP para o cluster do IBM Cloud Private. Substitua `{PDF_file}`
pelo caminho para o PDF a ser analisado.

```bash
curl -k -X POST -F 'file=@{PDF_file};type=application/pdf' https://{ICP_IP_address}/api/v1/parse?version=2018-03-23
```
{: pre}

**Importante:** se você estiver executando uma versão do
{{site.data.keyword.cnc_short}} anterior à 1.0.4, deverá incluir o especificador
`:{port_number}` com o endereço IP do cluster do IBM Cloud Private ao fazer chamadas para o
serviço, conforme a seguir:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://{ICP_IP_address}:{port_number}/api/v1/parse?version=2018-03-23
```
{: pre}

Por exemplo:

```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

Consulte a [nota sobre a liberação
no ingresso](/docs/services/compare-and-comply/relnotes.html#ingress) para obter detalhes.

O método retorna um objeto JSON que contém:

 - Uma conversão HTML do arquivo PDF de origem.
 - O título do documento extraído.
 - Uma matriz de `elements` que define as partes do componente do contrato.
 - Uma matriz de `parties` que define entidades que foram identificadas como
partes.
 
**Importante**: a API do {{site.data.keyword.cnc_short}} no IBM Cloud
Private não requer autorização, tal como APIs no IBM Cloud público fazem.

## Etapa 3: Revise a análise
{: #review_analysis}

Cada objeto na matriz de `elements` descreve um elemento do contrato que o
{{site.data.keyword.cnc_short}} identificou. O código a seguir representa um elemento típico:

```
{
  "sentence_text": "If the parties are unable to agree on the License Price within thirty (30) days after Richmond Enterprises provides the written notice of its exercise of the Optional Patent License to Savage Narwhal Studios in Section 2.3(a), such FMV Dispute shall, at either party's request, be resolved solely and exclusively by final, binding and confidential arbitration to be filed and the decision rendered in New York, New York (with hearings at the request of either party to be held in San Francisco, California or other mutually agreeable place convenient for the parties) in accordance with the Commercial Arbitration Rules of the American Arbitration Association (\" AAA \"), including as supplemented by the Procedures for Large, Complex Commercial Disputes.", "attributes": [ {
      "type": "Location", 		"text": "New York", 		"attribute": {
        "begin": 58372,
			"end": 58380
		}
    },
    {
      "type": "Location", 		"text": "New York", 		"attribute": {
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
  ], "categorias": [ {
      "label": "Communication",
		"assurance": "High",
		"provenance": [
			{
          "id": " C7xhbsepUodh0 9zmJdUXSvYZCdixx00wFyCZuAnTujok=" }
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
          "id": "CdPeg8mAxM5YIsdpzzaluDg7jOj3F30ABGaGLKKxppFA=" }
      ]
    },
    {
      "label": "Intellectual Property",
		"assurance": "High",
		"provenance": [
			{
          "id": " Cor/mgcf1UE/zmsKm68M6 + a9LSRCpcKe8EWCUdwsjrgs=" }
      ]
    }
  ], "types": [ {
      "label": {
        "nature": "Obligation",
			"party": "All Parties"
      },
		"assurance": "High",
		"provenance": [
			{
          "id": "NNpSqaNkY2zHtxI6Zh78NaZccVNtutrQxISkzdnaVjx0 ="
        },
        {
          "id": " PlyERkjg5is36RpFjFFjVUFXp69eDmGmCxLCXRs1sDMDUCo=" }
      ]
    }
  ], "sentença": {
    "begin": 57998, "end": 58952
  }
}
```

O elemento possui cinco seções importantes:
  - `sentence_text`: o texto que foi analisado.
  - `attributes`: uma matriz que lista um ou mais atributos do elemento. Atualmente, os
objetos suportados na matriz `attributes` incluem `Location` (local ou
região geográfica referenciada pelo elemento), `DateTime` (data, hora, intervalo de data ou
intervalo de tempo especificado pelo elemento) e `Currency` (valores e unidades monetários). 
  - `categories`: uma matriz que lista as categorias funcionais nas quais a sentença
identificada se enquadra; em outras palavras, o assunto da sentença.
  - `types`: uma matriz que descreve qual é o elemento e quem ele afeta. Ela consiste
em um ou mais conjuntos de chaves `nature` (o efeito da sentença na `party`
identificada) e de chaves `party` (quem a sentença afeta).
  - `sentence`: um objeto que descreve onde o elemento foi localizado no HTML
convertido. Ele contém um valor de caractere `start` e um valor de caractere
`end`.

**Nota**: algumas sentenças não se enquadram em nenhum tipo ou categoria, caso em que
o serviço retorna as matrizes `types` e `categories` como objetos vazios.

**Nota:** algumas sentenças abrangem múltiplos tópicos, caso em que o serviço retorna
múltiplos conjuntos de objetos `types` e `categories`.

**Nota**: algumas sentenças não contêm nenhum atributo identificável, caso em que o
serviço retorna a matriz `attributes` como objetos vazios.

Além disso, quaisquer partes identificadas são definidas na matriz `parties`. A matriz `parties` está localizada após a matriz `elements` na saída do JSON.

```json
  "parties" : [ {
    "party" : "Customer",
    "role" : "Buyer"
  } ]
```

A matriz `parties` inclui duas seções importantes:

  - `party`: o texto que foi identificado como uma parte no documento.
  - `role`: a função da parte identificada. As funções mudam com base no subdomínio;
consulte [a documentação sobre o
subdomínio especificado para obter uma lista de possíveis funções](/docs/services/compare-and-comply/parsing.html#contract_parties). As partes que não podem ser
identificadas como tendo uma função específica são listadas com o valor `unknown`.

## Próximas etapas
{: #next_steps}

Você analisou com sucesso um contrato para identificar a natureza, as partes e as categorias das partes
do componente do documento. É possível usar a análise para entender e cumprir rapidamente o contrato
analisado. As próximas etapas são:

 - [Entendo a análise sintática do contrato](/docs/services/compare-and-comply/parsing.html#contract_parsing)
 - [Entendendo o esquema de saída](/docs/services/compare-and-comply/schema.html#output_schema) e a [análise sintática da tabela](/docs/services/compare-and-comply/tables.html#understanding_tables).


