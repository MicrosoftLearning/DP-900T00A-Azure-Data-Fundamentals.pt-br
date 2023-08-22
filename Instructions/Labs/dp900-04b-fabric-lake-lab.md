---
lab:
  title: Explorar a análise de dados no Microsoft Fabric
  module: Explore fundamentals of large-scale data analytics
---

# Explorar a análise de dados no Microsoft Fabric

Neste exercício, você vai explorar a ingestão e a análise de dados em um Microsoft Fabric Lakehouse.

Este laboratório levará aproximadamente **25** minutos para ser concluído.

> **Observação**: você precisará ter uma licença do Microsoft Fabric para concluir este exercício. Confira [Introdução ao Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial) para obter detalhes de como habilitar uma licença de avaliação gratuita do Fabric. Você precisará ter uma conta *corporativa* ou de *estudante* da Microsoft para fazer isso. Caso não tenha uma, [inscreva-se em uma avaliação do Microsoft Office 365 E3 ou superior](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

## Criar um workspace

Antes de trabalhar com os dados no Fabric, crie um workspace com a avaliação do Fabric habilitada.

1. Entre no [Microsoft Fabric](https://app.fabric.microsoft.com) em `https://app.fabric.microsoft.com`.
2. Na barra de menus à esquerda, selecione **Workspaces** (o ícone é semelhante a &#128455;).
3. Crie um workspace com um nome de sua escolha, selecionando um modo de licenciamento na seção **Avançado** que inclua a capacidade do Fabric (*Avaliação*, *Premium* ou *Malha*).
4. Quando o novo workspace for aberto, ele estará vazio.

    ![Captura de tela de um workspace vazio no Power BI.](./images/new-workspace.png)

## Criar um lakehouse

Agora que você tem um workspace, é hora de alternar para a experiência de *Engenharia de dados* no portal e criar um data lakehouse para os arquivos de dados.

1. No canto inferior esquerdo do portal, alterne para a experiência de **Engenharia de Dados**.

    ![Captura de tela do menu do alternador de experiência.](./images/fabric-switcher.png)

    A home page de engenharia de dados inclui blocos para criação de ativos de engenharia de dados comumente usados.

2. Na home page de **Engenharia de dados**, crie um **Lakehouse** com um nome de sua escolha.

    Após alguns minutos, um lakehouse será criado:

    ![Captura de tela de um novo lakehouse.](./images/new-lakehouse.png)

3. Veja o novo lakehouse e observe que o painel do **Lakehouse Explorer** à esquerda permite que você navegue pelas tabelas e pelos arquivos no lakehouse:
    - A pasta **Tabelas** contém tabelas que você pode consultar usando o SQL. As tabelas de um lakehouse do Microsoft Fabric são baseadas no formato de arquivo *Delta Lake* de código aberto, comumente usado no Apache Spark.
    - A pasta **Arquivos** contém arquivos de dados no armazenamento OneLake para o lakehouse que não estão associados às tabelas delta gerenciadas. Você também pode criar *atalhos* nessa pasta para referenciar os dados armazenados externamente.

    Atualmente, não há tabelas nem arquivos no lakehouse.

## Ingestão de dados

Uma forma simples de ingerir dados é usar uma atividade **Copiar Dados** em um pipeline para extrair os dados de uma fonte e copiá-los para um arquivo no lakehouse.

1. Na **home page** do lakehouse, no menu **Obter dados**, selecione **Novo pipeline de dados** e crie um pipeline de dados chamado **Ingerir Dados de Vendas**.
1. No assistente **Copiar Dados**, na página **Escolher uma fonte de dados**, selecione o conjunto de dados de exemplo **Modelo de Dados de Varejo da Wide World Importers**.

    ![Captura de tela da página Escolher fonte de dados.](./images/choose-data-source.png)

1. Selecione **Avançar** e veja as tabelas na fonte de dados na página **Conectar-se à fonte de dados**.
1. Escolha a tabela **dimension_stock_item**, que contém registros de produtos. Em seguida, selecione **Avançar** para avançar para a página **Escolher destino de dados**.
1. Na página **Escolher destino de dados**, selecione o lakehouse existente. Em seguida, selecione **Avançar**.
1. Defina as seguintes opções de destino de dados e selecione **Avançar**:
    - **Pasta raiz**: Tabelas
    - **Carregar configurações**: Carregar em uma nova tabela
    - **Nome da tabela de destino**: dimension_stock_item
    - **Mapeamentos de coluna**: *deixe os mapeamentos padrão no estado em que se encontram*
    - **Habilitar partição**: *Não selecionado*
1. Na página **Revisar + salvar**, verifique se a opção **Iniciar transferência de dados imediatamente** está selecionada e escolha **Salvar + Executar**.

    Um pipeline que contém uma atividade **Copiar Dados** será criado, conforme mostrado aqui:

    ![Captura de tela de um pipeline com uma atividade Copiar Dados.](./images/copy-data-pipeline.png)

    Quando o pipeline começar a ser executado, você poderá monitorar o status dele no painel **Saída** no designer de pipeline. Use o ícone **&#8635;** (*Atualizar*) para atualizar o status e aguarde até que ele tenha sido concluído com sucesso.

1. Na barra de menus do hub à esquerda, selecione o lakehouse.
1. Na **Página inicial**, no painel do **Explorador do lakehouse**, expanda **Tabelas** e verifique se a tabela **dimension_stock_item** foi criada.

    > **Observação**: se a nova tabela estiver listada como *não identificada*, use o botão **Atualizar** na barra de ferramentas do lakehouse para atualizar a exibição.

1. Selecione a tabela **dimension_stock_item** para ver os conteúdos dela.

    ![Captura de tela da tabela dimension_stock_item.](./images/dimProduct.png)

## Consultar dados em um lakehouse

Agora que você ingeriu dados em uma tabela do lakehouse, use o SQL para consultá-los.

1. No canto superior direito da página Lakehouse, alterne para o **ponto de extremidade SQL** do lakehouse.

    ![Captura de tela do menu do ponto de extremidade do SQL.](./images/endpoint-switcher.png)

1. Na barra de ferramentas, selecione **Nova consulta SQL**. Em seguida, insira o seguinte código SQL no editor de consultas:

    ```sql
    SELECT Brand, COUNT(StockItemKey) AS Products
    FROM dimension_stock_item
    GROUP BY Brand
    ```

1. Selecione o botão **&#9655; Executar** para executar a consulta e analisar os resultados, o que revela que há dois valores de marca (*N/A* e *Northwind*) e mostra o número de produtos em cada um.

    ![Captura de tela de uma consulta SQL.](./images/sql-query.png)

## Visualizar dados em um lakehouse

Os lakehouses do Microsoft Fabric organizam todas as tabelas em um modelo de dados, que você pode usar para criar visualizações e relatórios.

1. No canto inferior esquerdo da página, no painel **Explorador**, selecione a guia **Modelo** para ver o modelo de dados das tabelas no lakehouse (nesse caso, há apenas uma tabela).

    ![Captura de tela da página de modelo em um lakehouse do Fabric.](./images/fabric-model.png)

1. Na barra de ferramentas, selecione **Novo relatório** para abrir uma nova guia do navegador que contém o designer de relatórios do Power BI.
1. No designer de relatório:
    1. No painel **Dados**, expanda a tabela **dimension_stock_item** e selecione os campos **Brand** e **StockItemKey**.
    1. No painel **Visualizações**, selecione a visualização **Gráfico de barras empilhadas** (a primeira listada). Em seguida, verifique se o **eixo Y** contém o campo **Brand** e altere a agregação no **eixo X** para **Count**, de modo que ele contenha o campo **Count of StockItemKey**. Por fim, redimensione a visualização na tela do relatório para preencher o espaço disponível.

        ![Captura de tela de um relatório do Power BI.](./images/fabric-report.png)

    > **Dica**: use os ícones **>>** para ocultar os painéis do designer de relatório para ver o relatório com mais clareza.

1. No menu **Arquivo**, selecione **Salvar** para salvar o relatório como **Relatório de Quantidade de Marcas** no workspace do Fabric.

    Feche a guia do navegador que contém o relatório para voltar ao lakehouse. Encontre o relatório na página do seu workspace no portal do Microsoft Fabric.

## Limpar os recursos

Se você terminou de explorar o Microsoft Fabric, exclua o workspace criado para este exercício.

1. Na barra à esquerda, selecione o ícone do workspace para ver todos os itens que ele contém.
2. No menu **…** da barra de ferramentas, selecione **Configurações do workspace**.
3. Na seção **Outros**, selecione **Remover este workspace**.
