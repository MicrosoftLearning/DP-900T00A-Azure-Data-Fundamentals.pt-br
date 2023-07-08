---
lab:
  title: Explorar o Banco de Dados do Azure para PostgreSQL
  module: Explore relational data in Azure
---

# Explorar o Banco de Dados do Azure para PostgreSQL

Neste exercício, você provisionará um recurso do Banco de Dados do Azure para PostgreSQL em sua assinatura do Azure.

Este laboratório levará aproximadamente **5** minutos para ser concluído.

## Antes de começar

É necessário ter uma [assinatura do Azure](https://azure.microsoft.com/free) com acesso de nível administrativo.

## Provisionar um recurso do Banco de Dados do Azure para PostgreSQL

Neste exercício, você provisionará um recurso do Banco de Dados do Azure para PostgreSQL.

1. No portal do Azure, selecione **&#65291; Criar recurso** no canto superior esquerdo e pesquise *Banco de Dados do Azure para PostgreSQL*. Então, na página resultante do **Banco de Dados do Azure para PostgreSQL**, escolha **Criar**.

1. Examine as opções de Banco de Dados do Azure para PostgreSQL disponíveis e, em seguida, no bloco **Banco de Dados do Azure para PostgreSQL**, selecione **Servidor flexível (Recomendado)** e, em seguida, **Criar**.

    ![Captura de tela das opções de implantação do Banco de Dados do Azure para PostgreSQL](images/postgresql-options.png)

1. Insira os seguintes valores na página **Criar Banco de Dados SQL**:
    - **Assinatura**: Selecione sua assinatura do Azure.
    - **Grupo de recursos**: crie um grupo de recursos com um nome de sua escolha.
    - **Nome do servidor**: insira um nome exclusivo.
    - **Região**: selecione uma região perto de você.
    - **Versão do PostgreSQL**: não alterar.
    - **Tipo de carga de trabalho**: selecione **Desenvolvimento**.
    - **Computação + armazenamento**: não alterar.
    - **Zona de disponibilidade**: não alterar.
    - **Habilitar alta disponibilidade**: não alterar.
    - **Nome de usuário administrador**: seu nome.
    - **Senha** e **Confirmar senha**: uma senha complexa o suficiente.

1. Selecione **Avançar: Rede**.

1. Em **Regras de firewall**, selecione **&#65291; Adicionar endereço IP do cliente atual**.

1. Selecione **Examinar + Criar** e depois **Criar** para criar o banco de dados PostgreSQL do Azure.

1. Aguarde o fim da implantação. Em seguida, acesse o recurso que foi implantado, que deve ter essa aparência:

    ![Captura de tela do portal do Azure mostrando a página do Banco de Dados do Azure para PostgreSQL.](images/postgresql-portal.png)

1. Examine as opções de gerenciamento do seu recurso do Banco de Dados do Azure para PostgreSQL.

> **Dica**: se você tiver concluído a exploração do Banco de Dados do Azure para PostgreSQL, exclua o grupo de recursos criado neste exercício.
