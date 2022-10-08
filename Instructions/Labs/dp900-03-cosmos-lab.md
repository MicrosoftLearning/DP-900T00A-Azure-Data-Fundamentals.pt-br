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

Para usar o Cosmos DB, você precisa provisionar uma conta do Cosmos DB na sua assinatura do Azure. Neste exercício, você provisionará uma conta do Cosmos DB que usa a API do Core (SQL).

1. No portal do Azure, selecione **+ Criar um recurso** na parte superior esquerda e procure por *Azure Cosmos DB*.  Nos resultados, selecione **Azure Cosmos DB** e, em seguida, **Criar**.
1. No bloco **Core (SQL) – Recomendado**, clique em **Criar**.
1. Insira os seguintes detalhes, depois selecione **Examinar + Criar**:
    - **Assinatura**: se você estiver usando uma área restrita, selecione *Assinatura do Concierge*. Caso contrário, escolha sua assinatura do Azure.
    - **Grupo de recursos**: se você estiver usando uma área restrita, selecione o grupo de recursos existente (que terá um nome como *learn-xxxx...* ). Caso contrário, crie um grupo de recursos com o nome de sua escolha.
    - **Nome da Conta**: insira um nome exclusivo
    - **Local**: escolha qualquer local recomendado
    - **Modo de capacidade**: taxa de transferência provisionada
    - **Aplicar desconto de Camada Gratuita**: selecione Aplicar se disponível
    - **Limitar a taxa de transferência total da conta**: não selecionado
1. Quando a configuração tiver sido validada, selecione **Criar**.
1. Aguarde o fim da implantação. Em seguida, vá para o recurso implantado.

## <a name="create-a-sample-database"></a>Criar banco de dados de exemplo

*Ao longo deste procedimento, feche todas as dicas exibidas no portal*.

1. Na página de sua nova conta do Cosmos DB, no painel à esquerda, selecione **Data Explorer**.
1. Na página **Data Explorer**, selecione **Lançar início rápido**.
1. Na guia **Novo contêiner**, examine as configurações preenchidas para o banco de dados de amostra e selecione **OK**.
1. Observe o status no painel na parte inferior da tela do banco de dados **SampleDB** e seu contêiner **SampleContainer** foi criado (o que pode levar aproximadamente um minuto).

## <a name="view-and-create-items"></a>Exibir e criar itens

1. Na página Data Explorer, expanda o banco de dados **SampleDB** e o contêiner **SampleContainer** e selecione **Itens** para ver uma lista de itens no contêiner. Os itens representam endereços, cada um com uma ID exclusiva e outras propriedades.
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

    Você viu como criar e consultar entidades JSON em um banco de dados do Cosmos DB usando a interface do Data Explorer no portal do Azure. Em um cenário real, um desenvolvedor de aplicativos usaria um dos vários SDKs (Software Development Kits) específicos da linguagem de programação para chamar a API do Core (SQL) e trabalhar com os dados no banco de dados.

> **Dica**: se você tiver concluído a exploração do Azure Cosmos DB, exclua o grupo de recursos criado neste exercício.
