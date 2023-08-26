---
lab:
  title: Explorar a análise em tempo real no Microsoft Fabric
  module: Explore fundamentals of large-scale data analytics
---

# Explorar a análise em tempo real no Microsoft Fabric

Neste exercício, você vai explorar a análise em tempo real no Microsoft Fabric.

Este laboratório levará aproximadamente **25** minutos para ser concluído.

> **Observação**: você precisará ter uma licença do Microsoft Fabric para concluir este exercício. Confira [Introdução ao Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial) para obter detalhes de como habilitar uma licença de avaliação gratuita do Fabric. Você precisará ter uma conta *corporativa* ou de *estudante* da Microsoft para fazer isso. Caso não tenha uma, [inscreva-se em uma avaliação do Microsoft Office 365 E3 ou superior](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

## Criar um workspace

Antes de trabalhar com os dados no Fabric, crie um workspace com a avaliação do Fabric habilitada.

1. Entre no [Microsoft Fabric](https://app.fabric.microsoft.com) em `https://app.fabric.microsoft.com`.
2. Na barra de menus à esquerda, selecione **Workspaces** (o ícone é semelhante a &#128455;).
3. Crie um workspace com um nome de sua escolha, selecionando um modo de licenciamento na seção **Avançado** que inclua a capacidade do Fabric (*Avaliação*, *Premium* ou *Malha*).
4. Quando o novo workspace for aberto, ele estará vazio.

    ![Captura de tela de um workspace vazio no Power BI.](./images/new-workspace.png)

## Criar um banco de dados KQL

Agora que você tem um workspace, crie um banco de dados KQL para armazenar os dados em tempo real.

1. No canto inferior esquerdo do portal, alterne para a experiência de **Análise em Tempo Real**.

    ![Captura de tela do menu do alternador de experiência.](./images/fabric-real-time.png)

    A home page da análise em tempo real inclui blocos para criação dos ativos comumente usados para a análise de dados em tempo real

2. Na home page da análise em tempo real, crie um **Banco de Dados KQL** com um nome de sua escolha.

    Após alguns minutos, um banco de dados KQL será criado:

    ![Captura de tela de um novo banco de dados KQL.](./images/kql-database.png)

    Atualmente, não há tabelas no banco de dados.

## Criar um fluxo de eventos

Os fluxos de eventos fornecem uma forma escalonável e flexível de ingerir dados em tempo real de uma fonte de streaming.

1. Na barra de menus à esquerda, selecione a **home page** da experiência de análise em tempo real.
1. Na home page, selecione o bloco para criar um **Fluxo de eventos** com um nome de sua escolha.

    Após alguns instantes, o designer visual do fluxo de eventos será exibido.

    ![Captura de tela do designer de Fluxo de eventos.](./images/eventstream-designer.png)

    A tela do designer visual mostra uma origem que se conecta ao fluxo de eventos, que, por sua vez, está conectado a um destino.

1. Na tela do designer, na lista **Nova origem** da origem, selecione **Dados de exemplo**. Em seguida, no painel **Dados de exemplo**, especifique o nome **taxis** e selecione os dados de exemplo de **Táxis Amarelos** (que representam os dados coletados de corridas de táxis). Em seguida, selecione**Adicionar**.
1. Abaixo da tela do designer, selecione a guia **Visualização de dados** para visualizar os dados que estão sendo transmitidos da origem:

    ![Captura de tela da visualização de dados do Fluxo de eventos.](./images/eventstream-preview.png)

1. Na tela do designer, na lista **Novo destino** do destino, selecione **Banco de dados KQL**. Em seguida, no painel **Banco de dados KQL**, especifique o nome de destino **taxi-data** e selecione o workspace e o banco de dados KQL. Depois, escolha **Criar e configurar**.
1. No assistente **Ingerir dados**, na página **Destino**, selecione **Nova tabela** e insira o nome da tabela **taxi-data**. Em seguida, selecione **Avançar: Origem**.
1. Na página **Origem**, revise o nome da conexão de dados padrão e escolha **Avançar: Esquema**.
1. Na página **Esquema**, altere o **Formato de dados** de TXT para **JSON** e veja a visualização para verificar se esse formato resulta em várias colunas de dados. Em seguida, selecione **Avançar: Resumo**.
1. Na página **Resumo**, aguarde até que a ingestão contínua seja estabelecida e escolha **Fechar**.
1. Verifique se o fluxo de eventos concluído tem esta aparência:

    ![Captura de tela de um Fluxo de eventos concluído.](./images/complete-eventstream.png)

## Consultar dados em tempo real em um banco de dados KQL

O fluxo de eventos preenche continuamente uma tabela no banco de dados KQL, permitindo que você consulte os dados em tempo real.

1. No hub de menus à esquerda, selecione o banco de dados KQL (ou escolha o workspace e localize o banco de dados KQL nele).
1. No menu **…** da tabela **taxi-data** (que foi criada pelo fluxo de eventos), selecione **Tabela de consulta > Registros ingeridos nas últimas 24 horas**.

    ![Captura de tela do menu Tabela de consulta em um banco de dados KQL.](./images/kql-query.png)

1. Veja os resultados da consulta: ela deverá ser uma consulta KQL como esta:

    ```kql
    ['taxi-data']
    | where ingestion_time() between (now(-1d) .. now())
    ```

    Os resultados mostram todos os registros de táxis ingeridos da fonte de streaming nas últimas 24 horas.

1. Substitua todo o código da consulta KQL na metade superior do editor de consultas pelo seguinte código:

    ```kql
    // This query returns the number of taxi pickups per hour
    ['taxi-data']
    | summarize PickupCount = count() by bin(tpep_pickup_datetime, 1h)
    ```

1. Use o botão **&#9655; Executar** para executar a consulta e analisar os resultados, que mostram o número de embarques em táxis em cada hora.

## Limpar os recursos

Se você terminou de explorar a análise em tempo real no Microsoft Fabric, exclua o workspace criado para este exercício.

1. Na barra à esquerda, selecione o ícone do workspace para ver todos os itens que ele contém.
2. No menu **…** da barra de ferramentas, selecione **Configurações do workspace**.
3. Na seção **Outros**, selecione **Remover este workspace**.
