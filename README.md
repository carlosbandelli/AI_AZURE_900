# AI_AZURE_900
## Explorar um índice de pesquisa do Azure AI (UI)

Imagine que você trabalha para a Fourth Coffee, uma cadeia nacional de café. Você foi solicitado a ajudar a construir uma solução de mineração de conhecimento que facilite a busca por insights sobre experiências de clientes. Você decide construir um índice de pesquisa do Azure AI usando dados extraídos de avaliações de clientes.

Neste laboratório você irá:

- Criar recursos do Azure
- Extrair dados de uma fonte de dados
- Enriquecer dados com habilidades de IA
- Usar o indexador do Azure no portal do Azure
- Consultar seu índice de pesquisa
- Revisar resultados salvos em um Knowledge Store

Recursos do Azure necessários
A solução que você criará para a Fourth Coffee requer os seguintes recursos em sua assinatura do Azure:

- Um recurso de Pesquisa do Azure AI, que gerenciará a indexação e a consulta.
- Um recurso de serviços de IA do Azure, que fornece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.

Observação: seus recursos de Pesquisa do Azure AI e serviços de IA do Azure devem estar na mesma localização!

- Uma conta de Armazenamento com contêineres de blob, que armazenará documentos brutos e outras coleções de tabelas, objetos ou arquivos.

Criar um recurso de Pesquisa do Azure AI
Faça login no portal do Azure.

Clique no botão + Criar um recurso, procure por Pesquisa do Azure AI e crie um recurso de Pesquisa do Azure AI com as seguintes configurações:

- Assinatura: Sua assinatura do Azure.
- Grupo de recursos: Selecione ou crie um grupo de recursos com um nome único.
- Nome do serviço: Um nome único.
- Localização: Escolha qualquer região disponível.
- Nível de preços: Básico
Selecione Revisar + criar e, após ver a resposta Sucesso na validação, selecione Criar.

Após a conclusão da implantação, selecione Ir para o recurso. Na página de visão geral da Pesquisa do Azure AI, você pode adicionar índices, importar dados e pesquisar índices criados.

## Criar um recurso de serviços de IA do Azure

Você precisará provisionar um recurso de serviços de IA do Azure que esteja na mesma localização que seu recurso de Pesquisa do Azure AI. Sua solução de pesquisa usará este recurso para enriquecer os dados no datastore com insights gerados por IA.

Volte para a página inicial do portal do Azure. Clique no botão + Criar um recurso e pesquise por Serviços de IA do Azure. Selecione criar um plano de serviços de IA do Azure. Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:
- Assinatura: Sua assinatura do Azure.
- Grupo de recursos: O mesmo grupo de recursos que seu recurso de Pesquisa do Azure AI.
- Local: A mesma localização que seu recurso de Pesquisa do Azure AI.
- Nome: Um nome único.
- Nível de preços: Standard S0
Ao marcar esta caixa, reconheço que li e compreendi todos os termos abaixo: Selecionado
Selecione Revisão + criar. Depois de ver a resposta Validação Aprovada, selecione Criar.

Aguarde o término da implantação e, em seguida, visualize os detalhes da implantação.

## Criar uma conta de armazenamento

Volte para a página inicial do portal do Azure e selecione o botão + Criar um recurso.

Pesquise por conta de armazenamento e crie um recurso de conta de armazenamento com as seguintes configurações:
- Assinatura: Sua assinatura do Azure.
- Grupo de recursos: O mesmo grupo de recursos que seus recursos de Pesquisa do Azure AI e Serviços de IA do Azure.
- Nome da conta de armazenamento: Um nome único.
- Localização: Escolha qualquer local disponível.
- Desempenho: Padrão
- Redundância: Armazenamento redundante localmente (LRS)
Clique em Revisar e depois clique em Criar. Aguarde o término da implantação e depois vá para o recurso implantado.

Na conta de armazenamento do Azure que você criou, no painel de menu à esquerda, selecione Configuração (em Configurações).
Altere a configuração para Permitir acesso anônimo de blob para Habilitado e depois selecione Salvar.

## Carregar Documentos no Armazenamento do Azure

No painel de menu à esquerda, selecione Contêineres.

![Screenshot que mostra a página de visão geral do blob de armazenamento.](link_da_imagem_aqui)

Selecione + Contêiner. Uma janela à sua direita será aberta.

