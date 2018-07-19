---

copyright:
years: 2018
lastupdated: "2018-06-28"

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

# Entendendo as configurações de implementação
{: #configs}

A configuração de implementação padrão para o {{site.data.keyword.cnc_short}} é `2Cores 10G 1 concurrent document (2 VPC)`. Essa configuração pode processar aproximadamente 1.500 páginas por hora. É possível aumentar a capacidade de sua implementação do {{site.data.keyword.cnc_short}} no IBM Cloud Private para diferentes níveis, dependendo de seus requisitos de rendimento. Entre em contato com o representante de vendas da IBM para obter mais informações sobre o aumento da capacidade do {{site.data.keyword.cnc_short}} no IBM Cloud Private.

O {{site.data.keyword.cnc_short}} pode ser escalado em duas dimensões: _vertical_ e _horizontal_.

 - O ajuste de escala _Vertical_ aumenta o rendimento no qual um documento é analisado.
 - O ajuste de escala _Horizontal_ aumenta o número de documentos simultâneos analisados.

As duas opções de ajuste de escala requerem o aumento do número de Virtual Processor Cores (VPCs) usados pelo {{site.data.keyword.cnc_short}}. Para obter informações sobre VPCs, consulte a [documentação de licenciamento ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0/manage_cluster/licensing.html){: new_window} do IBM Cloud Private.

As opções de ajuste de escala vertical incluem o seguinte:

| Configuração                             |Rendimento aproximada *         |
|:-----------------------------------------:|--------------------------------|
|`2Cores 10G 1 concurrent document (2 VPC)` |1500 páginas por hora (padrão)   |
|`4Cores 20G 1 concurrent document (4 VPC)` |2.500 páginas por hora             |
|`8Cores 40G 1 concurrent document (8 VPC)` |3000 páginas por hora             |

As opções de escala horizontal incluem o seguinte:

| Configuração                               |Rendimento aproximada *         |
|:-------------------------------------------:|--------------------------------|
|`2Cores 10G 2 concurrent documents (4 VPC)`  |3000 páginas por hora             |
|`4Cores 20G 2 concurrent documents (8 VPC)`  |5000 páginas por hora             |
|`8Cores 40G 2 concurrent documents (16 VPC)` |6000 páginas por hora             |

\***Nota**: as estimativas de rendimento são baseadas em teste interno. O rendimento em um ambiente de produção depende de múltiplos fatores, incluindo outras cargas de trabalho no mesmo cluster do IBM Cloud Private, latências entre o aplicativo cliente e o cluster do IBM Cloud Private e outras condições ambientais.

Para mudar a configuração de implementação incluindo ou removendo clusters do {{site.data.keyword.cnc_short}}, execute as etapas a seguir.

**Nota**: se você reduzir a capacidade de uma implementação do {{site.data.keyword.cnc_short}}, o IBM Cloud Private recuperará imediatamente os recursos liberados, reduzindo, assim, a capacidade de processamento da implementação. Não reduza a capacidade de uma implementação que está sendo usada, especialmente se a implementação estiver em um ambiente de produção.
	
Da mesma forma, se você aumentar a capacidade de uma implementação do {{site.data.keyword.cnc_short}}, o IBM Cloud Private aplicará imediatamente os recursos solicitados à implementação, supondo que os recursos estejam disponíveis. Se você precisar de mais recursos do que sua instalação do IBM Cloud Private pode fornecer, fale com seu representante de vendas da IBM sobre o aumento da capacidade de sua instalação do IBM Cloud Private.

  1. Efetue login em seu cluster do IBM Cloud Private.

  1. No ícone de menu próximo ao canto superior esquerdo, selecione **Cargas de trabalho -> Implementações**.
  
  1. O console exibe a página  ** Implementações ** . Na tabela de implementações, localize a implementação cuja capacidade deseja aumentar.
  
  1. Na linha da tabela que lista sua implementação, clique no ícone **Ação** na última coluna da tabela e, em seguida, clique em **Escalar**.
  
  1. O console exibe a janela **Escalar implementação**. No campo **Instâncias**, insira o número desejado de instâncias ou use as setas para cima e para baixo para selecionar o valor desejado.
  
  1. Clique em  ** Escalar implementação **.
  
  1. A janela **Escalar implementação** é fechada e o console retorna para a exibição da página **Implementações**. Localize sua implementação e use as colunas **Desejada** e **Atual** para verificar se seu número solicitado de instâncias foi alocado para a implementação.
  
  1. Opcionalmente, clique no nome da implementação na página **Implementações** para visualizar os detalhes da implementação.
  
  

