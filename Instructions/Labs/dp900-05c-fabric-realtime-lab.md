---
lab:
  title: Explorar a análise em tempo real no Microsoft Fabric
  module: Explore real-time analytics in Microsoft Fabric
---

# Explorar a análise em tempo real no Microsoft Fabric

O Microsoft Fabric fornece inteligência em tempo real, permitindo criar soluções analíticas para fluxos de dados em tempo real. Neste exercício, você usará os recursos de inteligência em tempo real no Microsoft Fabric para ingerir, analisar e visualizar um fluxo em tempo real de dados de uma empresa de táxi.

Este laboratório leva cerca de **30** minutos para ser concluído.

> **Observação**: para concluir este exercício, você precisa de um [locatário do Microsoft Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial).

## Criar um workspace

Antes de trabalhar com os dados no Fabric, você precisa criar um espaço de trabalho em um locatário com a funcionalidade do Fabric habilitada.

> _**Dica**: um workspace é o contêiner para tudo o que você cria (eventstreams, eventhouses, dashboards). A capacidade do Fabric permite que esses itens sejam executados._

1. Navegue até a [home page do Microsoft Fabric](https://app.fabric.microsoft.com/home?experience=fabric) em `https://app.fabric.microsoft.com/home?experience=fabric` em um navegador e entre com suas credenciais do Fabric.

1. Na barra de menus à esquerda, selecione **Workspaces** (o ícone é semelhante a &#128455;).

1. Crie um workspace com um nome de sua escolha selecionando um modo de licenciamento que inclua a capacidade do Fabric (*Avaliação*, *Premium* ou *Malha*).

    > _**Dica**: o uso de uma capacidade que inclui o Fabric garante que o workspace tenha os mecanismos necessários para ingestão e análise em tempo real. Um workspace separado mantém os recursos do laboratório isolados e fáceis de limpar._

1. Quando o novo workspace for aberto, ele estará vazio.

    ![Captura de tela de um espaço de trabalho vazio no Fabric.](./images/new-workspace.png)

## Criar um fluxo de eventos

Agora você está pronto para encontrar e ingerir dados em tempo real de uma fonte de streaming. Para fazer isso, você começará no hub em tempo real do Fabric.

> _**Dica**: o Hub em Tempo Real centraliza as fontes de streaming e facilita a conexão delas. Um eventstream costura fontes a destinos e pode adicionar transformações entre eles._

> **Dica**: na primeira vez que você usar o hub em tempo real, algumas dicas de *introdução* podem ser exibidas. Você pode fecha-las.

1. Na barra de menus à esquerda, selecione o hub em **tempo real**.

    O hub em tempo real fornece uma maneira fácil de encontrar e gerenciar fontes de dados de streaming.

    ![Captura de tela do hub em tempo real no Fabric.](./images/real-time-hub.png)

1. No hub em tempo real, na seção **Conectar-se a**, selecione **Fontes de dados**.

1. Encontre a fonte de dados de amostra **Táxi amarelo** e selecione **Conectar**. Em seguida, no assistente **Conexão**, nomeie a fonte como `taxi` e edite o nome padrão do eventstream para alterá-lo para `taxi-data`. O fluxo padrão associado a esses dados será automaticamente denominado *taxi-data-stream*:

    > _**Dica**: a amostra de táxi amarelo é um fluxo público seguro — não são necessárias credenciais — e é consistente para todos os aprendizes. Nomes claros facilitam a localização da fonte, do eventstream e do fluxo posterior._

    ![Captura de tela de um novo eventstream.](./images/name-eventstream.png)

1. Selecione **Avançar** e aguarde a criação da fonte e do eventstream e, em seguida, selecione **Abrir eventstream**. O eventstream mostrará a fonte **táxi** e o **taxi-data-stream** na tela de design:

   ![Captura de tela da tela do eventstream.](./images/new-taxi-stream.png)

## Criar uma eventhouse

O eventstream ingere os dados de táxi em tempo real, mas atualmente não faz nada com eles. Vamos criar um eventhouse onde podemos armazenar os dados capturados em uma tabela.

> _**Dica**: uma eventhouse fornece um armazenamento durável e um banco de dados KQL para que você possa persistir o fluxo e consultá-lo mais tarde — mesmo conforme novos eventos chegarem. O KQL (Linguagem de Consulta Kusto) é uma linguagem somente leitura, semelhante ao SQL, usada para explorar, filtrar e analisar rapidamente grandes conjuntos de dados_

1. Na barra de menus à esquerda, selecione **Criar**. Na página *Novo*, na seção *Inteligência em Tempo Real*, selecione **Eventhouse**. Dê um nome exclusivo de sua preferência.

    >**Observação**: se a opção **Criar** não estiver fixada na barra lateral, você precisará selecionar a opção de reticências (**...**) primeiro.

    ![Captura de tela de uma opção de criação na barra lateral](./images/fabric-create.png)

    Feche todas as dicas ou prompts exibidos até ver o novo eventhouse vazio.

    ![Captura de tela de um novo eventhouse](./images/create-eventhouse.png)

1. No painel à esquerda, o eventhouse contém um banco de dados KQL com o mesmo nome do eventhouse. Você pode criar tabelas para seus dados em tempo real neste banco de dados ou criar bancos de dados adicionais conforme necessário.

1. Selecione o banco de dados e observe que há um *queryset* associado. Esse arquivo contém algumas consultas KQL de exemplo que você pode usar para começar a consultar as tabelas em seu banco de dados.

    > _**Dica**: o banco de dados KQL contém suas tabelas. O conjunto de consultas é um local útil para gravar e executar consultas sem configuração extra._

    No entanto, atualmente não há tabelas para consultar. Vamos resolver esse problema obtendo dados do eventstream em uma nova tabela.

1. Na página principal do banco de dados KQL, selecione **Obter dados**.

    ![Captura de tela de uma eventhouse vazia](./images/fabric-empty-eventhouse.png)

1. Para a fonte de dados, selecione **Eventstream** > **Eventstream existente**.

1. No painel **Selecionar ou criar uma tabela de destino**, crie uma nova tabela chamada `taxi`. Em seguida, no painel **Configurar a fonte de dados**, selecione seu espaço de trabalho e o eventstream **taxi-data** e nomeie a conexão `taxi-table`.

   ![Captura de tela da configuração para carregar uma tabela de um eventstream.](./images/configure-destination.png)

1. Use o botão **Avançar** para concluir as etapas para inspecionar os dados e, em seguida, **concluir** a configuração. Em seguida, feche a janela de configuração para ver sua eventhouse com a tabela de táxi.

   ![Captura de tela de um eventhouse com uma tabela.](./images/eventhouse-with-table.png)

    A conexão entre o fluxo e a tabela foi criada. Vamos verificar isso no eventstream.

1. Na barra de menus à esquerda, selecione o hub em **tempo real** e exiba a página **Meus fluxos de dados**. No menu **...** do fluxo **taxi-data-stream**, selecione **Abrir eventstream**.

    O eventstream agora mostra um destino para o fluxo:

   ![Captura de tela de um eventstream com um destino.](./images/eventstream-destination.png)

    > _**Dica**: selecione o destino na tela de design e, se nenhuma visualização de dados for mostrada abaixo dele, selecione **Atualizar**._

    > _**Dica**: verificar no eventstream confirma se os eventos estão fluindo para o destino. A visualização pode armazenar em cache — a atualização efetua pull da amostra mais recente._

    Neste exercício, você criou um eventstream muito simples que captura dados em tempo real e os carrega em uma tabela. Em uma solução real, você normalmente adicionaria transformações para agregar os dados em janelas temporais (por exemplo, para capturar o preço médio de cada ação em períodos de cinco minutos).

    Agora vamos explorar como você pode consultar e analisar os dados capturados.

## Consultar os dados capturados

O eventstream captura dados de tarifas de táxi em tempo real e os carrega em uma tabela no seu banco de dados KQL. Você pode consultar esta tabela para ver os dados capturados.

> _**Dica**: o KQL foi criado para exploração rápida de dados de alto volume com carimbo de data/hora. A consulta permite validar a ingestão e iniciar a análise imediatamente._

1. Na barra de menus à esquerda, selecione o banco de dados da sua casa de eventos.

1. Selecione o *queryset* do banco de dados.

1. No painel de consulta, modifique a primeira consulta de exemplo, conforme mostrado aqui:

    ```kql
    taxi
    | take 100
    ```

    > _**Dica**: `take 100` é uma verificação rápida de integridade — confirme se as linhas estão chegando e inspecione uma pequena amostra sem escanear tudo._

1. Selecione o código de consulta e execute-o para ver 100 linhas de dados na tabela.

    ![Captura de tela de uma consulta KQL.](./images/kql-stock-query.png)

1. Revise os resultados e modifique a consulta para mostrar o número de embarques em táxis em cada hora:

    ```kql
    taxi
    | summarize PickupCount = count() by bin(todatetime(tpep_pickup_datetime), 1h)
    ```

1. Realce a consulta modificada e execute-a para ver os resultados.

    > _**Dica**: `bin(..., 1h)` agrupa eventos em buckets de hora em hora, facilitando a identificação de tendências ao longo do tempo._

1. Aguarde alguns segundos e execute-a novamente, observando que o número de embarques muda à medida que novos dados são adicionados à tabela a partir do fluxo em tempo real.

    > _**Dica**: o fluxo continua adicionando dados, portanto, os resultados mudam ao longo do tempo. Executar novamente mostra como as agregações são atualizadas à medida que novos eventos chegam._

## Limpar os recursos

Neste exercício, você criou um eventhouse, ingeriu dados em tempo real usando um eventstream, consultou os dados ingeridos em uma tabela de banco de dados KQL, criou um painel em tempo real para visualizar os dados em tempo real e configurou um alerta usando o Ativador.

Se você terminou de explorar a Inteligêmcia em tempo real no Fabric, exclua o workspace criado para este exercício.

> _**Dica**: A exclusão do workspace remove todos os itens criados no laboratório e ajuda a evitar cobranças contínuas._

1. Na barra à esquerda, selecione o ícone do seu workspace.

1. Na barra de ferramentas, selecione **Configurações do espaço de trabalho**.

1. Na seção **Geral**, selecione **Remover este espaço de trabalho**.