Digite as seguintes configurações e clique em Criar:
- Nome: coffee-reviews
- Nível de acesso público: Contêiner (acesso de leitura anônimo para contêineres e blobs)
- Avançado: sem alterações.

Em uma nova aba do navegador, baixe os reviews de café compactados de https://aka.ms/mslearn-coffee-reviews e extraia os arquivos para a pasta reviews.

No portal do Azure, selecione seu contêiner coffee-reviews. No contêiner, selecione Upload.

![Screenshot que mostra o contêiner de armazenamento.](link_da_imagem_aqui)

Na janela de upload de blob, selecione Selecionar um arquivo.

Na janela do Explorer, selecione todos os arquivos na pasta reviews, selecione Abrir e depois selecione Upload.

![Screenshot que mostra os arquivos enviados para o contêiner do Azure.](link_da_imagem_aqui)

Após o término do upload, você pode fechar a janela de upload de blob. Seus documentos agora estão no seu contêiner de armazenamento coffee-reviews.

## Indexar os Documentos

Após ter os documentos armazenados, você pode usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um Assistente de Importação de Dados. Com este assistente, você pode criar automaticamente um índice e um indexador para fontes de dados suportadas. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice do Azure AI Search.

No portal do Azure, acesse o seu recurso Azure AI Search. Na página de Visão geral, selecione Importar dados.

![Captura de tela que mostra o assistente de importação de dados.](link_da_imagem_aqui)

Na página Conectar-se aos seus dados, na lista Fonte de dados, selecione Armazenamento de Blob do Azure. Preencha os detalhes do armazenamento de dados com os seguintes valores:
- Fonte de dados: Armazenamento de Blob do Azure
- Nome da fonte de dados: coffee-customer-data
- Dados a serem extraídos: Conteúdo e metadados
- Modo de análise: Padrão
- String de conexão: *Selecione Escolher uma conexão existente. Selecione sua conta de armazenamento, selecione o contêiner coffee-reviews e clique em Selecionar.
- Autenticação de identidade gerenciada: Nenhum
- Nome do contêiner: esta configuração é preenchida automaticamente após você escolher uma conexão existente.
- Pasta de Blob: Deixe em branco.
- Descrição: Avaliações para lojas de café Fourth Coffee.
- Selecione Avançar: Adicionar habilidades cognitivas (Opcional).

Na seção Anexar Serviços Cognitivos, selecione seu recurso Azure AI services.

Na seção Adicionar enriquecimentos:
- Altere o nome do Conjunto de habilidades para coffee-skillset.
- Selecione a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content.
  Observação: É importante selecionar Habilitar OCR para ver todas as opções de campos enriquecidos.
- Garanta que o campo Dados de origem esteja definido como merged_content.
- Altere o nível de granularidade de enriquecimento para Páginas (partes de 5000 caracteres).
- Não selecione Habilitar enriquecimento incremental.
- Selecione os seguintes campos enriquecidos:

  Habilidade Cognitiva	     Parâmetro	   Nome do Campo
  Extrair nomes de local	  	          localizações
  Extrair frases-chave	  	          fraseschave
  Detectar sentimento	  	          sentimento
  Gerar tags de imagens	  	          tagsDeImagens
  Gerar legendas de imagens	  	  legendaDeImagens

Sob Salvar enriquecimentos em um conhecimento armazenar, selecione:
- Projeções de imagem
- Documentos
- Páginas
- Frases-chave
- Entidades
- Detalhes da imagem
- Referências de imagem
## Selecionar Projeções de Blob Azure: Documento
Uma configuração para Nome do contêiner com o contêiner de armazenamento de conhecimento auto-populado é exibida. Não altere o nome do contêiner.

Selecione Avançar: Personalizar índice de destino. Altere o Nome do índice para coffee-index.

Garanta que a Chave esteja definida como metadata_storage_path. Deixe o nome do Sugestor em branco e o modo de pesquisa será preenchido automaticamente.

Revise as configurações padrão dos campos do índice. Selecione filtrável para todos os campos que já estão selecionados por padrão.

![Captura de tela que mostra o painel de personalização do índice com o nome do índice inserido e 'Filtrável' selecionado para um campo de índice padrão.](link_da_imagem_aqui)

Selecione Avançar: Criar um indexador.

Altere o Nome do indexador para coffee-indexer.

Deixe o Agendamento definido como Uma vez.

