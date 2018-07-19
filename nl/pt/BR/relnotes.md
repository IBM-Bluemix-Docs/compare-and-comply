
---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-03"

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

# Notas sobre a Liber
{: #release_notes}

As notas sobre a liberação fornecem informações sobre as mudanças na liberação de serviço do
{{site.data.keyword.cnc_long}} no {{site.data.keyword.BluOpenStackDed}}.

**Importante:** este conjunto de documentação se aplica somente ao serviço do
{{site.data.keyword.cnc_short}} no {{site.data.keyword.BluOpenStackDed}}. Isso não é aplicável aos outros serviços Watson que estão disponíveis no IBM Cloud público.

## Versão da API de Serviço
{: #api_versioning}

As solicitações de API requerem um parâmetro version que use uma data no formato `version=YYYY-MM-DD`. Sempre que mudamos a API de uma maneira incompatível com versões anteriores, lançamos uma nova versão secundária da API.

Envie o parâmetro version com cada solicitação de API. O serviço usa a versão da API para a data que você especificar ou a versão mais recente antes dessa data. Não padronize para a data atual. Em vez disso, especifique uma data que corresponda a uma versão que seja
compatível com seu aplicativo e não mude-a até que seu aplicativo esteja pronto para uma versão mais recente.

A versão atual é `2018-03-23`.

## Mudanças
{: #changes}

Os novos recursos e mudanças a seguir para o serviço estão disponíveis.

**Importante**: o número da versão referido nas seções a seguir é a versão do
gráfico Helm do {{site.data.keyword.cnc_long}} que você implementou em seu cluster do
{{site.data.keyword.BluOpenStackDed}}.

### 1.0.4, 5 de julho de 2018
{: #ingress}

**Importante**: devido às mudanças descritas nesta seção, não é possível fazer
upgrade para a versão 1.0.4 de versões anteriores do {{site.data.keyword.cnc_short}} usando os
procedimentos de upgrade padrão do {{site.data.keyword.BluOpenStackDed}}. Deve-se reimplementar o
{{site.data.keyword.cnc_short}} conforme a seguir:

1.  Remova a implementação existente do {{site.data.keyword.cnc_short}} conforme
descrito em [Removendo
uma implementação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/manage_applications/remove_app.html){: new_window}.

1.  Implemente o {{site.data.keyword.cnc_short}} versão 1.0.4 ou mais recente conforme
descrito no arquivo `README.md` do gráfico Helm do {{site.data.keyword.cnc_short}}
em [Implementando o serviço](/docs/services/compare-and-comply/deploy.html).

**Importante:** a reimplementação do serviço tem as consequências a seguir:

- O serviço está indisponível entre a remoção da instância anterior e a implementação da instância
atual.
- Se você tiver o código que faz chamadas API que incluem números de porta nas 'URLs' de chamadas, deve-se
remover os números de porta. Se você fizer uma chamada API que inclui um número da porta no
{{site.data.keyword.cnc_short}} versão 1.0.4 ou mais recente, a chamada falhará com um erro
semelhante ao seguinte:
  ```
  curl: (7) Failed to connect to 10.19.74.45 port 8443: Connection refused
  ```
  {: pre}

**Importante:** se você estiver executando uma versão do
{{site.data.keyword.cnc_short}} anterior à 1.0.4, deverá incluir o especificador
`:{port_number}` com o endereço IP do cluster do
{{site.data.keyword.BluOpenStackDed}} ao fazer chamadas para o serviço, como no exemplo a seguir:
```bash
curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
```
{: pre}

#### Notas adicionais

-   Instruções de implementação adicionais agora são fornecidas em
[ Implementando o serviço](/docs/services/compare-and-comply/deploy.html).
-   O serviço do {{site.data.keyword.cnc_short}} está agora integrado ao
[controlador
de ingresso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/components.html){: new_window}
do {{site.data.keyword.BluOpenStackDed}}. Essa mudança permite chamar os métodos de API do serviço sem
a especificação de um número de porta nas URLs de chamada. A seguir está uma chamada API típica antes da
integração do **ingresso**:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45:8443/api/v1/parse?version=2018-03-23
    ```
    {: pre}

  A chamada equivalente na versão 1.0.4 ou mais recente é:

    ```bash
    curl -k -X POST -F 'file=@./myPDF.pdf;type=application/pdf' https://10.19.74.45/api/v1/parse?version=2018-03-23
    ```
    {: pre}

- Ao implementar a liberação 1.0.4, pode ser exibido um erro semelhante ao seguinte:

    ```
    Unexpected response code 500 from request: GET https://icp-management-ingress:8443/helm-api/api/v1/releases/weblib?locale=en-US HTTP/1.1 Accept: application/json Content-Type: application/json Content-Type: application/json Cookie: *** {} HTTP/1.1 500 Server: openresty/1.11.2.4 Date: Thu, 21 Jun 2018 23:09:05 GMT Content-Type: application/json; charset=utf-8 Content-Length: 93 Connection: close X-Powered-By: Express Cache-Control: private, max-age=0, no-cache Etag: W/"5d-6htOwRqeFWIdvat6vK/kTg" {"statusCode":500,"message":"Internal service error : Cannot read property '0' of undefined"}
    ```
    {: codeblock}

    É possível desconsiderar e fechar com segurança o erro.

### 1.0.3, 25 de maio de 2018

- A saída do método `parse` agora inclui a matriz `attributes`.
Consulte [Revisar a
análise](/docs/services/compare-and-comply/getting-started.html#review_analysis) e [Atributos](/docs/services/compare-and-comply/parsing.html#attributes)
para obter informações. É possível usar as informações na matriz `attributes` para procurar
por elementos de documento que se referem a locais, horários, datas, intervalo de horário ou
intervalo de data e valores e unidades monetárias específicos.
- Cada objeto nas matrizes `types` e `categories` inclui um objeto `provenance`. O objeto `provenance` tem uma ou mais chaves de `id`. Cada chave de `id` tem um valor com hash que você pode enviar para a IBM para fornecer feedback ou receber suporte.
- Melhorias na precisão da análise sintática de PDF e no desempenho da análise sintática de arquivos PDF
grandes.
- Esta liberação está disponível no {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 ou
superior. Veja a entrada do catálogo para o serviço para obter detalhes sobre os requisitos do {{site.data.keyword.BluOpenStackDed}}.
- Os valores possíveis para a chave de `assurance` não incluem mais
`Low`.

### 1.0.2, 19 de abril de 2018

- Categorias suportadas adicionais. Consulte a
[Documentação de categorias](/docs/services/compare-and-comply/parsing.html#contract_categories)
para obter a lista mais recente de categorias suportadas.
    - `Asset Use`
    - `Communication`
    - `Safety and Security`
-  Atualizações da documentação, incluindo correções para os exemplos em
[Introdução](/docs/services/compare-and-comply/getting-started.html).

### 1.0.1 (liberação de Disponibilidade Geral), 23 de março de 2018

As notas a seguir se aplicam à liberação de Disponibilidade Geral (GA) do serviço
do {{site.data.keyword.cnc_long}}.

- A liberação GA está disponível apenas no {{site.data.keyword.BluOpenStackDed}} 2.1.0.2 ou
superior. Veja a entrada do catálogo para o serviço para obter detalhes sobre os requisitos do {{site.data.keyword.BluOpenStackDed}}.

## Notas Gerais
{: #general_notes}

- A API do {{site.data.keyword.cnc_short}} no {{site.data.keyword.BluOpenStackDed}}
não requer autorização tal como as APIs no IBM Cloud público fazem.
 - Os PDFs devem estar no formato de texto para que sejam analisados. Documentos que foram varridos não podem ser analisados, mesmo que as varreduras tenham sido processadas por um leitor de caracteres ópticos (OCR).

## Problemas Conhecidos
{: #known_issues}

- O tamanho máximo de um arquivo PDF que pode ser transferido por upload é de 50 Mb.
- Os PDFs com a segurança ativada não podem ser analisados.
- Documentos com layouts de página não padrão (como 2 ou 3 colunas por página) não são analisados
corretamente.
