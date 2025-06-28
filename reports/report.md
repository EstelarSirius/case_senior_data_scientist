# Relatório de Predição de DAU

---

## 1. Dados e Pré-processamento
Obtive acesso a 4 tabelas, sendo elas listadas abaixo, todas com duas chaves.

- **Fontes**: 4 tabelas (usuários, instalações, desinstalações e reviews).
- **Integração**: realizada via `appId` e `date`.

### Tratamentos aplicados
Para todas as tabelas, realizei tratamentos a fim de padronizar os formatos, remover dados problematicos e entender melhor o dado. Abaixo estão todos os tratamentos realizados.

- **Deduplicação**:
  - `daumau`: 153 duplicadas removidas.

- **Valores nulos**:
  - `dauReal`: Valores nulos preenchidos com 0 onde haviam valores presentes em mauReal e os restantes descartados.
  - `category`: preenchida com valores históricos do mesmo `appId`.

- **Limpeza adicional**:
  - Datas inválidas (ex: 2220, 2044, etc.) excluídas.
  - `lang` e `country`: removidas por serem constantes.
  - Valores negativos em `daily_ratings` e `daily_reviews`: removidos pois não pareciam fazer sentido.

---

## 2. Engenharia de Features

- **Variável-alvo**: `dauReal` do dia seguinte.
- **Entrada**: janelas deslizantes de 7 dias anteriores (excluindo o atual).
- **Features temporais**: dias da semana, fins de semana e feriados.

---

## 3. Modelagem
Foi realizada a modelagem das seguintes tecnicas: **Regressão Linear**, **Random Forest**, **XGBoost**, **LightBGM**

O modelo selecionado foi o XGBoost, por uma boa combinação de custo de recurso e performance

---

## 4. Tunning de Hiperparâmetros

Utilizada a técnica de Otimização Bayesiana para encontrar os melhores hiperparâmetros com menos iterações do que métodos tradicionais.

### Parâmetros otimizados
```python
OrderedDict([
    ('colsample_bytree', 0.5),
    ('gamma', 0),
    ('learning_rate', 0.03467909988375867),
    ('max_depth', 2),
    ('n_estimators', 346),
    ('reg_alpha', 0),
    ('reg_lambda', 0),
    ('subsample', 1)
])
```
## **5. Resultados**

O modelo final obteve um MAE de 41.699,57, o que indica uma boa generalização.

Há margens para melhorias, que seriam possiveis com mais tempo, principalmente na etapa de future engineering, para explorar melhor a criação de novas features, combinando novas janelas, e validando mais detalhadamente o comportamento com a nossa variavel resposta.