Expanda as opções Avançadas. Garanta que a opção Base-64 Encode Keys esteja selecionada, pois a codificação de chaves pode tornar o índice mais eficiente.

Selecione Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
  - Extrai os campos de metadados e conteúdo do documento da fonte de dados.
  - Executa o conjunto de habilidades de habilidades cognitivas para gerar mais campos enriquecidos.
  - Mapeia os campos extraídos para o índice.

Retorne à página do recurso Azure AI Search. No painel esquerdo, em Gerenciamento de Pesquisa, selecione Indexadores. Selecione o indexador coffee-indexer recém-criado. Aguarde um minuto e selecione &orarr; Atualizar até que o Status indique sucesso.

Selecione o nome do indexador para ver mais detalhes.

![Captura de tela que mostra o indexador coffee-indexer criado com sucesso.](link_da_imagem_aqui)

### Consultar o índice

Use o Explorador de Pesquisa para escrever e testar consultas. O Explorador de Pesquisa é uma ferramenta integrada ao portal do Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Explorador de Pesquisa para escrever consultas e revisar resultados em JSON.

Na página Visão geral do seu serviço de Pesquisa, selecione Explorador de Pesquisa no topo da tela.

![Captura de tela de como encontrar o Explorador de Pesquisa.](link_da_imagem_aqui)

Observe como o índice selecionado é o coffee-index que você criou. Abaixo do índice selecionado, altere a visualização para visualização JSON.

![Captura de tela do Explorador de Pesquisa.](link_da_imagem_aqui)

No campo do editor de consulta JSON, copie e cole:

```json
{
    "search": "*",
    "count": true
}
```
Selecione Pesquisar. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.

Agora vamos filtrar por localização. No campo do editor de consulta JSON, copie e cole:
```json
{
 "search": "locations:'Chicago'",
 "count": true
}

```
Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra as avaliações com uma localização em Chicago. Você deve ver 3 no campo @odata.count.

Agora vamos filtrar por sentimento. No campo do editor de consulta JSON, copie e cole:

```json
{
 "search": "sentiment:'negative'",
 "count": true
}

```
Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra as avaliações com um sentimento negativo. Você deve ver 1 no campo @odata.count.

Observe como os resultados são classificados por @search.score. Esta é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão bem os resultados correspondem à consulta fornecida.

Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa. O que você acha que pode ser a causa da avaliação?
## Revisar o armazenamento de conhecimento

Vamos ver o poder do armazenamento de conhecimento em ação. Quando você executou o assistente de Importação de dados, também criou um armazenamento de conhecimento. Dentro do armazenamento de conhecimento, você encontrará os dados enriquecidos extraídos pelas habilidades de IA persistindo na forma de projeções e tabelas.

No portal do Azure, navegue de volta para sua conta de armazenamento do Azure.

No painel de menu à esquerda, selecione Contêineres. Selecione o contêiner knowledge-store.

![Captura de tela do contêiner knowledge-store.](link_da_imagem_aqui)

Selecione um dos itens e clique no arquivo objectprojection.json.

![Captura de tela do objectprojection.json.](link_da_imagem_aqui)

Selecione Editar para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

![Captura de tela de como encontrar o botão de edição.](link_da_imagem_aqui)

Selecione o caminho do blob de armazenamento no canto superior esquerdo da tela para retornar aos Contêineres da conta de armazenamento.

![Captura de tela do caminho do blob de armazenamento.](link_da_imagem_aqui)

Nos Contêineres, selecione o contêiner coffee-skillset-image-projection. Selecione qualquer um dos itens.

![Captura de tela do contêiner de skillset.](link_da_imagem_aqui)

Selecione qualquer um dos arquivos .jpg. Selecione Editar para ver a imagem armazenada do documento. Observe como todas as imagens dos documentos são armazenadas dessa maneira.

![Captura de tela da imagem salva.](link_da_imagem_aqui)

Selecione o caminho do blob de armazenamento no canto superior esquerdo da tela para retornar aos Contêineres da conta de armazenamento.

Selecione Navegador de armazenamento no painel à esquerda e selecione Tabelas. Há uma tabela para cada entidade no índice. Selecione a tabela coffeeSkillsetKeyPhrases.

Observe as frases-chave que o armazenamento de conhecimento foi capaz de capturar a partir do conteúdo das avaliações. Muitos dos campos são chaves, então você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.
