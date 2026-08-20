# UC8 — Previsão de readmissão hospitalar em pacientes com diabetes

Projeto de machine learning aplicado à previsão de **readmissão hospitalar em menos de 30 dias** em pacientes com diabetes, com foco em **interpretabilidade, análise de enviesamento e enquadramento ético-regulamentar**.

O trabalho está organizado em quatro módulos, cada um num notebook próprio.

---

## Objetivo

Estudar a possibilidade de prever a readmissão hospitalar precoce (< 30 dias) a partir de variáveis demográficas, clínicas e administrativas. A readmissão precoce pode indicar maior risco clínico, necessidade de melhor acompanhamento após a alta e impacto acrescido nos recursos hospitalares.

---

## Dados

| Item | Valor |
|---|---|
| Ficheiro | `data/diabetic_data.csv` |
| Observações | 101 766 |
| Variáveis originais | 50 |
| Variáveis após limpeza | 48 |
| Variáveis explicativas finais | 44 |

**Variável-alvo original** — `readmitted`, com três categorias:

| Categoria | N |
|---|---|
| `NO` (não readmitido) | 54 864 |
| `>30` (readmitido após 30 dias) | 35 545 |
| `<30` (readmitido em menos de 30 dias) | 11 357 |

**Variável-alvo binária** — `readmitted_30`, em que a classe positiva corresponde a readmissão em menos de 30 dias: **11 357 casos (11,16%)** contra 90 409 casos negativos (88,84%). O conjunto é fortemente desequilibrado.

### Preparação

- Valores `?` convertidos em valores ausentes reais.
- Removidas as variáveis com ausência elevada: `weight`, `payer_code`, `medical_specialty`.
- Ausências preenchidas com a categoria `Unknown` em: `max_glu_serum`, `A1Cresult`, `race`, `diag_1`, `diag_2`, `diag_3`.
- Excluídos os identificadores `encounter_id` e `patient_nbr` das variáveis explicativas.
- Divisão estratificada 80/20: **81 412** observações de treino e **20 354** de teste, mantendo ~11,16% de classe positiva em ambos.
- Os valores extremos identificados por intervalo interquartil (`number_outpatient`, `number_emergency`, `number_inpatient`, `num_procedures`) **não** foram removidos, por poderem representar situações clínicas reais.

A variável numérica com maior correlação com `readmitted_30` é `number_inpatient` (≈ 0,165). As correlações são globalmente baixas — a readmissão precoce não é explicada por nenhuma variável isolada.

---

## Módulos

### Módulo 1 — Análise exploratória e preparação dos dados
`notebooks/01_modulo1_eda_preparacao.ipynb`

Contextualização do conjunto de dados, análise da variável-alvo, criação da variável binária, tratamento de valores em falta, análise univariada e bivariada, matriz de correlação, análise de anomalias e divisão treino/teste.

### Módulo 2 — Modelação e avaliação de desempenho
`notebooks/02_modulo2_modelacao_avaliacao.ipynb`

Pipeline de pré-processamento e treino de três modelos — dois interpretáveis (Regressão Logística, Árvore de Decisão) e um menos interpretável (Random Forest) — seguidos de comparação e afinação de hiperparâmetros.

### Módulo 3 — Interpretabilidade e explicações
`notebooks/03_modulo3_interpretabilidade.ipynb`

- Interpretação direta dos coeficientes da Regressão Logística e da estrutura da Árvore de Decisão
- Métodos globais: importância de variáveis do Random Forest, *permutation importance*, *Partial Dependence Plots*
- Método local: LIME
- Explicações baseadas em exemplos
- Discussão da qualidade das explicações

### Módulo 4 — Ética, enviesamento e regulamentação
`notebooks/04_modulo4_etica_regulamentacao.ipynb`

Comparação de desempenho por grupo (género, raça, escalão etário), riscos éticos, privacidade e proteção de dados, supervisão humana e responsabilidade, e enquadramento regulamentar.

---

## Resultados

Desempenho no conjunto de teste (classe positiva = readmissão < 30 dias):

| Modelo | Accuracy | Precision | Recall | F1 | AUC |
|---|---|---|---|---|---|
| Regressão Logística | 0,661 | 0,180 | 0,573 | 0,274 | 0,670 |
| Árvore de Decisão | 0,674 | 0,180 | 0,539 | 0,270 | 0,661 |
| Random Forest | 0,631 | 0,175 | 0,624 | 0,274 | 0,675 |
| **Random Forest afinado** | 0,623 | 0,175 | **0,638** | 0,274 | **0,676** |

O Random Forest afinado obtém o melhor AUC e o melhor *recall* da classe positiva, à custa de menor *accuracy* global. A precisão baixa (~0,18) reflete o forte desequilíbrio de classes: a maioria dos casos sinalizados como risco não corresponde a readmissão efetiva.

### Variáveis mais relevantes (*permutation importance*, Random Forest)

| Variável | Importância média |
|---|---|
| `number_inpatient` | 0,0284 |
| `discharge_disposition_id` | 0,0275 |
| `metformin` | 0,0091 |
| `number_emergency` | 0,0079 |
| `diag_3` | 0,0070 |
| `diag_1` | 0,0057 |

### Enviesamento por grupo

A análise do Módulo 4 mostra diferenças de desempenho entre grupos. Exemplos:

- **Género** — taxa de falsos negativos de 0,340 nas mulheres (n = 10 924) contra 0,389 nos homens (n = 9 430).
- **Raça** — *recall* de 0,655 no grupo `Caucasian` (n = 15 223) contra 0,300 no grupo `Asian` (n = 123). Os grupos minoritários têm amostras muito reduzidas, pelo que estas métricas devem ser lidas com cautela.
- **Idade** — taxa de falsos positivos de 0,525 no escalão [80-90) contra 0,289 no escalão [40-50).

Tabelas completas em `outputs/tabelas/enviesamento_por_*.csv`.

---

## Estrutura do projeto

```
.
├── data/
│   └── diabetic_data.csv              # Conjunto de dados original
├── notebooks/
│   ├── 01_modulo1_eda_preparacao.ipynb
│   ├── 02_modulo2_modelacao_avaliacao.ipynb
│   ├── 03_modulo3_interpretabilidade.ipynb
│   ├── 04_modulo4_etica_regulamentacao.ipynb
│   └── _rascunho/
├── outputs/
│   ├── dados/                         # Dados limpos
│   ├── figuras/                       # Gráficos gerados
│   └── tabelas/                       # Métricas e resultados em CSV
├── relatorio/
│   └── relatorio_final.md
└── requirements.txt
```

---

## Como executar

```bash
# Criar ambiente virtual
python3 -m venv .venv
source .venv/bin/activate        # No Windows: usar o script activate correspondente

# Instalar dependências
pip install -r requirements.txt

# Abrir os notebooks
jupyter lab
```

Executar os notebooks pela ordem numérica (01 → 04). Os caminhos dos dados são relativos à pasta `notebooks/`.

## Tecnologias

`pandas` · `numpy` · `scikit-learn` · `matplotlib` · `lime` · `shap` · `jupyterlab`

As versões exatas estão fixadas em `requirements.txt`.

---

## Aviso

Trabalho académico com fins de aprendizagem. **O modelo não tem validação clínica e não deve ser utilizado para apoiar decisões médicas.** O desempenho obtido (AUC ≈ 0,68, precisão ≈ 0,18) é insuficiente para uso real, e a análise do Módulo 4 identifica diferenças de desempenho entre grupos demográficos que constituem risco de enviesamento.

## Autor

Francisco Vaz — [@CunhaVaz](https://github.com/CunhaVaz)
