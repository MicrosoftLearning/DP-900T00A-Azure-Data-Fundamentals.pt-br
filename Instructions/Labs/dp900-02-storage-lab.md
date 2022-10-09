---
lab:
  title: Explorar o Armazenamento do Azure
  module: Explore Azure Storage for non-relational data
---

# <a name="explore-azure-storage"></a>Explorar o Armazenamento do Azure

Neste exercício, você vai provisionar uma conta do Armazenamento do Azure na assinatura do Azure e explorar as várias maneiras de usá-la para armazenar dados.

Este laboratório levará aproximadamente **15** minutos para ser concluído.

## <a name="before-you-start"></a>Antes de começar

É necessário ter uma [assinatura do Azure](https://azure.microsoft.com/free) com acesso de nível administrativo.

## <a name="provision-an-azure-storage-account"></a>Provisionar uma conta do Armazenamento do Azure

A primeira etapa ao usar o Armazenamento do Azure é provisionar uma conta do Armazenamento do Azure em sua assinatura do Azure.

1. Se você ainda não tiver feito isso, entre no [portal do Azure](https://portal.azure.com?azure-portal=true).
1. Na home page do portal do Azure, selecione **&#65291; Criar recurso** no canto superior esquerdo e pesquise a *conta de armazenamento*. Em seguida, na página **Conta de armazenamento** resultante, selecione **Criar**.
1. Insira os seguintes valores na página **Criar conta de armazenamento**:
    - **Assinatura**: Selecione sua assinatura do Azure.
    - **Grupo de recursos**: crie um grupo de recursos com um nome de sua escolha.
    - **Nome da conta de armazenamento**: insira um nome exclusivo para sua conta de armazenamento usando letras minúsculas e números.
    - **Região**: selecione qualquer local disponível.
    - **Desempenho**: *padrão*
    - **Redundância**: *LRS (armazenamento com redundância local)*

1. Selecione **Avançar: Avançado >** e veja as opções de configuração avançada. Em particular, observe que é aqui que você pode habilitar o namespace hierárquico para dar suporte ao Azure Data Lake Storage Gen2. Deixe essa opção **<u>desmarcada</u>** (ela será habilitada posteriormente) e selecione **Avançar: Rede >** para visualizar as opções de rede da conta de armazenamento.
1. Selecione **Avançar: Proteção de dados >** e, na seção **Recuperação**, <u>desmarque</u> todas as opções **Habilitar exclusão temporária...** . Essas opções retêm os arquivos excluídos para recuperação subsequente, mas podem causar problemas posteriormente quando você habilitar o namespace hierárquico.
1. Continue avançando pelas páginas restantes com **Avançar >** sem alterar nenhuma das configurações padrão e, na página **Revisar + Criar**, aguarde a validação de suas seleções e selecione **Criar** para criar a conta de Armazenamento do Azure.
1. Aguarde o fim da implantação. Em seguida, vá para o recurso que foi implantado.

## <a name="explore-blob-storage"></a>Explorar o armazenamento de blobs

Agora que você tem uma conta de armazenamento do Azure, pode criar um contêiner para dados de blob.

1. Baixe o arquivo JSON [product1.json](https://aka.ms/product1.json?azure-portal=true) de `https://aka.ms/product1.json` e salve-o em seu computador (você pode salvá-lo em qualquer pasta, ele será carregado no armazenamento de blobs posteriormente).

    *Se o arquivo JSON for exibido no navegador, salve a página como **product1.json**.*

1. Na página do portal do Azure para seu contêiner de armazenamento, no lado esquerdo, na seção **Armazenamento de dados**, selecione **Contêineres**.
1. Na página **Contêineres**, selecione **&#65291; Contêiner** e adicione um contêiner chamado **dados** com um nível de acesso público **Privado (sem acesso anônimo)** .
1. Quando o contêiner de **dados** for criado, verifique se ele está listado na página **Contêineres**.
1. No painel do lado esquerdo, na seção superior, selecione **Navegador de armazenamento**. Esta página fornece uma interface baseada em navegador que você pode usar para trabalhar com os dados em sua conta de armazenamento.
1. Na página do navegador de armazenamento, selecione **Contêineres de blob** e verifique se seu contêiner de **dados** está listado.
1. Selecione o contêiner de **dados** e observe que ele está vazio.
1. Selecione **&#65291; Adicionar diretório** e leia as informações sobre pastas antes de criar um diretório chamado **produtos**.
1. No navegador do armazenamento, verifique se o modo de exibição atual mostra o conteúdo da pasta **produtos** recém-criada. Observe que as "trilhas de navegação" na parte superior da página refletem o caminho **Contêineres de blobs > dados > produtos**.
1. Nas trilhas, selecione **dados** para alternar para o contêiner de **dados** e observe que ele <u>não</u> contém uma pasta chamada **produtos**.

    As pastas no armazenamento de blob são virtuais e existem apenas como parte do caminho de um blob. Como a pasta de **produtos** não continha blobs, ela não está realmente lá!

1. Use o botão **&#10514; Carregar** para abrir o painel **Carregar blob**.
1. No painel **Carregar blob**, selecione o arquivo **product1.json** que você salvou em seu computador local anteriormente. Em seguida, na seção **Avançado**, na caixa **Carregar na pasta**, insira **product_data** e selecione o botão **Carregar**.
1. Feche o painel **Carregar blob** se ainda estiver aberto e verifique se uma pasta virtual **product_data** foi criada no contêiner de **dados**.
1. Selecione a pasta **product_data** e verifique se ela contém o blob **product1.json** que você carregou.
1. No lado esquerdo, na seção **Armazenamento de dados**, selecione **Contêineres**.
1. Abra o contêiner de **dados** e verifique se a pasta **product_data** que você criou está listada.
1. Selecione o ícone **&#x2027;&#x2027;&#x2027;** na extremidade direita da pasta e observe que nenhuma opção é exibida. As pastas em um contêiner de blobs de namespace simples são virtuais e não podem ser gerenciadas.
1. Use o ícone **X** no canto superior direito da página de **dados** para fechar a página e retornar à página **Contêineres**.

## <a name="explore-azure-data-lake-storage-gen2"></a>Explorar Azure Data Lake Storage Gen2

O suporte do Azure Data Lake Store Gen2 permite que você use pastas hierárquicas para organizar e gerenciar o acesso a blobs. Ele também permite que você use o armazenamento de blobs do Azure para hospedar sistemas de arquivos distribuídos para plataformas comuns de análise de Big Data.

1. Baixe o arquivo JSON [product2.json](https://aka.ms/product2.json?azure-portal=true) de `https://aka.ms/product2.json` e salve-o em seu computador na mesma pasta em que você baixou o **product1.json** anteriormente, ele será carregado no armazenamento de blobs mais adiante).
1. Na página do portal do Azure da conta de armazenamento, no lado esquerdo, role para baixo até a seção **Configurações** e selecione **Atualização do Data Lake Gen2**.
1. Na página ****Atualização do Data Lake Gen2****, expanda e conclua cada etapa para atualizar a conta de armazenamento a fim de habilitar o namespace hierárquico e dar suporte ao Azure Data Lake Storage Gen 2. Isso pode levar algum tempo.
1. Quando a atualização estiver concluída, no painel à esquerda, na seção superior, selecione **Navegador de armazenamento** e volte à raiz do contêiner de blobs **dados**, que ainda contém a pasta **product_data**.
1. Selecione a pasta **product_data** e verifique se ela ainda contém o arquivo **product1.json** que você carregou anteriormente.
1. Use o botão **&#10514; Carregar** para abrir o painel **Carregar blob**.
1. No painel **Carregar blob**, selecione o arquivo **product2.json** que você salvou em seu computador local. Em seguida, selecione o botão **Carregar**.
1. Feche o painel **Carregar blob** se ainda estiver aberto e verifique se uma pasta **product_data** agora contém o arquivo **product2.json**.
1. No lado esquerdo, na seção **Armazenamento de dados**, selecione **Contêineres**.
1. Abra o contêiner de **dados** e verifique se a pasta **product_data** que você criou está listada.
1. Selecione o ícone **&#x2027;&#x2027;&#x2027;** na extremidade direita da pasta e observe que, com o namespace hierárquico habilitado, é possível executar tarefas de configuração no nível da pasta, incluindo renomear pastas e definir permissões.
1. Use o ícone **X** no canto superior direito da página de **dados** para fechar a página e retornar à página **Contêineres**.

## <a name="explore-azure-files"></a>Explorar os Arquivos do Azure

O Armazenamento de Arquivos do Azure fornece uma maneira de criar compartilhamentos de arquivos baseados em nuvem.

1. Na página do portal do Azure para seu contêiner de armazenamento, no lado esquerdo, na seção **Armazenamento de dados**, selecione **Compartilhamentos de arquivos**.
1. Na página Compartilhamentos de arquivos, selecione **&#65291; Compartilhamento de arquivos** e inclua um compartilhamento de arquivos chamado **arquivos** usando a camada **Transação otimizada**.
1. Nos **Compartilhamentos de arquivos**, abra o compartilhamento de novos **arquivos**.
1. Na parte superior da página, selecione **Conectar**. Em seguida, no painel **Conectar**, observe que há guias para sistemas operacionais comuns (Windows, Linux e macOS) que contêm scripts que você pode executar para se conectar à pasta compartilhada a partir de um computador cliente.
1. Feche o painel **Conectar** e, em seguida, feche a página de **arquivos** para retornar à página **Compartilhamentos de arquivos** para sua conta de armazenamento do Azure.

## <a name="explore-azure-tables"></a>Explorar o Tabelas do Azure

O Tabelas do Azure fornece um armazenamento de chave/valor para aplicativos que precisam armazenar valores de dados, mas não precisam da funcionalidade completa e da estrutura de um banco de dados relacional.

1. Na página do portal do Azure para seu contêiner de armazenamento, no lado esquerdo, na seção **Armazenamento de dados**, selecione **Tabelas**.
1. Na página **Tabelas**, selecione **&#65291; Tabela** e crie uma tabela chamada **produtos**.
1. Após a criação da tabela **produtos**, no painel à esquerda, na seção superior, selecione **Navegador de armazenamento**.
1. No gerenciador de armazenamento, selecione **Tabelas** e verifique se a tabela de **produtos** está listada.
1. Selecione a tabela de **produtos**.
1. Na página do **produto**, selecione **&#65291; Adicionar entidade**.
1. No painel **Adicionar entidade**, insira os seguintes valores de chave:
    - **PartitionKey**: 1
    - **RowKey**: 1
1. Selecione **Adicionar propriedade** e crie uma nova propriedade com os seguintes valores:

    |Nome da propriedade | Type | Valor |
    | ------------ | ---- | ----- |
    | Nome | String | Widget |

1. Adicione uma segunda propriedade com os seguintes valores:

    |Nome da propriedade | Type | Valor |
    | ------------ | ---- | ----- |
    | Preço | Double | 2,99 |

1. Selecione **Inserir** para inserir uma linha para a nova entidade na tabela.
1. No navegador de armazenamento, verifique se uma linha foi adicionada à tabela de **produtos** e se uma coluna **Carimbo de data/hora** foi criada para indicar quando a linha foi modificada pela última vez.
1. Adicione outra entidade à tabela de **produtos** com as seguintes propriedades:

    |Nome da propriedade | Type | Valor |
    | ------------ | ---- | ----- |
    | PartitionKey | String | 1 |
    | RowKey | String | 2 |
    | Nome | String | Kniknak |
    | Preço | Double | 1,99 |
    | Descontinuado | Boolean | true |

1. Após inserir a nova entidade, verifique se uma linha contendo o produto descontinuado é mostrada na tabela.

    Você inseriu dados manualmente na tabela usando a interface do navegador de armazenamento. Em um cenário real, os desenvolvedores de aplicativos podem usar a API de Tabela do Armazenamento do Azure para criar aplicativos que leem e gravam valores em tabelas, tornando-a uma solução econômica e escalonável para armazenamento NoSQL.

> **Dica**: se você tiver concluído da exploração do Armazenamento do Azure, exclua o grupo de recursos criado no exercício.
