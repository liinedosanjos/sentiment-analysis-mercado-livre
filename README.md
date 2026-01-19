# Analise de Sentimento de Comentarios – Mercado Livre (PT-BR)

## Visao Geral
Este repositorio contem um MVP de Analise de Sentimento, desenvolvido para o Hackathon One/Alura.
O objetivo do projeto e classificar automaticamente comentarios de clientes em tres categorias:
Positivo, Neutro e Negativo, utilizando apenas o texto do comentario.

O foco do projeto esta na etapa de Data Science, com modelo treinado, avaliado, documentado
e pronto para integracao via API REST.

## Problema de Negocio
Empresas recebem um grande volume de comentarios e avaliacoes e nao conseguem analisa-los
manualmente. A analise automatica de sentimento permite:
- Identificar rapidamente reclamacoes e elogios
- Priorizar atendimentos criticos
- Monitorar a satisfacao dos clientes ao longo do tempo

## Dados Utilizados
Foram utilizados comentarios publicos do Mercado Livre, em portugues, armazenados em um
repositorio separado de dados brutos:
https://github.com/liinedosanjos/ecommerce-product-dataset

Apos a limpeza inicial (remocao de comentarios vazios ou nulos), o dataset final contem
aproximadamente 203 mil comentarios validos.

Os dados incluem:
- Texto do comentario (content)
- Nota do cliente (rating)

Os dados brutos nao sao alterados neste projeto.

## Abordagem de Data Science

### Criacao de rotulos (Proxy)
Como o dataset nao possuia rotulos explicitos de sentimento, a nota do cliente foi utilizada
como proxy apenas durante o treinamento do modelo:
- Rating 4 ou 5: Positivo
- Rating 3: Neutro
- Rating 1 ou 2: Negativo

A nota nao e utilizada na predicao, apenas o texto.

### Pre-processamento de Texto
- Conversao para minusculas
- Remocao de links
- Remocao de caracteres especiais e pontuacao

### Modelagem
- TF-IDF para vetorizacao dos textos
- Regressao Logistica para classificacao multiclasse
- Tratamento de desbalanceamento com class_weight = balanced

### Avaliacao
O modelo foi avaliado utilizando as metricas:
- Acuracia
- Precisao
- Recall
- F1-score

## Estrutura do Repositorio

notebooks/
  sentiment_analysis_mercadolivre.ipynb
model/
  sentiment_analysis_model.pkl
README.md
requirements.txt

## Como Executar o Projeto
1. Clonar o repositorio:
git clone https://github.com/liinedosanjos/sentiment-analysis-mercadolivre.git

2. Instalar as dependencias:
pip install -r requirements.txt

3. Executar o notebook:
jupyter notebook notebooks/sentiment_analysis_mercadolivre.ipynb

O notebook carrega os dados diretamente do repositorio de dados via URL publica.

## Integracao com Back-End
O modelo treinado e salvo no arquivo:
model/sentiment_analysis_model.pkl

Esse arquivo pode ser carregado por uma API para expor um endpoint como:
POST /sentiment
{
  "text": "Produto excelente, entrega rapida"
}

Resposta esperada:
{
  "previsao": "Positivo",
  "probabilidade": 0.92
}

## Observacoes Importantes
- O modelo utiliza apenas o texto para realizar previsoes
- O uso de rating como proxy e adequado para MVPs e cenarios reais de e-commerce
- O back-end nao foi implementado neste repositorio, mas o contrato de integracao esta definido

## Contexto
Projeto desenvolvido para fins educacionais no Hackathon One/Alura, com foco em alunos de
Data Science e Back-End.

## Licenca
Uso educacional.
