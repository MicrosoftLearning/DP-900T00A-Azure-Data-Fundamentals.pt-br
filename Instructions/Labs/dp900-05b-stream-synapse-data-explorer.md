---
lab:
  title: Explorar o Azure Synapse Data Explorer
  module: Explore fundamentals of real-time analytics
---

# Explorar o Azure Synapse Data Explorer

> **Observação**: devido a alterações no produto, há alguns problemas conhecidos com a seção **Criar um banco de dados e ingerir dados** deste laboratório. Estamos trabalhando para resolver esses problemas.

Neste exercício, você usará o Azure Synapse Data Explorer para analisar dados de série temporal.

Este laboratório levará aproximadamente **25** minutos para ser concluído.

## Antes de começar

É necessário ter uma [assinatura do Azure](https://azure.microsoft.com/free) com acesso de nível administrativo.

## Provisionar um workspace do Synapse Analytics

> **Dica**: se você já tiver um workspace do Azure Synapse de um exercício anterior, ignore esta seção e acesse **[Criar um pool do Data Explorer](#create-a-data-explorer-pool)** .

1. Abra o portal do Azure em [https://portal.azure/com](https://portal.azure.com?azure-portal=true) e entre usando as credenciais associadas a sua assinatura do Azure.

    >                 **Observação**: verifique se você está trabalhando no diretório que contém a sua assinatura (indicado no canto superior direito embaixo da sua ID de usuário). Caso contrário, selecione o ícone de usuário e o troque o diretório.

1. Na portal do Azure, na **Página Inicial**, use o ícone **&#65291; Criar um recurso** para criar um recurso.
1. Pesquise por *Azure Synapse Analytics* e crie um recurso do **Azure Synapse Analytics** com as seguintes configurações:
    - **Assinatura**: *sua assinatura do Azure*
        - **Grupo de recursos**: *Criar um grupo de recursos com um nome apropriado, como "synapse-rg"*
        - **Grupo de recursos gerenciado**: *Insira um nome apropriado, por exemplo, "synapse-managed-rg"*.
    - **Nome do workspace**: *Insira um nome exclusivo para o workspace, por exemplo, "synapse-ws-<seu_nome>*.
    - **Região**: *Selecione qualquer região disponível*.
    - **Selecione o Data Lake Storage Gen 2**: Da assinatura
        - **Nome da conta**: *Crie conta com um nome exclusivo, por exemplo, "datalake<seu_nome>"*.
        - **Nome do sistema de arquivos**: *Crie um sistema de arquivos com um nome exclusivo, por exemplo "fs<seu_nome>"*.

    >                 **Observação**: um workspace do Synapse Analytics requer dois grupos de recursos na assinatura do Azure: um para recursos criados explicitamente e outro para recursos gerenciados que são usados ​​pelo serviço. Ele também requer uma conta de armazenamento de Data Lake para armazenar dados, scripts e outros artefatos.

1. Depois de inserir esses detalhes, selecione **Revisar + criar** e selecione **Criar** para criar o workspace.
1. Aguarde a criação do workspace, isso levará cerca de cinco minutos.
1. Quando a implantação for concluída, vá para o grupo de recursos que foi criado e observe que ele contém o workspace do Synapse Analytics e uma conta de armazenamento Data Lake.
1. Selecione o seu workspace do Synapse e a página de **Visão Geral** dele, no cartão do **Open Synapse Studio**, selecione **Abrir** para abrir o Synapse Studio em uma nova guia do navegador. O Synapse Studio é uma interface baseada na Web que pode ser usada para trabalhar com o seu workspace do Synapse Analytics.
1. No lado esquerdo do Synapse Studio, use o ícone **&rsaquo;&rsaquo;** para expandir o menu, o que revela as diferentes páginas do Synapse Studio usadas para gerenciar recursos e executar tarefas de análise de dados.

## Criar um pool do Data Explorer

1. No Synapse Studio, selecione a página **Gerenciar**.
1. Selecione a guia **Pools do Data Explorer** e use o ícone **&#65291; Novo** para criar um pool com as seguintes configurações:
    - **Nome do pool do Data Explorer**: dxpool
    - **Carga de trabalho**: computação otimizada
    - **Tamanho**: extra pequeno (2 núcleos)
1. Selecione **Avançar: Configurações adicionais >** e habilite a configuração de **Ingestão de streaming**. Isso permite que o Data Explorer ingira novos dados de uma fonte de streaming, como os Hubs de Eventos do Azure.
1. Selecione **Revisar e criar** para criar o pool do Data Explorer e aguarde até que ele seja implantado (o que pode levar 15 minutos ou mais – o status mudará de *Criando* para *Online*).

## Criar um banco de dados e ingerir dados

1. No Synapse Studio, selecione a página **Dados**.
1. Verifique se a guia **Workspace** está selecionada e, se necessário, selecione o ícone **&#8635;** no canto superior esquerdo da página para atualizar a visualização a fim de que os **Bancos de dados do Data Explorer** sejam listados.
1. Expanda **bancos de dados do Data Explorer** e verifique se **dxpool** está listado.
1. No painel **Dados**, use o ícone **&#65291;** para criar um **Banco de dados do Data Explorer** no pool **dxpool** com o nome **iot-data**.
1. Enquanto aguarda a criação do banco de dados, baixe **devices.csv** de [https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv?azure-portal=true) e salve-o em qualquer pasta do computador local.
1. No Synapse Studio, aguarde até que o banco de dados seja criado, se necessário, e no menu **...** do banco de dados **iot-data**, selecione **Abrir no Azure Data Explorer**.
1. Na nova guia do navegador que contém o Azure Data Explorer, na guia **Dados**, selecione **Ingerir novos dados**.
1. Na página **Destino**, selecione as seguintes configurações:
    - **Cluster**: *o pool do Data Explorer **dxpool** em seu workspace do Azure Synapse*
    - **Banco de dados**: iot-data
    - **Tabela**: criar uma nova tabela chamada **dispositivos**
1. Selecione **Próximo: origem** e, na página **Origem**, selecione as seguintes opções:
    - **Tipo de origem**: arquivo
    - **Arquivos**: carregue o arquivo **devices.csv** no computador local.
1. Selecione **Próximo: esquema** e, na página **Esquema**, verifique se as seguintes configurações estão corretas:
    - **Tipo de compactação**: descompactado
    - **Formato dos dados**: CSV
    - **Ignorar o primeiro registro**: selecionado
    - **Mapeamento**: devices_mapping
1. Verifique se os tipos de dados de coluna foram identificados corretamente como *Time (datetime)*, *Device (string)* e *Value (long)*). Selecione **Avançar: iniciar ingestão**.
1. Quando a ingestão for concluída, selecione **Fechar**.
1. No Azure Data Explorer, na guia **Consulta**, verifique se o banco de dados **iot-data** está selecionado e, no painel de consulta, insira a consulta a seguir.

    ```kusto
    devices
    ```

1. Na barra de ferramentas, selecione **&#9655; Executar** para executar a consulta e revise os resultados, que devem ser semelhantes ao seguinte:

    | Hora | Dispositivo | Valor |
    | --- | --- | --- |
    | 2022-01-01T00:00:00Z | Dev1 | 7 |
    | 2022-01-01T00:00:01Z | Dev2 | 4 |
    | ... | ... | ... |

    Se os resultados corresponderem a isso, você criou com êxito a tabela **dispositivos** dos dados no arquivo.

    >                 **Dica**: neste exemplo, uma quantidade muito pequena de dados em lote foi importada de um arquivo, o que é útil para os propósitos do exercício. Na realidade, você pode usar o Data Explorer para analisar volumes muito maiores de dados; como você habilita a ingestão de fluxo, também pode ter configurado o Data Explorer para ingerir dados na tabela de uma fonte de streaming, como Hubs de Eventos do Azure.

## Use a linguagem de consulta Kusto para consultar a tabela no Synapse Studio

1. Feche a guia do navegador do Azure Data Explorer e retorne à guia que contém o Synapse Studio.
1. Na página **Dados**, expanda o banco de dados **iot-data** e sua pasta **Tabelas**. No menu **...**, para a tabela **dispositivos**, selecione **Novo script KQL** > **Tomar 1.000 linhas**.
1. Revise a consulta gerada e seus resultados. A consulta deve conter o seguinte código:

    ```kusto
    devices
    | take 1000
    ```

    Os resultados da consulta contêm as primeiras 1.000 linhas de dados.

1. Modifique a consulta conforme o seguinte exemplo:

    ```kusto
    devices
    | where Device == 'Dev1'
    ```

1. Selecione **&#9655; Executar** para executar a consulta. Revise os resultados, que devem conter apenas as linhas do dispositivo *Dev1*.

1. Modifique a consulta conforme o seguinte exemplo:

    ```kusto
    devices
    | where Device == 'Dev1'
    | where Time > datetime(2022-01-07)
    ```

1. Execute a consulta e revise os resultados, que devem conter apenas as linhas do dispositivo *Dev1* depois de 7 de janeiro de 2022.

1. Modifique a consulta conforme o seguinte exemplo:

    ```kusto
    devices
    | where Time between (datetime(2022-01-01 00:00:00) .. datetime(2022-07-01 23:59:59))
    | summarize AvgVal = avg(Value) by Device
    | sort by Device asc
    ```

1. Execute a consulta e revise os resultados, que devem conter o valor médio do dispositivo registrado entre 1º de janeiro e 7 de janeiro de 2022 em ordem crescente do nome do dispositivo.

1. Feche a guia consulta KQL, descartando as alterações.

## Excluir recursos do Azure

Agora que você terminou de explorar Azure Synapse Analytics, exclua os recursos que criou para evitar custos desnecessários do Azure.

1. Feche a guia do navegador do Synapse Studio, sem salvar nenhuma alteração, e retorne ao portal do Azure.
1. No portal do Azure, na **Página Inicial**, selecione **Grupos de recursos**.
1. Selecione o grupo de recursos para o workspace do Synapse Analytics (não o grupo de recursos gerenciado) e verifique se ele contém o workspace do Synapse, a conta de armazenamento e o pool do Data Explorer para seu workspace (se você concluiu o exercício anterior, ele também conterá um pool do Spark).
1. Na parte superior da página de **Visão Geral** do grupo de recursos, selecione **Excluir o grupo de recursos**.
1. Digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir**.

    Após alguns minutos, seu workspace do Azure Synapse e o workspace gerenciado associado a ele serão excluídos.
