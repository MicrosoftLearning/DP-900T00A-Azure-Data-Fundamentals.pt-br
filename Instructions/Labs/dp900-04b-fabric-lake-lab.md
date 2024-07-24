---
lab:
  title: Explorar a análise de dados no Microsoft Fabric
  module: Explore fundamentals of large-scale data analytics
---

# Explorar a análise de dados no Microsoft Fabric

Neste exercício, você vai explorar a ingestão e a análise de dados em um Microsoft Fabric Lakehouse.

Este laboratório levará aproximadamente **25** minutos para ser concluído.

> **Observação**: você precisará ter uma licença do Microsoft Fabric para concluir este exercício. Confira [Introdução ao Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial) para obter detalhes de como habilitar uma licença de avaliação gratuita do Fabric. Você precisará ter uma conta *corporativa* ou de *estudante* da Microsoft para fazer isso. Caso não tenha uma, [inscreva-se em uma avaliação do Microsoft Office 365 E3 ou superior](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

*Na primeira vez que você usar qualquer recurso do Microsoft Fabric, prompts com dicas poderão aparecer. Dispense-os.*

## Criar um workspace

Antes de trabalhar com os dados no Fabric, crie um workspace com a avaliação do Fabric habilitada.

1. Entre no [Microsoft Fabric](https://app.fabric.microsoft.com) em `https://app.fabric.microsoft.com`.
1. Na barra de menu, no canto inferior esquerdo, alterne para a experiência de **Engenharia de Dados**.

    ![Captura de tela do menu do alternador de experiência.](./images/fabric-switcher.png)

1. Na barra de menus à esquerda, selecione **Workspaces** (o ícone é semelhante a &#128455;).
1. Crie um workspace com um nome de sua escolha, selecionando um modo de licenciamento na seção **Avançado** que inclua a capacidade do Fabric (*Avaliação*, *Premium* ou *Malha*).
1. Quando o novo workspace for aberto, ele estará vazio.

    ![Captura de tela de um workspace vazio no Power BI.](./images/new-workspace.png)

## Criar um lakehouse

Agora que você tem um espaço de trabalho, é hora de criar um data lakehouse para seus arquivos de dados.

1. Na home page do workspace, crie um **Lakehouse** com um nome de sua escolha.

    Após alguns minutos, um lakehouse será criado:

    ![Captura de tela de um novo lakehouse.](./images/new-lakehouse.png)

1. Veja o novo lakehouse e observe que o painel do **Lakehouse Explorer** à esquerda permite que você navegue pelas tabelas e pelos arquivos no lakehouse:
    - A pasta **Tabelas** contém tabelas que você pode consultar usando o SQL. As tabelas de um lakehouse do Microsoft Fabric são baseadas no formato de arquivo *Delta Lake* de código aberto, comumente usado no Apache Spark.
    - A pasta **Arquivos** contém arquivos de dados no armazenamento OneLake para o lakehouse que não estão associados às tabelas delta gerenciadas. Você também pode criar *atalhos* nessa pasta para referenciar os dados armazenados externamente.

    Atualmente, não há tabelas nem arquivos no lakehouse.

## Ingerir dados

Uma forma simples de ingerir dados é usar uma atividade **Copiar Dados** em um pipeline para extrair os dados de uma fonte e copiá-los para um arquivo no lakehouse.

1. Na **Página Inicial** do Lakehouse, no menu **Obter dados**, clique em **Novo pipeline de dados** e crie um pipeline de dados chamado **Ingerir dados**.
1. No assistente **Copiar dados**, na página **Escolher uma fonte de dados**, clique em **Dados de exemplo** e, em seguida, selecione o conjunto de dados de exemplo **Táxi de Nova York – Verde**.

    ![Captura de tela da página Escolher fonte de dados.](./images/choose-data-source.png)

1. Veja as tabelas na fonte de dados na página **Conectar-se à fonte de dados**. Haverá uma tabela com os detalhes das viagens de táxi na cidade de Nova York. Em seguida, selecione **Avançar** para avançar para a página **Escolher destino de dados**.
1. Na página **Escolher destino de dados**, selecione o lakehouse existente. Em seguida, selecione **Avançar**.
1. Defina as seguintes opções de destino de dados e selecione **Avançar**:
    - **Pasta raiz**: Tabelas
    - **Carregar configurações**: Carregar em uma nova tabela
    - **Nome da tabela de destino**: taxi_rides *(talvez você precise aguardar a exibição da visualização dos mapeamentos de coluna antes de poder alterar isso)*
    - **Mapeamentos de coluna**: *deixe os mapeamentos padrão no estado em que se encontram*
    - **Habilitar partição**: *Não selecionado*
1. Na página **Revisar + salvar**, verifique se a opção **Iniciar transferência de dados imediatamente** está selecionada e escolha **Salvar + Executar**.

    Um pipeline que contém uma atividade **Copiar Dados** será criado, conforme mostrado aqui:

    ![Captura de tela de um pipeline com uma atividade Copiar Dados.](./images/copy-data-pipeline.png)

    Quando o pipeline começar a ser executado, você poderá monitorar o status dele no painel **Saída** no designer de pipeline. Use o ícone **&#8635;** (*Atualizar*) para atualizar o status e aguarde até a finalização (que pode demorar dez minutos ou mais).

1. Na barra de menus do hub à esquerda, selecione o lakehouse.
1. Na **Página Inicial**, no painel **Lakehouse Explorer** , no menu **...** do nó **Tabelas**, clique em **Atualizar** e expanda **Tabelas** para confirmar se a tabela **taxi_rides** foi criada.

    > **Observação**: se a nova tabela estiver listada como *não identificada*, use a opção do menu **Atualizar** para atualizar a exibição.

1. Selecione a tabela **taxi_rides** para ver o conteúdo dela.

    ![Captura de tela da tabela taxi_rides.](./images/dimProduct.png)

## Consultar dados em um lakehouse

Agora que você ingeriu dados em uma tabela do lakehouse, use o SQL para consultá-los.

1. No canto superior direito da página Lakehouse, alterne da exibição **Lakehouse** para o **ponto de extremidade de análise de SQL** do lakehouse.

1. Na barra de ferramentas, selecione **Nova consulta SQL**. Em seguida, insira o seguinte código SQL no editor de consultas:

    ```sql
    SELECT  DATENAME(dw,lpepPickupDatetime) AS Day,
            AVG(tripDistance) As AvgDistance
    FROM taxi_rides
    GROUP BY DATENAME(dw,lpepPickupDatetime)
    ```

1. Clique no botão **&#9655; Executar** para executar a consulta e examinar os resultados, que incluírão a distância média da viagem de  cada dia da semana.

    ![Captura de tela de uma consulta SQL.](./images/sql-query.png)

## Visualizar dados em um lakehouse

Os lakehouses do Microsoft Fabric organizam todas as tabelas em um modelo de dados semântico, que você pode usar para criar visualizações e relatórios.

1. No canto inferior esquerdo da página, no painel **Explorer**, clique na guia **Modelo** para ver o modelo de dados das tabelas no lakehouse (nesse caso, inclui as tabelas do sistema e a tabela **taxi_rides**).
1. Na barra de ferramentas, clique em **Novo relatório** para criar um novo relatório com base em **taxi_rides**.
1. No designer de relatório:
    1. No painel **Dados**, expanda a tabela **taxi_rides** e selecione os campos **lpepPickupDatetime** e **passengerCount**
    1. No painel **Visualizações**, clique na visualização **Gráfico de linhas**. Em seguida, verifique se o **eixo X** contém o campo **lpepPickupDatetime** e se o eixo **Y** contém **Soma de passengerCount**.

        ![Captura de tela de um relatório do Power BI.](./images/fabric-report.png)

    > **Dica**: use os ícones **>>** para ocultar os painéis do designer de relatório para ver o relatório com mais clareza.

1. No menu **Arquivo**, clique em **Salvar** para salvar o relatório como **Relatório de viagens de táxi** no workspace do Fabric.

    Feche a guia do navegador que contém o relatório para voltar ao lakehouse. Encontre o relatório na página do seu workspace no portal do Microsoft Fabric.

## Limpar os recursos

Se você terminou de explorar o Microsoft Fabric, exclua o workspace criado para este exercício.

1. Na barra à esquerda, selecione o ícone do workspace para ver todos os itens que ele contém.
2. No menu **…** da barra de ferramentas, selecione **Configurações do workspace**.
3. Na seção **Outros**, selecione **Remover este workspace**.
