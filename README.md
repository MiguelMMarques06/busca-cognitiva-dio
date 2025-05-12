# ☕ Mecanismo de Busca Cognitiva para Avaliações de Cafeterias

## 📖 Introdução
Este documento descreve o funcionamento de um **mecanismo de busca cognitiva** construído com serviços do **Microsoft Azure** para processar e analisar avaliações de clientes de uma rede de cafeterias. O sistema utiliza **Azure Blob Storage**, **Azure AI Search** e **Azure AI Services (Text Analytics)** para armazenar documentos .docx, indexar conteúdos e extrair sentimentos (positivo, negativo ou neutro) associados às regiões das filiais. O objetivo é ajudar a rede a entender a satisfação dos clientes por localização, identificando pontos fortes e áreas de melhoria.

## 🎯 Contexto da Aplicação
Uma rede de cafeterias recebe avaliações dos clientes em formulários .docx, contendo comentários sobre atendimento, qualidade do café, ambiente, entre outros. Essas avaliações são enviadas por filiais em diferentes cidades (ex.: São Paulo, Rio de Janeiro). O mecanismo de busca cognitiva automatiza a análise desses dados, permitindo que a rede:
- Identifique rapidamente o sentimento predominante em cada filial.
- Filtre avaliações por região ou palavras-chave (ex.: "atendimento").
- Tome decisões baseadas em dados, como melhorar o treinamento em filiais com feedback negativo.

## 🛠️ Funcionamento do Sistema
O sistema integra três serviços do Azure para processar avaliações de forma escalável e inteligente:

### 1. **Armazenamento com Azure Blob Storage** 📂
- **Função**: Armazena os arquivos .docx com avaliações enviadas pelas cafeterias.
- **Como opera**:
  - Cada filial faz upload dos documentos para um contêiner (ex.: `avaliacoes-clientes`) no Blob Storage.
  - O serviço organiza os arquivos e suporta grandes volumes, ideal para uma rede com várias filiais.
- **Exemplo**: Um arquivo `avaliacao_sp.docx` com o texto "O café em São Paulo estava frio" é salvo no contêiner.

### 2. **Indexação com Azure AI Search** 🔍
- **Função**: Extrai texto dos documentos e cria um índice pesquisável para consultas rápidas.
- **Como opera**:
  - Um **indexer** monitora o contêiner do Blob Storage e processa novos arquivos .docx.
  - Extrai o texto usando habilidades integradas (ex.: OCR para documentos digitalizados).
  - Organiza os dados em um índice com campos como:
    - *Conteúdo*: Texto completo da avaliação.
    - *Região*: Cidade ou filial mencionada (ex.: São Paulo).
    - *Sentimento*: Resultado da análise (positivo, negativo, neutro).
  - Permite consultas como: "Mostrar avaliações negativas da filial Rio".
- **Exemplo**: O texto "O café em São Paulo estava frio" é indexado com região "São Paulo" e sentimento "negativo".

### 3. **Análise de Sentimentos com Azure AI Services (Text Analytics)** 🧠
- **Função**: Analisa o texto para determinar o sentimento e identificar a região mencionada.
- **Como opera**:
  - Durante a indexação, o Text Analytics processa o texto das avaliações.
  - Classifica o sentimento:
    - Positivo: "Atendimento excepcional!"
    - Negativo: "Café estava frio."
    - Neutro: "O ambiente é ok."
  - Extrai entidades, como nomes de cidades (ex.: "São Paulo"), para associar a avaliação à filial.
  - Os resultados são salvos no índice do Azure AI Search.
- **Exemplo**: Para "Atendimento em São Paulo foi ótimo, mas o café estava frio", o sistema identifica um sentimento misto (neutro) e a região "São Paulo".

### 4. **Consulta e Insights** 📊
- **Função**: Permite que a equipe consulte o índice e obtenha relatórios.
- **Como opera**:
  - Através do portal do Azure ou APIs, a rede realiza consultas como:
    - "Quantas avaliações positivas na filial São Paulo?"
    - "Quais filiais têm mais comentários sobre 'atendimento'?"
  - Os resultados podem ser visualizados em ferramentas como Power BI para identificar tendências.
- **Exemplo**: A filial Rio tem 70% de avaliações positivas, enquanto São Paulo tem 30% negativas devido a reclamações sobre a temperatura do café.

## 📝 Exemplo Prático
Uma cafeteria em São Paulo envia o arquivo `avaliacao_sp.docx` com o comentário:  
> "O atendimento na filial Centro foi excepcional, mas o café estava frio."

**Como o sistema processa**:
1. O arquivo é salvo no **Blob Storage**.
2. O **Azure AI Search** extrai o texto e envia ao **Text Analytics**.
3. O **Text Analytics** analisa:
   - Sentimento: Neutro (positivo para "atendimento excepcional", negativo para "café frio").
   - Região: São Paulo, filial Centro.
4. O índice é atualizado com:
   - Conteúdo: "O atendimento na filial Centro foi excepcional, mas o café estava frio."
   - Sentimento: Neutro.
   - Região: São Paulo.
5. A equipe consulta o Azure AI Search e descobre que a filial Centro precisa melhorar a temperatura do café, mas tem um ótimo atendimento.

## 🌟 Benefícios para a Rede de Cafeterias
- **Automatização**: Processa avaliações sem intervenção manual, economizando tempo.
- **Escalabilidade**: Lida com avaliações de múltiplas filiais, mesmo em grande volume.
- **Insights Acionáveis**: Identifica problemas específicos (ex.: café frio em São Paulo) e sucessos (ex.: atendimento elogiado no Rio).
- **Flexibilidade**: Pode ser expandido para analisar outros aspectos, como feedback sobre produtos específicos (ex.: "croissant").

## 🚀 Conclusão
O mecanismo de busca cognitiva no Azure transforma avaliações de clientes em insights valiosos para a rede de cafeterias. Ao integrar **Blob Storage**, **AI Search** e **Text Analytics**, o sistema organiza, analisa e torna pesquisáveis grandes quantidades de dados, ajudando a rede a melhorar a experiência do cliente. Para implementar, configure os serviços no Azure e adapte as consultas às necessidades da rede.

*Interessado em mais detalhes ou exemplos? Consulte a [documentação oficial do Azure](https://learn.microsoft.com/azure/)!*
