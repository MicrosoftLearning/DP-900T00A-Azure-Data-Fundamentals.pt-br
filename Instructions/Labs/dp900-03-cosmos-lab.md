---
lab:
  title: Explorar o Azure Cosmos DB
  module: Explore fundamentals of Azure Cosmos DB
---
# <a name="explore-azure-cosmos-db"></a>Explorar o Azure Cosmos DB

Neste exercício, você vai provisionar um banco de dados do Azure Cosmos DB na assinatura do Azure e explorar as várias maneiras de usá-lo para armazenar dados não relacionais.

Este laboratório levará aproximadamente **15** minutos para ser concluído.

## <a name="before-you-start"></a>Antes de começar

É necessário ter uma [assinatura do Azure](https://azure.microsoft.com/free) com acesso de nível administrativo.

## <a name="create-a-cosmos-db-account"></a>Criar uma conta do BD Cosmos

To use Cosmos DB, you must provision a Cosmos DB account in your Azure subscription. In this exercise, you'll provision a Cosmos DB account that uses the core (SQL) API.

1. In the Azure portal, select <bpt id="p1">**</bpt>+ Create a resource<ept id="p1">**</ept> at the top left, and search for <bpt id="p2">*</bpt>Azure Cosmos DB<ept id="p2">*</ept>.  In the results, select <bpt id="p1">**</bpt>Azure Cosmos DB<ept id="p1">**</ept> and select  <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.
1. No bloco **Core (SQL) – Recomendado**, clique em **Criar**.
1. Insira os seguintes detalhes, depois selecione **Examinar + Criar**:
    - <bpt id="p1">**</bpt>Subscription<ept id="p1">**</ept>: If you're using a sandbox, select <bpt id="p2">*</bpt>Concierge Subscription<ept id="p2">*</ept>. Otherwise, select your Azure subscription.
    - **Grupo de recursos**: se você estiver usando uma área restrita, selecione o grupo de recursos existente (que terá um nome como *learn-xxxx...* ). Caso contrário, crie um grupo de recursos com o nome de sua escolha.
    - **Nome da Conta**: insira um nome exclusivo
    - **Local**: escolha qualquer local recomendado
    - **Modo de capacidade**: taxa de transferência provisionada
    - **Aplicar desconto de Camada Gratuita**: selecione Aplicar se disponível
    - **Limitar a taxa de transferência total da conta**: não selecionado
1. Quando a configuração tiver sido validada, selecione **Criar**.
1. Wait for deployment to complete. Then go to the deployed resource.

## <a name="create-a-sample-database"></a>Criar banco de dados de exemplo

*Ao longo deste procedimento, feche todas as dicas exibidas no portal*.

1. Na página de sua nova conta do Cosmos DB, no painel à esquerda, selecione **Data Explorer**.
1. Na página **Data Explorer**, selecione **Lançar início rápido**.
1. Na guia **Novo contêiner**, examine as configurações preenchidas para o banco de dados de amostra e selecione **OK**.
1. Observe o status no painel na parte inferior da tela do banco de dados **SampleDB** e seu contêiner **SampleContainer** foi criado (o que pode levar aproximadamente um minuto).

## <a name="view-and-create-items"></a>Exibir e criar itens

1. In the Data Explorer page, expand the <bpt id="p1">**</bpt>SampleDB<ept id="p1">**</ept> database and the <bpt id="p2">**</bpt>SampleContainer<ept id="p2">**</ept> container, and select <bpt id="p3">**</bpt>Items<ept id="p3">**</ept> to see a list of items in the container. The items represent addresses, each with a unique id and other properties.
1. Selecione qualquer um dos itens na lista para ver uma representação dos dados do item em JSON.
1. Na parte superior da página, selecione **Novo Item** para criar um item em branco.
1. Modifique o JSON para o novo item conforme mostrado a seguir e selecione **Salvar**.

    ```json
    {
        "address": "1 Any St.",
        "id": "123456789"
    }
    ```

1. Depois de salvar o novo item, observe que as propriedades de metadados adicionais são adicionadas automaticamente.

## <a name="query-the-database"></a>Consultar o banco de dados

1. Na página **Data Explorer**, selecione o ícone **Nova Consulta SQL**.
1. No editor de consultas SQL, examine a consulta padrão (`SELECT * FROM c`) e use o botão `SELECT * FROM c` para executá-la.
1. Examine os resultados, que incluem a representação JSON completa de todos os itens.
1. Modifique a consulta conforme o seguinte exemplo:

    ```sql
    SELECT c.id, c.address
    FROM c
    WHERE CONTAINS(c.address, "Any St.")
    ```

1. Use o botão **Executar Consulta** para executar a consulta revisada e examinar os resultados, que inclui entidades JSON para quaisquer itens com um campo de **endereço** que contenha o texto "Qualquer rua".
1. Feche o editor de consultas SQL, descartando suas alterações.

    You've seen how to create and query JSON entities in a Cosmos DB database by using the data explorer interface in the Azure portal. In a real scenario, an application developer would use one of the many programming language specific software development kits (SDKs) to call the core (SQL) API and work with data in the database.

> **Dica**: se você tiver concluído a exploração do Azure Cosmos DB, exclua o grupo de recursos criado neste exercício.
