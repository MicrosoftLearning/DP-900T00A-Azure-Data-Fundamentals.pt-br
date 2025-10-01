---
lab:
  title: Explorar os conceitos básicos da visualização de dados com o Power BI
  module: Explore fundamentals of data visualization
---

# Explorar os conceitos básicos da visualização de dados com o Power BI

Ao concluir este laboratório, você aprenderá a importar e modelar dados de várias fontes no Power BI Desktop, criar relações e hierarquias para uma análise mais profunda e formatar e categorizar dados para aprimorar a visualização. Você criará relatórios interativos usando tabelas, gráficos e mapas e explorará insights de dados por meio de recursos de detalhamento e realce cruzado. Essas habilidades permitirão que você transforme dados brutos em histórias visuais atraentes e business intelligence acionável.

Este laboratório levará aproximadamente **20** minutos para ser concluído.

## Instalar o Power BI Desktop

Se o Microsoft Power BI Desktop ainda não estiver instalado em seu computador Windows, você poderá baixá-lo e instalá-lo gratuitamente.

> _**Dica**: o Power BI Desktop é a ferramenta de criação em que você cria modelos e relatórios localmente antes de compartilhá-los. A instalação dele é gratuita para fins de aprendizado e protótipo._

1. Baixe o instalador do Power BI Desktop de [https://aka.ms/power-bi-desktop](https://aka.ms/power-bi-desktop?azure-portal=true).

1. Quando o arquivo for baixado, abra-o e use o assistente de configuração para instalar o Power BI Desktop em seu computador. Essa instalação poderá levar alguns minutos.

## Importar dados

1. Abra o Power BI Desktop. A interface do aplicativo deve ser semelhante a esta:

    ![Captura de tela mostrando a tela inicial do Power BI Desktop.](images/power-bi-start.png)

    Agora você está pronto para importar os dados para o relatório.

1. Na tela de boas-vindas do Power BI Desktop, selecione **Obter dados de outras fontes** e, na lista de fontes de dados, selecione **Web** e, em seguida, selecione **Conectar**.

    ![Captura de tela mostrando como selecionar a fonte de dados da Web no Power BI.](images/web-source.png)

1. Na caixa de diálogo **Da Web**, digite a URL a seguir e, em seguida, selecione **OK**:

    ```
    https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/power-bi/customers.csv
    ```

    > _**Dica**: usar o conector da Web com arquivos CSV de amostra significa que todos trabalham com os mesmos dados limpos, sem necessidade de arquivos ou credenciais locais._

1. Na caixa de diálogo “Acessar conteúdo da Web”, selecione **Conectar**.

1. Verifique se a URL abre um conjunto de dados que contém os dados do cliente, conforme mostrado abaixo. Em seguida, selecione **Carregar** para carregar os dados no modelo de dados para seu relatório.

    ![Captura de tela mostrando um conjunto de dados de clientes exibido no Power BI.](images/customers.png)

    > _**Dica**: carregar os dados diretamente é mais rápido para este laboratório. Você sempre pode aplicar transformações posteriormente no Power Query, se necessário._

1. Na janela principal do Power BI Desktop, no menu Dados, selecione **Obter dados** e **Web**:

    ![Captura de tela mostrando o menu “Obter dados” no Power BI.](images/get-data.png)

1. Na caixa de diálogo **Da Web**, digite a URL a seguir e, em seguida, selecione **OK**:

    ```
    https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/power-bi/products.csv
    ```

1. Na caixa de diálogo, selecione **Carregar** para carregar os dados do produto deste arquivo no modelo de dados.

1. Repita as três etapas anteriores para importar um terceiro conjunto de dados que contém os dados do pedido da seguinte URL:

    ```
    https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/power-bi/orders.csv
    ```

    > _**Dica**: incluir Clientes, Produtos e Pedidos cria um modelo pequeno e realista. Várias tabelas relacionadas permitem analisar entre entidades (por exemplo, receita por categoria de produto e cidade)._

## Explorar um modelo de dados

As três tabelas de dados que você importou foram carregadas em um modelo de dados, que agora você explorará e refinará.

1. No Power BI Desktop, na borda esquerda, selecione a guia **Modelo** e organize as tabelas no modelo para que seja possível vê-las. É possível ocultar os painéis no lado direito usando os ícones **>>** :

    ![Captura de tela mostrando a guia Modelo no Power BI.](images/model-tab.png)

1. Na tabela **pedidos**, selecione o campo **Receita** e, em seguida, no painel **Propriedades**, defina sua propriedade **Formato** como **Moeda**:

    ![Captura de tela mostrando como definir o formato Receita como Moeda no Power BI.](images/revenue-currency.png)

    Esta etapa garante que os valores de receita sejam exibidos como moeda nas visualizações de relatórios.

    > _**Dica**: formatar as medidas melhora a legibilidade nos visuais e alinha os números com a forma como os usuários corporativos esperam vê-los._

1. Na tabela de produtos, clique com o botão direito do mouse no campo **Categoria** (ou abra o respectivo menu **&vellip;** ) e selecione **Criar hierarquia**. Esta etapa cria uma hierarquia chamada **Hierarquia de categorias**. Pode ser necessário expandir ou rolar para baixo a tabela de **produtos** para vê-la, mas também é possível exibi-la no painel **Campos**:

    ![Captura de tela mostrando como adicionar a hierarquia de categorias no Power BI.](images/category-hierarchy.png)

1. Na tabela de produtos, clique com o botão direito do mouse no campo **ProductName** (ou abra o respectivo menu **&vellip;** ) e selecione **Adicionar à hierarquia** > **Hierarquia de categorias**. Isso adiciona o campo **NomeDoProduto** à hierarquia que você criou anteriormente.

    ![Captura de tela mostrando como adicionar productname à hierarquia de categoria no Power BI.](images/add-category-hierarchy.png)

1. No painel **Campos**, clique com o botão direito do mouse em **Hierarquia de Categoria** (ou abra seu menu **...**) e selecione **Renomear**. Renomeie a hierarquia para **Produto categorizado**.

    ![Captura de tela mostrando como renomear a hierarquia no Power BI.](images/rename-hierarchy.png)

    > _**Dica**: uma hierarquia de categoria→produto permite o detalhamento em visuais, para que os visualizadores possam explorar do resumo aos detalhes._

1. Na extremidade esquerda, selecione a guia **Exibição de tabelas** e, em seguida, no painel **Dados**, selecione a tabela **Clientes**.

1. Selecione o cabeçalho da coluna **Cidade** e defina sua propriedade **Categoria de Dados** como **Cidade**:

    ![Captura de tela mostrando como definir uma categoria de dados no Power BI.](images/data-category.png)

    Esta etapa garante que os valores nesta coluna sejam interpretados como nomes de cidades, o que pode ser útil ao incluir visualizações de mapas.

    > _**Dica**: as categorias de dados ajudam o Power BI a codificar geograficamente os locais corretamente para que os visuais do mapa coloquem pontos nos lugares certos._

## Criar um relatório

Agora você está quase pronto para criar um relatório. Primeiro, você precisará verificar algumas configurações para garantir que todas as visualizações estejam habilitadas.

1. No menu **Arquivo**, selecione **Opções e Configurações**. Em seguida, selecione **Opções** e, na seção **Segurança**, verifique se a opção **Usar elementos visuais de Mapa e Mapa Preenchido** está habilitada e selecione **OK**.

    ![Captura de tela mostrando como definir a propriedade dos visuais Usar mapa e Mapa preenchido no PowerBI.](images/set-options.png)

    Essa configuração garante a possibilidade de incluir visualizações de mapa nos relatórios.

    > _**Dica**: os visuais do mapa estão desativados em alguns ambientes. Habilitá-los garante que o visual Mapa apareça no painel de visualização._

1. Na extremidade esquerda, selecione a guia **Exibição de relatório** e exiba a interface de design de relatório.

    ![Captura de tela mostrando a guia de relatório no Power BI.](images/report-tab.png)

1. Na faixa de opções, acima da superfície de design do relatório, selecione **Caixa de Texto** e adicione uma caixa de texto com o texto **Relatório de Vendas** ao relatório. Formate o texto com negrito e um tamanho de fonte 32.

    ![Captura de tela mostrando como adicionar uma caixa de texto no Power BI.](images/text-box.png)

    > _**Dica**: um título claro ajuda os usuários a entender a finalidade do relatório em um relance._

1. Selecione qualquer área vazia no relatório para desmarcar a caixa de texto. Em seguida, no painel **Dados**, expanda **Produtos** e selecione o campo **Produtos Categorizados**. Esta etapa adiciona uma tabela ao relatório.

    ![Captura de tela mostrando como adicionar uma tabela de produtos categorizados a um relatório no Power BI.](images/categorized-products-table.png)

    > _**Dica**: usar o campo de hierarquia agora permite alternar diretamente para visuais que dão suporte à busca detalhada._

1. Com a tabela ainda selecionada, no painel **Dados**, expanda **Pedidos** e selecione **Receita**. Uma coluna Receita é adicionada à tabela. Pode ser necessário expandir o tamanho da tabela para vê-la.

    A receita é formatada como moeda, como você especificou no modelo. No entanto, você não especificou o número de casas decimais e, portanto, os valores incluem valores fracionários. Isso não será importante para as visualizações criadas, mas você poderá voltar à guia **Modelo** ou **Dados** e alterar as casas decimais, se desejar.

    ![Captura de tela mostrando uma tabela de produtos categorizados com receita em um relatório.](images/revenue-column.png)

1. Com a tabela ainda selecionada, no painel **Visualizações**, selecione a visualização **Gráfico de colunas empilhadas**. A tabela é alterada para um gráfico de colunas mostrando receita por categoria.

    ![Captura de tela mostrando um gráfico de colunas empilhadas de produtos categorizados com receita em um relatório.](images/stacked-column-chart.png)

    > _**Dica**: um gráfico de colunas facilita a comparação de categorias lado a lado._

1. Acima do gráfico de colunas selecionado, selecione o ícone **&#8595;** para ativar o drill-down. Em seguida, no gráfico, selecione qualquer coluna para fazer uma busca detalhada e ver a receita dos produtos individuais nesta categoria. Essa funcionalidade é possível porque você definiu uma hierarquia de categorias e produtos.

    ![Captura de tela mostrando um gráfico de colunas com drill-down aplicado para ver os produtos em uma categoria.](images/drill-down.png)

    > _**Dica**: o detalhamento revela detalhes sob demanda sem sobrecarregar a visualização, perfeito para insights em camadas._

1. Use o ícone **&#x2191;** para fazer drill-up para o nível da categoria. Em seguida, selecione o ícone **(** &#8595; **)** para desativar o recurso de drill-down.

1. Selecione uma área em branco do relatório e, no painel **Dados**, selecione o campo **Quantidade** na tabela **Pedidos** e o campo **Categoria** na tabela **produtos**. Essa etapa resulta em outro gráfico de colunas mostrando a quantidade de vendas por categoria de produto.

1. Com o novo gráfico de colunas selecionado, no painel **Visualizações**, selecione **Gráfico de pizza** e redimensione o gráfico e posicione-o ao lado do gráfico de colunas receita por categoria.

    ![Captura de tela mostrando um gráfico de pizza que exibe a quantidade de vendas por categoria.](images/category-pie-chart.png)

    > _**Dica**: mostrar a quantidade por categoria como uma pizza destaca a contribuição proporcional, complementando o gráfico de colunas de receita._

1. Selecione uma área em branco do relatório e, no painel **Dados**, selecione o campo **Cidade** na tabela **clientes** e, em seguida, selecione o campo **Receita** na tabela **pedidos**. Isso resulta em um mapa que mostra a receita de vendas por cidade. Reorganize e redimensione as visualizações conforme necessário:

    ![Captura de tela mostrando um mapa que exibe a receita por cidade.](images/revenue-map.png)

    > _**Dica**: mapear a receita por cidade adiciona uma visão geográfica: padrões que não são óbvios em tabelas ou gráficos podem aparecer em um mapa._

1. No mapa, observe que você pode arrastar, clicar duas vezes, usar uma roda do mouse ou pinçar e arrastar uma tela sensível ao toque para interagir. Em seguida, selecione uma cidade específica e observe que as outras visualizações no relatório são modificadas para realçar os dados da cidade selecionada.

    ![Captura de tela mostrando um mapa que exibe a receita por cidade, realçando os dados da cidade selecionada.](images/selected-data.png)

    > _**Dica**: o realce cruzado permite que os usuários interajam com um visual e vejam as alterações relacionadas na página, transformando um relatório estático em uma experiência interativa._

1. No menu **Arquivo**, selecione **Salvar**. Em seguida, salve o arquivo com um nome de arquivo. pbix apropriado. Você pode abrir o arquivo e explorar ainda mais a modelagem de dados e a visualização como quiser.

    > _**Dica**: salvar um .pbix mantém seu modelo, consultas e relatório juntos para que você possa reabrir e iterar mais tarde._

Com uma assinatura do [serviço do Power BI](https://www.powerbi.com/?azure-portal=true), é possível entrar em sua conta e publicar o relatório em um workspace do Power BI. 

> **Dica**: a publicação no serviço do Power BI permite que você compartilhe o relatório, agende a atualização e colabore com outras pessoas em seu workspace.
