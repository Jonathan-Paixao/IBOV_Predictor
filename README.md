# 📈 IBOV Predictor: Inteligência Artificial para o Mercado Financeiro

![Status do Projeto](https://img.shields.io/badge/Status-Finalizado-success)
![Acurácia](https://img.shields.io/badge/Acurácia_Teste-77%25-brightgreen)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)

> **Desenvolvido por [DeleteTableSemWhere](https://github.com/DeleteTableSemWhere)**

## Sobre o Projeto

O **IBOV Predictor** é um pipeline avançado de Machine Learning desenvolvido para prever a direção (Alta ou Baixa) do índice **IBOVESPA** (^BVSP) no pregão seguinte.

Diferente de modelos teóricos simples, este projeto foca na **realidade de mercado**: utiliza dados dinâmicos via API, validação temporal estrita e blindagem contra *Data Leakage*, garantindo que o modelo seja funcional e honesto.

---
## 🚀 Principais Destaques

* **Automação Total (ETL):** Abandono de arquivos estáticos (`.xlsx`). O sistema baixa, limpa e padroniza dados diretamente do **Yahoo Finance** a cada execução.
* **Engenharia de Features Robusta:** Criação automática de indicadores técnicos (Médias Móveis, RSI, IFR) e variáveis de atraso (Lags) para capturar o "momentum" do mercado.
* **Prevenção de Data Leakage:** Implementação rigorosa de divisão temporal (Treino no passado, Teste no futuro) e remoção cirúrgica de variáveis "futuras" (como Máxima/Mínima do dia alvo).
* **Benchmark de Modelos:** Comparação automatizada entre 5 algoritmos: **Logistic Regression, Random Forest, SVM, XGBoost e Voting Classifier**.

Com a implementação do algoritmo **XGBoost** e uma engenharia de atributos refinada, atingimos resultados expressivos na janela de teste mais recente (Dez/25 - Jan/26):

| Métrica | Resultado | Descrição |
| :--- | :--- | :--- |
| **Acurácia (Teste)** | **77%** | Taxa de acerto direcional em dados inéditos. |
| **Melhor modelo** | **XGBoost** | Superou Random Forest, SVM e Regressão Logística. |
| **Consistência** | Alta | Baixo overfitting (diferença saudável entre Treino e Teste). |

---
## Engenharia de Dados e Modelagem

O projeto segue um pipeline linear e auditável:

### 1. Coleta de Dados (`Data Ingestion`)
* **Fonte:** API `yfinance`.
* **Tratamento:** Achatamento de cabeçalhos (MultiIndex), conversão de tipos (`float64`, `datetime`) e tradução de colunas para PT-BR.
* **Período:** Dados históricos de 2016 até o dia atual (D-0).

### 2. Engenharia de Features
Transformamos preços brutos em indicadores preditivos, com **blindagem total contra vazamento de dados** (uso de `.shift(1)` em todas as variáveis técnicas):
* **Momentum:** RSI (Índice de Força Relativa), MACD, ROC.
* **Tendência:** Médias Móveis (5, 10, 20, 50 períodos) e cruzamentos.
* **Volatilidade:** Bandas de Bollinger, ATR (Average True Range).
* **Padrões de Candle:** Gap de Abertura e Range do dia anterior.

### 3. Validação Temporal (Backtesting)
Não usamos `train_test_split` aleatório (shuffle), pois isso destruiria a ordem cronológica do mercado.
* Utilização de `TimeSeriesSplit` para validação cruzada.
* Separação dos últimos 30 dias exclusivamente para teste final ("Prova Real").

### 4. Modelagem e Avaliação
Treinamento de múltiplos modelos e seleção baseada em métricas de negócio:
* **Acurácia:** Taxa global de acerto.
* **Precision:** Capacidade de não gerar falsos positivos (crucial para evitar prejuízo financeiro).
* **Matriz de Confusão:** Análise visual dos erros.

### 5. Modelos Avaliados
Uma bateria de algoritmos compete para definir a melhor predição:
* **XGBoost (Melhor modelo para os últimos 30 dias da avaliação)**
* Random Forest
* Support Vector Machine (SVM)
* Logistic Regression
* *Voting Classifier (Comitê de Modelos)*

## Executando o Projeto

### Pré-requisitos
* Python 3.0+
* Pip

### Instalação
1.  Clone o repositório:
    ```bash
    git clone [https://github.com/DeleteTableSemWhere/IBOV_Predictor.git]
    ```
2.  Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```

### Execução
Basta abrir o Jupyter Notebook e rodar todas as células. O pipeline irá:
1.  Baixar os dados mais recentes.
2.  Treinar os modelos.
3.  Exibir os gráficos de performance e a decisão final do dia.

---
## 📊 Resultados Alcançados

O modelo final selecionado (**XGBoost**) superou o benchmark (75%) e a média de mercado (62%).

![Gráfico de Predições no Tempo](predicoes_tempo.png)

* **Acurácia em Teste:** 77,8% (Consistente com dados reais e limpos).

![Gráfico Matriz de confusão](feature_importance.png)
* **Consistência:** O modelo demonstrou robustez ao migrar de dados estáticos para dados dinâmicos da API, mantendo a performance sem overfitting agressivo.

---

## Autores

Desenvolvido como parte do Tech Challenge (Fase 2) da Pós-Tech Data Analytics (FIAP).

<div align="center">
<table>
  <tr>
    <td align="center">
      <a href="https://github.com/BrunoAssis12">
        <img src="https://github.com/BrunoAssis12.png" width="100px;" alt=""/>
        <br /><sub><b>Bruno Assis</b></sub>
      </a><br />
      🚀 Data Scientist
    </td>
    <td align="center">
      <a href="https://github.com/gnunes-io">
        <img src="https://github.com/gnunes-io.png" width="100px;" alt=""/>
        <br /><sub><b>Gabriel Nunes</b></sub>
      </a><br />
      📊 Data Analyst
    </td>
    <td align="center">
      <a href="https://github.com/Jonathan-Paixao">
        <img src="https://github.com/Jonathan-Paixao.png" width="100px;" alt=""/>
        <br /><sub><b>Jonathan Paixão</b></sub>
      </a><br />
      🐍 Python Dev
    </td>
    <td align="center">
      <a href="https://github.com/rafaelvieiravidal-glitch">
        <img src="https://github.com/rafaelvieiravidal-glitch.png" width="100px;" alt=""/>
        <br /><sub><b>Rafael Vieira</b></sub>
      </a><br />
      📉 Quant
    </td>
    <td align="center">
      <a href="#">
        <img src="https://github.com/ghost.png" width="100px;" alt=""/>
        <br /><sub><b>Wagner da Silva</b></sub>
      </a><br />
      🧠 AI Engineer
    </td>
  </tr>
</table>
</div>

<br>

## 🛡️ Aviso Legal
Este projeto tem fins estritamente educacionais e acadêmicos. As previsões geradas por algoritmos de Machine Learning possuem margem de erro e **não constituem recomendação de investimento**. O mercado financeiro é volátil e envolve riscos.