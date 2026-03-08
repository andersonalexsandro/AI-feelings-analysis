# Relatorio - Etapa 1 (Classificacao) e Etapa 2 (Regressao)

## Base de dados escolhida
Foi utilizada a base `datasets/dataset_avaliacoes.csv`, pois ela atende os dois objetivos do trabalho:

- **Classificacao**: possui a coluna `sentimento` (rotulos `positivo` e `negativo`).
- **Regressao**: possui a coluna `nota` (escala numerica de 1 a 5), que pode ser tratada como variavel-alvo quantitativa.

A base tem 15.501 registros de avaliacoes textuais, tamanho suficiente para comparar modelos de ML com boa estabilidade.

## Etapa 1 - Classificacao

### Modelos treinados
- Logistic Regression
- Linear SVM
- Multinomial Naive Bayes

### Metodos de separacao avaliados
- Divisao simples (80/20)
- K-Fold Cross-Validation (5 folds)
- Stratified K-Fold (5 folds)

### Resultados (Acuracia, Precisao, Recall, F1)

| Metodo | Modelo | Acuracia | Precisao | Recall | F1 |
|---|---:|---:|---:|---:|---:|
| Divisao Simples (80/20) | Linear SVM | 0.9403 | 0.9523 | 0.9286 | **0.9403** |
| Divisao Simples (80/20) | Logistic Regression | 0.9349 | 0.9535 | 0.9158 | 0.9343 |
| Divisao Simples (80/20) | Multinomial NB | 0.9310 | 0.9495 | 0.9120 | 0.9304 |
| K-Fold (5 folds) | Linear SVM | 0.9393 | 0.9531 | 0.9263 | 0.9395 |
| K-Fold (5 folds) | Logistic Regression | 0.9331 | 0.9535 | 0.9131 | 0.9328 |
| K-Fold (5 folds) | Multinomial NB | 0.9268 | 0.9474 | 0.9065 | 0.9265 |
| Stratified K-Fold (5 folds) | Linear SVM | 0.9378 | 0.9505 | 0.9261 | 0.9381 |
| Stratified K-Fold (5 folds) | Logistic Regression | 0.9325 | 0.9540 | 0.9113 | 0.9321 |
| Stratified K-Fold (5 folds) | Multinomial NB | 0.9267 | 0.9466 | 0.9071 | 0.9264 |

### Analise dos metodos de separacao
- **Divisao simples**: rapida e facil, mas depende de uma unica particao (maior risco de variancia na avaliacao).
- **K-Fold**: mais robusta por usar multiplas particoes, reduz efeito de sorte da divisao.
- **Stratified K-Fold**: preserva proporcao de classes em cada fold, sendo geralmente a opcao mais segura para classificacao.

### Melhor modelo de classificacao
- **Linear SVM** foi o melhor em todas as estrategias, com melhor F1 e acuracia.
- Isso indica boa capacidade de separacao em espacos textuais de alta dimensionalidade.

### Interpretacao das metricas
- **Precisao**: entre os positivos previstos, quantos realmente sao positivos.
- **Recall**: entre os positivos reais, quantos o modelo conseguiu encontrar.
- **F1-Score**: equilibrio entre precisao e recall.
- **Acuracia**: proporcao total de acertos.

Quando priorizar:
- **Precisao maior**: quando falso positivo custa caro (ex.: acusar erro quando nao existe).
- **Recall maior**: quando falso negativo custa caro (ex.: deixar de detectar reclamacoes criticas).

## Etapa 2 - Regressao

### Variavel alvo
- `nota` (1 a 5), tratada como valor numerico para prever o nivel de avaliacao.

### Modelos treinados
- Linear Regression
- Ridge
- Linear SVR

### Metodos de separacao avaliados
- Divisao simples (80/20)
- K-Fold (5 folds)
- Stratified K-Fold (5 folds)

Observacao: para aplicar **Stratified K-Fold** em regressao, a `nota` foi estratificada por faixas (bins), mantendo distribuicoes similares entre folds.

### Resultados (R2)

| Metodo | Modelo | R2 |
|---|---|---:|
| Divisao Simples (80/20) | Ridge | 0.7310 |
| Divisao Simples (80/20) | Linear SVR | 0.7195 |
| Divisao Simples (80/20) | Linear Regression | -0.7997 |
| K-Fold (5 folds) | Ridge | 0.7310 |
| K-Fold (5 folds) | Linear SVR | 0.7211 |
| K-Fold (5 folds) | Linear Regression | -0.7051 |
| Stratified K-Fold (5 folds) | Ridge | **0.7325** |
| Stratified K-Fold (5 folds) | Linear SVR | 0.7232 |
| Stratified K-Fold (5 folds) | Linear Regression | -0.2176 |

### Melhor modelo de regressao
- **Ridge** teve o maior R2 (0.7325), com desempenho estavel entre os metodos.
- **Linear SVR** ficou em segundo lugar, proximo do Ridge.
- **Linear Regression** teve desempenho ruim (R2 negativo em varios cenarios), sugerindo falta de regularizacao para esse espaco de texto.

### O que significa R2 neste contexto
- **R2 = 0.7325** indica que cerca de **73.25% da variancia** da `nota` foi explicada pelo modelo.
- Quanto mais proximo de 1, melhor a explicacao da variacao da nota.
- R2 negativo indica modelo pior que uma baseline simples (predizer a media).

## Arquivos gerados
- `notebook/outputs/resultados_classificacao.csv`
- `notebook/outputs/resultados_regressao.csv`
- `notebook/outputs/comparacao_classificacao_f1.png`
- `notebook/outputs/comparacao_regressao_r2.png`
