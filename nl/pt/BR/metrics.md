---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-25"

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

# Usando métricas
{: #metrics}

É possível monitorar o status do {{site.data.keyword.cnc_short}} usando o painel de
monitoramento do IBM Cloud Private. O painel de monitoramento usa o Grafana para as métricas, o Prometheus para os alertas e o Kibana
para a criação de log para apresentar as informações detalhadas sobre a instância do {{site.data.keyword.cnc_short}}.

## Importando o painel de métricas

Para importar o painel de métricas para o {{site.data.keyword.cnc_short}} no IBM Cloud
Private, execute as etapas a seguir.

  1. Assegure-se de que você tenha extraído e gerado os painéis de métricas, conforme descrito na [Etapa 1: Fazer download, extrair e renderizar os modelos de painel](/docs/services/compare-and-comply/monitor.html#monitor).

  1. Efetue login em seu cluster do IBM Cloud Private.

  1. No ícone de Menu no canto superior esquerdo, selecione
**Plataforma -> Monitoramento**. <br />
      ![Ícone de Menu do IBM Cloud Private](images/icp-menu.png) <br />
      ![Plataforma -> Menu de monitoramento](images/icp-monitoring.png)

  1. Clique em **Início** próximo ao canto superior esquerdo da interface do Grafana. <br />
      ![Home icon](images/icp-home.png)

  1. Clique em **Importar painel**.
      ![Ícone Importar painel](images/import-dboard.png)

  1. Selecione o arquivo `metrics.json` que foi gerado na Etapa 6 do procedimento
anterior e, em seguida, clique em **Fazer upload do arquivo .json**. <br />
      ![Upload do arquivo metrics.json](images/metrics-json.png)

  1. Selecione **Prometheus** como a origem de dados e, em seguida, clique em
**Importar**.
![Selecione Prometheus](images/prometheus.png)

## Visualizando o painel de métricas
{: #view}

O painel de métricas se assemelha ao seguinte:
![O painel de métricas](images/metrics-dboard.png)

É possível mudar facilmente o intervalo de tempo e a frequência de atualização automática:
![Mudar o intervalo de tempo e a taxa de atualização](images/dboard-change.png)

## Editando o painel de métricas

É possível editar o painel de métricas ou criar um novo painel executando as etapas a seguir.

  1. No ícone de Menu no canto superior esquerdo, selecione **Plataforma-> Monitoramento**
para acessar a UI do Grafana.

  1. Clique em **Início** próximo ao canto superior esquerdo da interface
do Grafana e, em seguida, clique em **+ Novo painel**.

  1. Selecione o tipo de painel que você deseja incluir, como **Gráfico** ou
**Tabela**.

  1. Clique no título do painel e, em seguida, clique em **Editar**. O título do
painel padrão é `Título do painel`.

  1. Use a guia **Geral** para configurar o título, a descrição e as dimensões
do painel. Observe que 12 unidades é a largura total de uma janela do navegador.

  1. Use a guia **Métricas** para criar consultas que exibem dados do Prometheus.

    1. É possível gravar a consulta diretamente se você estiver familiarizado com a linguagem de
consulta ou usar o campo **Consulta de métrica** para escolher dentre as métricas que
estão sendo relatadas atualmente ao Prometheus.

    1. Os resultados das consultas são exibidos em tempo real na nova área do painel.

    1. Múltiplas consultas podem ser incluídas em um único painel. Por exemplo, é possível exibir
operações de leitura e gravação no mesmo gráfico ou o total de visitas e o total de visitantes na mesma tabela.
        
