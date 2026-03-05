# 💳 Previsão de Inadimplência de Cartão de Crédito (Credit Card Default Prediction)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/scikit--learn-Machine%20Learning-orange?style=flat-square&logo=scikit-learn)](https://scikit-learn.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=flat-square&logo=pandas)](https://pandas.pydata.org/)

**Autor:** Francisco Bruno Lopes Grangeiro

## 📌 Sobre o Projeto
Este projeto de Machine Learning supervisionado tem como objetivo prever a probabilidade de um cliente entrar em **inadimplência (default)** no mês seguinte, utilizando um histórico de pagamentos e dados demográficos de clientes de cartão de crédito em Taiwan.

O foco deste repositório não é apenas alcançar a maior métrica numérica possível, mas sim **traduzir a performance dos algoritmos para o impacto financeiro real no negócio**, lidando com o desafio de dados desbalanceados e os custos assimétricos de aprovação de crédito.

## 🎯 O Problema de Negócio: Por que a Acurácia mente?
No mercado de crédito, os erros do modelo possuem pesos financeiros muito diferentes:
* **Falso Positivo (Bloquear um bom pagador):** O banco perde a oportunidade de lucrar com as taxas do cartão. (Custo baixo).
* **Falso Negativo (Aprovar um caloteiro):** O banco perde todo o montante emprestado, o que gera um rombo no caixa. (Custo altíssimo).

Como a base de dados possui muito mais bons pagadores do que maus pagadores, um modelo pode facilmente atingir mais de 80% de **Acurácia** apenas prevendo que "ninguém dará calote". Por isso, a Acurácia é tratada como uma métrica de vaidade neste projeto. O foco da nossa otimização e avaliação técnica é o **Recall (Revocação)** e a **Curva ROC / AUC**.

## 📊 Base de Dados
* **Volume:** ~30.000 registros.
* **Features utilizadas:** 23 variáveis, incluindo dados demográficos (gênero, escolaridade, estado civil) e histórico de faturas e pagamentos ao longo de 6 meses.
* **Variável-Alvo (Target):** `default payment next month` (1 = Inadimplente, 0 = Bom pagador).

## 🧠 Modelos Desenvolvidos
Todo o projeto foi construído utilizando apenas a biblioteca `scikit-learn`, passando por uma evolução de complexidade:

1. **Perceptron (Baseline Linear):** Utilizado para testar a hipótese de separabilidade linear. Demonstrou *underfitting*, provando que o comportamento de crédito é altamente não-linear.
2. **Árvore de Decisão (Decision Tree):** Introduzida para captar condicionais complexas. Passou por otimização via `GridSearchCV` para evitar *overfitting*. Proporcionou alta explicabilidade de negócio (ex: o atraso no último mês é o fator mais crítico).
3. **Random Forest (Ensemble):** Modelo final otimizado. Reduziu a variância da árvore isolada através de votação majoritária (*Bagging*), obtendo a melhor estabilidade (F1-Score) e a maior Área Sob a Curva (AUC).

## 🚀 Principais Resultados e Recomendações
* **Feature Importance:** Comprovou-se que as variáveis demográficas (idade, gênero) têm pouca relevância frente ao **comportamento recente** de pagamentos (A variável `PAY_0`, referente ao atraso no último mês, dominou quase 30% da decisão da Random Forest).
* **Threshold Tuning (Ajuste de Limiar):** Devido à alta taxa de Falsos Negativos no limiar padrão (50%), a recomendação gerencial final é reduzir o ponto de corte preditivo. Rejeitar crédito a partir de 25% ou 30% de probabilidade de default aumenta significativamente o *Recall*, protegendo o caixa da instituição em detrimento de uma leve queda na *Precision*.

## ⚙️ Como executar
1. Clone este repositório: `git clone https://github.com/fbgrangeiro98/Projeto_machine_learning_analise_credito.git`
2. Instale as dependências necessárias:
   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```
3. Abra o arquivo Jupyter Notebook e execute as células sequencialmente:
   ```bash
   jupyter notebook projeto_inadimplencia_credito.ipynb
   ```

---
*Projeto desenvolvido para a disciplina de Machine Learning / Inteligência Artificial.*
