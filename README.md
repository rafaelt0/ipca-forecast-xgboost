# Previsão do IPCA com XGBoost

Este repositório apresenta um projeto de previsão do IPCA (Índice de Preços ao Consumidor Amplo) utilizando técnicas de Machine Learning, com foco no algoritmo XGBoost. O objetivo é avaliar a capacidade do modelo em capturar a dinâmica inflacionária brasileira a partir de variáveis macroeconômicas obtidas do FRED (Federal Reserve Economic Data) e de fontes nacionais.

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

---

## Resultados Principais
- RMSE: ~0.23  
- MAE: ~0.16  
- R²: ~0.71  

Os resultados mostram que o modelo consegue explicar aproximadamente 70% da variância do IPCA fora da amostra. O desempenho é satisfatório para capturar ciclos de alta e baixa da inflação, embora apresente limitações em choques inesperados, como crises externas e políticas fiscais abruptas.

---

## Conclusões
- O XGBoost demonstrou boa aderência para previsão do IPCA no curto prazo.  
- Variáveis como hiato do produto, câmbio, commodities e taxa Selic tiveram importância significativa.  
- Recomenda-se para etapas futuras:  
  - Testar modelos híbridos (ARIMAX + XGBoost, LSTM, Prophet).  
  - Expandir a base de dados com indicadores externos (ex.: VIX, juros americanos).  
  - Implementar validação walk-forward mais rigorosa.  

---

## Autor
Rafael Eiki Teruya - Estudante de Economia FEA-USP

---

 
- [ ] Desenvolver dashboard interativo em Power BI ou Streamlit.  

