## Módulo 1 — Análise exploratória e preparação dos dados

Neste módulo foi realizada a análise exploratória e a preparação inicial do conjunto de dados diabetic_data.csv, composto por 101 766 observações e 50 variáveis. O conjunto de dados contém informação demográfica, clínica e administrativa relativa a episódios hospitalares de pacientes com diabetes.

A variável-alvo original é readmitted, com três categorias: NO, >30 e <30. A análise da sua distribuição mostrou que 54 864 observações correspondem a pacientes não readmitidos, 35 545 a pacientes readmitidos após mais de 30 dias e 11 357 a pacientes readmitidos em menos de 30 dias.

Para efeitos de modelação posterior, foi criada a variável binária readmitted_30, em que a classe positiva corresponde aos pacientes readmitidos em menos de 30 dias. Após esta transformação, a classe positiva representa 11 357 observações, correspondendo a 11,16% da amostra, enquanto a classe negativa representa 90 409 observações, correspondendo a 88,84%. Esta distribuição revela um desequilíbrio significativo entre classes.

Na etapa de tratamento de valores em falta, os valores representados por ? foram convertidos em valores ausentes reais. A análise mostrou percentagens elevadas de ausência nas variáveis weight, max_glu_serum, A1Cresult, medical_specialty e payer_code. Foram removidas as variáveis weight, payer_code e medical_specialty. As variáveis max_glu_serum, A1Cresult, race, diag_1, diag_2 e diag_3 foram mantidas e os seus valores em falta foram preenchidos com a categoria Unknown.

Após a limpeza, o conjunto de dados preparado ficou com 101 766 observações e 48 variáveis, sem valores em falta.

Foi realizada análise univariada de variáveis numéricas, incluindo time_in_hospital, num_lab_procedures, num_procedures, num_medications, number_outpatient, number_emergency, number_inpatient e number_diagnoses. Esta análise permitiu observar diferenças relevantes de escala e a existência de valores extremos em algumas variáveis.

Também foi realizada análise univariada de variáveis categóricas como race, gender, age, max_glu_serum, A1Cresult, change e diabetesMed. Esta análise permitiu caracterizar a distribuição das principais categorias presentes no conjunto de dados.

Na análise bivariada, foram comparadas variáveis numéricas com a variável-alvo readmitted_30. Observou-se que os pacientes readmitidos em menos de 30 dias apresentavam, em média, maior número de internamentos anteriores. Este padrão sugere que o histórico de internamentos pode ser relevante para a previsão da readmissão precoce.

Foi também calculada uma matriz de correlação para as variáveis numéricas. A variável com maior correlação positiva com readmitted_30 foi number_inpatient, com correlação de aproximadamente 0,165. No entanto, as correlações observadas foram globalmente baixas, indicando que a readmissão precoce não é explicada por uma única variável numérica isolada.

A análise de outliers foi realizada através do método do intervalo interquartil. Foram identificados potenciais valores extremos em variáveis como number_outpatient, number_emergency, number_inpatient e num_procedures. Estes valores não foram removidos automaticamente, uma vez que, em contexto clínico, podem representar situações reais e relevantes.

Por fim, os dados foram separados em variáveis explicativas e variável-alvo. Foram excluídos os identificadores encounter_id e patient_nbr, bem como as variáveis readmitted e readmitted_30 do conjunto de variáveis explicativas. O conjunto final de variáveis explicativas ficou com 44 variáveis.

A divisão entre treino e teste foi realizada com 80% dos dados para treino e 20% para teste, utilizando divisão estratificada. O conjunto de treino ficou com 81 412 observações e o conjunto de teste com 20 354 observações. A proporção da classe positiva manteve-se aproximadamente igual nos dois conjuntos, cerca de 11,16%, garantindo coerência na distribuição da variável-alvo.