# Previsão do IPCA com XGBoost

Este repositório apresenta um projeto de previsão do IPCA (Índice de Preços ao Consumidor Amplo) utilizando técnicas de Machine Learning, com foco no algoritmo XGBoost. O objetivo é avaliar a capacidade do modelo em capturar a dinâmica inflacionária brasileira a partir de variáveis macroeconômicas obtidas do Sistema Gerenciador de Séries Temporais (Banco Central do Brasil) e do FRED (Federal Reserve Economic Data).

---

## Objetivos do Projeto
- Construir uma base de dados macroeconômicos relevante para previsão da inflação.
- Tratar e preparar as séries temporais evitando lookahead bias.
- Criar variáveis derivadas (lags, variações percentuais, interações e volatilidade).
- Treinar modelos XGBoost com ajuste de hiperparâmetros via Optuna.
- Avaliar previsões fora da amostra com métricas estatísticas robustas.
- Analisar os resultados e limitações do modelo.

---


---

## Metodologia
1. **Coleta de Dados**  
   - IPCA, PIB Mensal, IGP-M, IPA-DI, câmbio, Selic, petróleo, soja e outras commodities.  

2. **Pré-processamento**  
   - Criação de defasagens (lags de 1, 2, 3 e 12 meses).  
   - Cálculo de variações mensais em percentual.  
   - Construção de interações macroeconômicas (ex.: Hiato x Juros, Câmbio x Petróleo).  
   - Normalização com StandardScaler.  
   - Divisão treino (80%) e teste (20%).  

3. **Modelagem**  
   - Algoritmo: XGBoost Regressor.  
   - Ajuste de hiperparâmetros com Optuna (otimização bayesiana).  
   - Validação cruzada com TimeSeriesSplit (7 folds).  

4. **Avaliação**  
   - Métricas: RMSE, MAE, MAPE e R².  
   - Gráficos de previsão vs valores reais.  


## Autor
Rafael Eiki Teruya - Estudante de Economia FEA-USP

---

