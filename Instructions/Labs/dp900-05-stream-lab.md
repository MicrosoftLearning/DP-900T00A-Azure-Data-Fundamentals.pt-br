---
lab:
  title: Explorar o Azure Stream Analytics
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-azure-stream-analytics"></a>Explorar o Azure Stream Analytics

Neste exercício, você vai provisionar um trabalho do Azure Stream Analytics em sua assinatura do Azure e usá-lo para processar um fluxo de dados em tempo real.

Este laboratório levará aproximadamente **15** minutos para ser concluído.

## <a name="before-you-start"></a>Antes de começar

É necessário ter uma [assinatura do Azure](https://azure.microsoft.com/free) com acesso de nível administrativo.

## <a name="create-azure-resources"></a>Criar recursos do Azure

1. Entre em sua assinatura do Azure pelo [portal do Azure](https://portal.azure.com) usando as credenciais dela.

1. Use o botão **[\>_]** à direita da barra de pesquisa na parte superior da página para criar um Cloud Shell no portal do Azure, selecionando um ambiente ***Bash*** e criando um armazenamento caso solicitado. O Cloud Shell fornece uma interface de linha de comando em um painel na parte inferior do portal do Azure, conforme mostrado aqui:

    ![Portal do Azure com um painel do Cloud Shell](./images/cloud-shell.png)

1. No Azure Cloud Shell, digite o comando a seguir para baixar os arquivos necessários para este exercício.

    ```bash
    git clone https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals dp-900
    ```

1. Aguarde a conclusão do comando e digite o seguinte comando para alterar o diretório atual para a pasta que contém os arquivos deste exercício.

    ```bash
    cd dp-900/streaming
    ```

1. Insira o comando a seguir para executar um script que cria os recursos do Azure que você precisará neste exercício.

    ```bash
    bash setup.sh
    ```

    Aguarde enquanto o script é executado e executa as seguintes ações:

    1. Instala as extensões da CLI do Azure necessárias para criar recursos (*você pode ignorar quaisquer avisos sobre extensões experimentais*)
    1. Identifica o grupo de recursos do Azure fornecido para este exercício.
    1. Cria um recurso do *Hub IoT do Azure*, que será usado para receber um fluxo de dados de um dispositivo simulado.
    1. Cria uma *Conta de Armazenamento do Azure*, que será usada para armazenar dados processados.
    1. Cria um trabalho do *Azure Stream Analytics*, que processará os dados do dispositivo de entrada em tempo real e gravará os resultados na conta de armazenamento.

## <a name="explore-the-azure-resources"></a>Explorar os recursos do Azure

1. No [portal do Azure](https://portal.azure.com?azure-portal=true), na página inicial, selecione **Grupos de recursos** para ver os grupos de recursos em sua assinatura. O grupo de recursos **learn*xxxxxxxxxxxxxxxxxx...** *, identificado pelo script de configuração, deve estar incluído.
2. Selecione o grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * e revise os recursos dele, que devem incluir o seguinte:
    - Um *Hub IOT* chamado **iothub*xxxxxxxxxxxxx***, que é usado para receber dados de dispositivo de entrada.
    - Uma *conta de armazenamento* chamada **store*xxxxxxxxxxxx***, na qual os resultados do processamento de dados serão gravados.
    - Um *trabalho do Stream Analytics* chamado **stream*xxxxxxxxxxxxx***, que será usado para processar dados de streaming.

    Se todos esses três recursos não estiverem listados, clique no botão **&#8635; Atualizar** até que eles apareçam.

    > **Observação**: se você estiver usando a área restrita de aprendizado, o grupo de recursos também poderá conter uma segunda *Conta de armazenamento* chamada **cloudshell*xxxxxxxx***, que armazena os dados do Azure Cloud Shell usados para executar o script de configuração.

3. Selecione ao trabalho **stream*xxxxxxxxxxxxxxx*** do Stream Analytics e visualize as informações em sua página **Visão geral**, observando os seguintes detalhes:
    - O trabalho tem uma *entrada* chamada **iotinput** e uma *saída* chamada **bloboutput**. Elas fazem referência ao Hub IoT e à conta de armazenamento criada pelo script de configuração.
    - O trabalho tem uma *consulta*, que lê os dados da entrada **iotinput** e os agrega contando o número de mensagens processadas a cada 10 segundos; gravando os resultados na saída **bloboutput**.

## <a name="use-the-resources-to-analyze-streaming-data"></a>Usar os recursos para analisar dados de streaming

1. Na parte superior da página **Visão geral** do trabalho do Stream Analytics, selecione o botão **&#9655; Iniciar** e, no painel **Iniciar trabalho**, selecione **Iniciar** para iniciar o trabalho.
2. Aguarde uma notificação de que o trabalho de streaming foi iniciado com êxito.
3. Volte para o Azure Cloud Shell e insira o comando a seguir para simular um dispositivo que envia dados para o Hub IoT.

    ```
    bash iotdevice.sh
    ```

4. Aguarde o início da simulação, que será indicada por uma saída como esta:

    ```
    Device simulation in progress: 6%|#    | 7/120 [00:08<02:21, 1.26s/it]
    ```

5. Enquanto a simulação estiver em execução, volte ao portal do Azure, retorne à página do grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * e selecione a conta de armazenamento **store*xxxxxxxxxxxx***.
6. No painel à esquerda do painel da conta de armazenamento, selecione a guia **Contêineres**.
7. Abra o contêiner de **dados**.
8. No contêiner de **dados**, navegue pela hierarquia de pastas, que inclui uma pasta para o ano atual, com subpastas para o mês, dia e hora.
9. Na pasta da hora, selecione o arquivo que foi criado, que deve ter um nome semelhante a **0_xxxxxxxxxxxxxxxx.json**.
10. Na página do arquivo, selecione **Editar** e revise o conteúdo do arquivo; que deve consistir em um registro JSON para cada período de 10 segundos, mostrando o número de mensagens recebidas de dispositivos IoT, assim:

    ```
    {"starttime":"2021-10-23T01:02:13.2221657Z","endtime":"2021-10-23T01:02:23.2221657Z","device":"iotdevice","messages":2}
    {"starttime":"2021-10-23T01:02:14.5366678Z","endtime":"2021-10-23T01:02:24.5366678Z","device":"iotdevice","messages":3}
    {"starttime":"2021-10-23T01:02:15.7413754Z","endtime":"2021-10-23T01:02:25.7413754Z","device":"iotdevice","messages":4}
    ...
    ```

11. Use o botão **&#8635; Atualizar** para atualizar o arquivo, observando que resultados adicionais são gravados no arquivo à medida que o trabalho do Stream Analytics processa os dados do dispositivo em tempo real conforme são transmitidos do dispositivo para o Hub IoT.
12. Retorne ao Azure Cloud Shell e aguarde a conclusão da simulação do dispositivo (ela deve ser executada por cerca de 3 minutos).
13. De volta ao portal do Azure, atualize o arquivo mais uma vez para ver o conjunto completo de resultados que foram produzidos durante a simulação.
14. Retorne ao grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * e abra novamente o trabalho **stream*xxxxxxxxxxxxx*** do Stream Analytics.
15. Na parte superior da página de trabalho do Stream Analytics, use o botão **&#11036; Parar** para interromper o trabalho, confirmando quando solicitado.

> **Observação**: se você terminou de explorar a solução de streaming, exclua o grupo de recursos criado neste exercício.
