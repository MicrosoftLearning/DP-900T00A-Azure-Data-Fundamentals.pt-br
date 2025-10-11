---
lab:
  title: Explorar o Banco de Dados SQL do Azure
  module: Explore relational data in Azure
---

# Explorar o Banco de Dados SQL do Azure

Neste laboratório, você aprenderá a provisionar um Banco de Dados SQL do Azure e interagir com ele usando consultas SQL. Você usará o banco de dados de amostra AdventureWorks da Microsoft, que fornece tabelas e dados pré-preenchidos, para que você possa se concentrar em explorar e consultar dados relacionais sem precisar criar seu esquema ou inserir registros de exemplo. Essa abordagem mantém as coisas simples e permite que você se concentre em entender os principais conceitos do banco de dados e da sintaxe do SQL.

Este laboratório levará aproximadamente **15** minutos para ser concluído.

## Antes de começar

É necessário ter uma [assinatura do Azure](https://azure.microsoft.com/free) com acesso de nível administrativo.

## Provisionar um recurso do Banco de Dados SQL do Azure

1. No [Portal do Azure](https://portal.azure.com?azure-portal=true), selecione **&#65291; Criar recurso** no canto superior esquerdo e pesquise `Azure SQL`. Em seguida, na página **SQL do Azure** resultante, selecione **Criar**.

    ![Captura de tela do Portal do Azure mostrando o SQL do Azure no marketplace](images/azure-sql-marketplace.png)

1. Examine as opções de SQL do Azure que estão disponíveis e, em seguida, no bloco **Bancos de dados SQL**, verifique se a opção **Banco de dados individual** está selecionada e escolha **Criar**.

    ![Captura de tela do portal do Azure mostrando a página do SQL do Azure.](images/azure-sql-portal.png)

    > _**Dica**: o banco de dados individual é o mais simples e rápido de configurar para este laboratório. As outras opções adicionam configurações de que você ainda não precisa._

1. Insira os seguintes valores na página **Criar Banco de Dados SQL** e deixe todas as outras propriedades com a configuração padrão:
    - **Assinatura**: Selecione sua assinatura do Azure.
    - **Grupo de recursos**: crie um grupo de recursos com um nome de sua escolha.
    - **Nome do banco de dados**: `AdventureWorks`
    - **Servidor**: selecione **Criar novo** e crie um servidor com um nome exclusivo em qualquer local disponível. Use **Autenticação de SQL** e especifique seu nome como o logon de administrador do servidor e uma senha devidamente complexa (lembre-se da senha, você precisará dela mais tarde).
    - **Deseja usar o pool elástico do SQL?**: *Não*
    - **Ambiente de carga de trabalho**: desenvolvimento
    - **Computação + armazenamento**: deixar inalterado
    - **Redundância de armazenamento de backup**: *Armazenamento de backup com redundância local*

    > _**Dica**: a autenticação SQL é rápida de configurar para a última (sem etapas adicionais do Microsoft Entra ID). Os padrões de desenvolvimento são mais baratos e rápidos. O backup local é a opção de baixo custo e funciona bem para um banco de dados de prática temporário._

1. Na página **Criar Banco de Dados SQL**, selecione **Avançar: Redes >** e, na página **Redes**, na seção **Conectividade de rede**, selecione **Ponto de extremidade público**. Em seguida, selecione **Sim** para ambas as opções na seção **Regras de firewall** para permitir o acesso ao servidor de banco de dados por meio dos serviços do Azure e do seu endereço IP de cliente atual.

    ![Captura de tela do Portal do Azure mostrando as configurações de rede do banco de dados SQL](images/sql-database-network.png)

    > _**Dica**: o ponto de extremidade público + permitir seu IP permite que você se conecte imediatamente. Isso é bom para um laboratório curto. Em projetos reais, você normalmente bloqueia mais o acesso._

1. Selecione **Avançar: Segurança >** e configure a opção **Habilitar Microsoft Defender para SQL** como **Não agora**.

    > _**Dica**: o Defender é um complemento de segurança pago. Nós o ignoramos aqui para manter as coisas simples e evitar custos em um exercício curto._

1. Selecione **Avançar: Configurações adicionais >** e, na guia **Configurações adicionais**, configure a opção **Usar dados existentes** como **Amostra** (isso criará um banco de dados de exemplo que pode ser explorado posteriormente).

    > _**Dica**: os dados de amostra fornecem tabelas e linhas prontas para que você possa começar a consultar imediatamente._

1. Selecione **Examinar + Criar** e depois **Criar** para criar o Banco de Dados SQL do Azure.

1. Aguarde o fim da implantação. Em seguida, acesse o recurso que foi implantado, que deve ter essa aparência:

    ![Captura de tela do portal do Azure mostrando a página do Banco de Dados SQL.](images/sql-database-portal.png)

1. No painel no lado esquerdo da página, selecione **Editor de consultas (versão prévia)** e, em seguida, entre usando as credenciais de administrador e a senha que você especificou para o servidor.
    
    >**Observação**: se uma mensagem de erro informando que o endereço IP do cliente não é permitido for exibida, selecione o link **Incluir o IP na lista de permitidos...** no final da mensagem para permitir o acesso e tente entrar novamente (anteriormente, você adicionou o endereço IP do cliente do seu computador às regras de firewall, mas o editor de consultas pode se conectar por meio de um endereço diferente, dependendo da configuração da rede).
    
    O editor de consultas tem essa aparência:
    
    ![Uma captura de tela do portal do Azure mostrando o editor de consultas.](images/query-editor.png)

1. Expanda a pasta **Tabelas** para ver as tabelas no banco de dados.

1. No painel **Consulta 1**, insira a seguinte instrução SQL:

    ```sql
   SELECT * FROM SalesLT.Product;
    ```

    > _**Dica**: SELECT * mostra rapidamente todas as colunas e alguns valores. (Em aplicativos reais, você geralmente o evita e escolhe apenas as colunas de que precisa.)_

1. Selecione **&#9655; Executar** acima da consulta para executá-la e visualizar os resultados, que devem incluir todas as colunas de todas as linhas na tabela **SalesLT.Product**, conforme mostrado abaixo:

    ![Captura de tela do portal do Azure mostrando o editor de consultas com os resultados da consulta.](images/sql-query-results.png)

1. Substitua a instrução SELECT pelo seguinte código e selecione **&#9655; Executar** para executar a nova consulta e revisar os resultados (que incluem somente as colunas **ProductID**, **Name**, **ListPrice** e **ProductCategoryID**):

    ```sql
   SELECT ProductID, Name, ListPrice, ProductCategoryID
   FROM SalesLT.Product;
    ```

    > _**Dica**: listar apenas as colunas necessárias mantém os resultados menores e pode ser executado mais rapidamente._

1. Agora, experimente a seguinte consulta, que usa uma JOIN para obter o nome da categoria da tabela **SalesLT.ProductCategory**:

    ```sql
    SELECT 
        p.ProductID, 
        p.Name AS ProductName,
        c.Name AS Category, 
        p.ListPrice
    FROM SalesLT.Product AS p
    INNER JOIN SalesLT.ProductCategory AS c 
        ON p.ProductCategoryID = c.ProductCategoryID;
    ```

    > _**Dica**: JOIN mostra como extrair dados relacionados (o nome da categoria) de outra tabela usando um ID correspondente._

1. Feche o painel do editor de consultas, descartando suas edições.

> _**Dica**: se você tiver concluído a exploração do Banco de Dados SQL do Azure, exclua o grupo de recursos criado neste exercício. A exclusão do grupo de recursos remove todos os recursos em uma etapa. Ela também minimiza o custo._
