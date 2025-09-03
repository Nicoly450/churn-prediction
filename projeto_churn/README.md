📊 Predição de Cancelamento de Clientes (Churn Prediction)

Este projeto tem como objetivo analisar e prever o risco de cancelamento de clientes (Churn) em uma base fictícia.
Foi criado um pipeline que combina Machine Learning, SQL e Python (pandas + matplotlib) para gerar insights estratégicos que poderiam ser aplicados em uma empresa real.

📑 Sumário

🚀 Tecnologias Utilizadas

📂 Estrutura do Projeto

🔎 Etapas do Projeto

📊 Visualizações

📈 Insights de Negócio

⚡ Exemplos de Código

🎯 Possíveis Ações

📌 Melhorias Futuras

👩‍💻 Autora

🚀 Tecnologias Utilizadas

Python → Pandas, Matplotlib, Scikit-learn, Joblib

SQL (MySQL) → Filtros e consultas

Jupyter Notebook → Análise exploratória e visualização

Machine Learning → Random Forest Classifier

📂 Estrutura do Projeto
projeto_churn/
│-- dados/                     
│   ├── clientes_ativos.csv        # Base inicial (sem score)
│   ├── clientes_ativos_com_score  # Base após predição
│   ├── clientes_churn.csv         # Dados usados no treino
│
│-- notebooks/
│   ├── churn.treino.ipynb         # Treinamento do modelo
│   ├── clientes.ativos.ipynb      # Aplicação do modelo + análises
│
│-- modelo_churn.pkl               # Modelo treinado salvo
│-- graficos/
│   ├── barras_risco.png
│   ├── hist_probs.png
│   ├── risco_por_satisfacao.png
│
│-- README.md                      # Documentação

🔎 Etapas do Projeto
1️⃣ Treinamento do Modelo

Foi utilizado RandomForestClassifier para prever se o cliente iria cancelar.

Métricas avaliadas no treino/teste (acurácia, recall, precisão).

O modelo foi salvo em modelo_churn.pkl via joblib para reuso.

2️⃣ Predição nos Clientes Ativos

Base: clientes_ativos.csv (contendo clientes atuais, sem status de cancelamento).

Após aplicar o modelo, foram geradas as colunas:

ProbCancelamento → probabilidade prevista pelo modelo.

Previsto → 0 = não cancela, 1 = cancela.

Risco(>=0.40) → regra de negócio: clientes acima de 40% de risco são considerados em risco.

3️⃣ Análises no SQL

Consultas realizadas:

-- Seleciona clientes com risco
SELECT Nome, ProbCancelamento
FROM churn_ativos
WHERE `Risco>=0.40` = 1
ORDER BY ProbCancelamento DESC;

-- Média de satisfação, compras e meses dos clientes em risco
SELECT AVG(Satisfacao), AVG(QtdCompras), AVG(MesesComoCliente)
FROM churn_ativos
WHERE ProbCancelamento >= 0.70;

4️⃣ Visualizações em Python

Foram criados gráficos em matplotlib (dark mode) para facilitar a análise:

Distribuição da probabilidade de cancelamento

Comparação entre clientes em risco e sem risco

Risco médio por nível de satisfação

📊 Visualizações

📌 Distribuição da Probabilidade de Cancelamento
Mostra como as probabilidades estão distribuídas e destaca o threshold de 40%.


📌 Clientes em Risco vs Sem Risco
62% dos clientes estão com risco ≥ 40%.


📌 Risco Médio por Nível de Satisfação
Mesmo clientes que avaliaram satisfação = 5 ainda têm risco médio de ~42%.


📈 Insights de Negócio

🔴 62% dos clientes ativos estão em risco de cancelamento.

📉 Clientes com satisfação baixa (1 e 2) apresentam risco de até 53%.

😮 Mesmo clientes com nota máxima de satisfação (5) têm risco acima de 40%, indicando que outros fatores (tempo de cliente, quantidade de compras, plano contratado) influenciam fortemente.

🛒 As variáveis mais importantes no modelo foram:

QtdCompras

MesesComoCliente

Idade
(superando até mesmo a Satisfação isolada).

⚡ Exemplos de Código
📌 Predição com o Modelo Treinado

import joblib, pandas as pd

# Carrega modelo
modelo = joblib.load("modelo_churn.pkl")

# Carrega clientes ativos
df_ativos = pd.read_csv("clientes_ativos.csv")

# Prepara dados
X_ativos = pd.get_dummies(df_ativos.drop(columns=["ClienteID", "Nome", "Email"]),
                          drop_first=True)

# Predição
probs = modelo.predict_proba(X_ativos)[:,1]
df_ativos["ProbCancelamento"] = probs
df_ativos["Previsto"] = (probs >= 0.5).astype(int)
df_ativos["Risco(>=0.40)"] = (probs >= 0.4).astype(int)
🎯 Possíveis Ações
Criar campanhas de retenção personalizadas para clientes em risco ≥ 40%.

Reforçar ações de engajamento (ex.: descontos, upgrades) para clientes de satisfação baixa.

Revisar os planos (Básico, Premium) para identificar quais geram mais churn.

📌 Melhorias Futuras
Testar outros modelos de Machine Learning (XGBoost, Logistic Regression).

Criar pipeline de atualização automática (ETL) para alimentar os dados em tempo real.

Conectar ao Power BI para relatórios interativos.

Implementar alertas automáticos para clientes em risco via e-mail marketing.

👩‍💻 Autora
Nicoly Cardoso
💼 Foco em Análise de Dados & Machine Learning
📍 SQL | Python | Power BI | ETL

