# Azure-AI-Machine-Learning
DIO.ME / DEVOPS

https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/02-content-safety.html
https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html

# Previsão de Aluguel de Bicicletas com Azure Machine Learning (AutoML)

Este projeto utiliza o recurso de **Aprendizado de Máquina Automatizado (AutoML)** do **Azure Machine Learning Studio** para treinar, avaliar e implantar um modelo preditivo capaz de estimar a quantidade de aluguéis de bicicletas em determinado dia com base em variáveis sazonais e meteorológicas.

---

## Objetivo

Desenvolver e implantar um modelo de **regressão** que prevê o número de aluguéis de bicicletas utilizando o AutoML do Azure Machine Learning.

---

## Etapas Realizadas

### 1. Criação do Espaço de Trabalho
- **Recurso**: Azure Machine Learning  
- **Região**: Leste dos EUA  
- Recursos vinculados criados automaticamente:
  - Conta de Armazenamento (Blob)
  - Cofre de Chaves
  - Application Insights

---

### 2. Upload do Conjunto de Dados
- Nome: `bike-rentals`
- Fonte: [Download dos dados](https://aka.ms/bike-rentals)
- Tipo: Tabela (mltable)

---

### 3. Configuração do Experimento AutoML
- Nome do experimento: `mslearn-bike-rental`
- Tipo de tarefa: `Regressão`
- Coluna alvo: `rentals`
- Modelos utilizados:
  - `RandomForest`
  - `LightGBM`
- Métrica primária: `NormalizedRootMeanSquaredError`
- Validação: Divisão de treino/validação com 10%
- Restrições:
  - Máximo de 3 testes
  - Máximo de 3 ensaios simultâneos 
  - Tempo limite: 15 minutos
  - Stacking e explicação desativados

---

### 4. Treinamento e Avaliação
- Melhor modelo: `RandomForest` (ou `LightGBM`, dependendo dos resultados)
- Análise:
  - **Gráfico de resíduos**: distribuição do erro de predição
  - **Gráfico expected_true**: relação entre valores reais e previstos

---

### 5. Implantação do Modelo
- Tipo: Ponto de extremidade em tempo real
- VM: `Standard_DS3_v2`  
- Instâncias: 3  
- Endpoint: `predict-rentals`

---

### 6. Teste do Modelo Implantado
#### Payload JSON para teste:

{
  "input_data": {
    "columns": [
      "day", "mnth", "year", "season", "holiday",
      "weekday", "workingday", "weathersit", 
      "temp", "atemp", "hum", "windspeed"
    ],
    "index": [0],
    "data": [[1, 1, 2022, 2, 0, 1, 1, 2, 0.3, 0.3, 0.3, 0.3]]
  }
}