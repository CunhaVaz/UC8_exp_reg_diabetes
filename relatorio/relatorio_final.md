# Modelo explicável para previsão de readmissão hospitalar em pacientes diabéticos

## 1. Introdução

Este projeto tem como objetivo desenvolver, avaliar e interpretar modelos de aprendizagem automática aplicados à previsão de readmissão hospitalar em pacientes com diabetes.

O problema é relevante porque a readmissão hospitalar pode estar associada a maior risco clínico, aumento dos custos de saúde e necessidade de melhor acompanhamento dos pacientes após a alta.

O conjunto de dados utilizado contém informação demográfica, clínica e hospitalar de pacientes diabéticos, incluindo variáveis como idade, género, raça, tempo de internamento, número de medicamentos, número de diagnósticos, histórico de internamentos e resultado de readmissão.

A variável-alvo original é `readmitted`, com três categorias: pacientes não readmitidos, pacientes readmitidos após mais de 30 dias e pacientes readmitidos em menos de 30 dias.

Neste trabalho, será dada prioridade à previsão da readmissão em menos de 30 dias, transformando o problema numa tarefa de classificação binária. Assim, a classe positiva corresponde aos pacientes readmitidos em menos de 30 dias, enquanto a classe negativa corresponde aos restantes casos.

O projeto inclui análise exploratória dos dados, preparação dos dados, treino e comparação de modelos interpretáveis e modelos mais complexos, aplicação de métodos de explicabilidade global e local, bem como uma análise crítica de riscos éticos, enviesamento algorítmico e enquadramento regulamentar.

## 2. Definição da variável-alvo

A variável-alvo original do conjunto de dados é `readmitted`, que identifica se um paciente foi readmitido no hospital após a alta. Esta variável apresenta três categorias: `NO`, quando o paciente não foi readmitido; `>30`, quando o paciente foi readmitido após mais de 30 dias; e `<30`, quando o paciente foi readmitido em menos de 30 dias.

No conjunto de dados analisado, existem 54 864 casos classificados como `NO`, correspondendo a 53,91% da amostra; 35 545 casos classificados como `>30`, correspondendo a 34,93%; e 11 357 casos classificados como `<30`, correspondendo a 11,16%.

Para efeitos de modelação, a variável original foi transformada numa variável binária denominada `readmitted_30`. A classe positiva, codificada como `1`, corresponde aos pacientes readmitidos em menos de 30 dias. A classe negativa, codificada como `0`, inclui os pacientes não readmitidos e os pacientes readmitidos apenas após mais de 30 dias.

Esta transformação permite focar o problema preditivo na identificação de pacientes com maior risco de readmissão precoce, uma situação potencialmente mais relevante do ponto de vista clínico e organizacional. Após a transformação, a classe positiva representa 11 357 observações, correspondendo a 11,16% da amostra, enquanto a classe negativa representa 90 409 observações, correspondendo a 88,84%.

Esta distribuição revela um desequilíbrio significativo entre classes, o que deverá ser considerado na fase de modelação. Por esse motivo, a avaliação dos modelos não deverá depender apenas da exactidão global, sendo necessário recorrer também a métricas como precisão, recall, F1-score, matriz de confusão e AUC.
## 3. Tratamento inicial de valores em falta

Após a substituição dos valores representados por `?` por valores em falta reais, foi analisada a percentagem de valores ausentes em cada variável.

A variável `weight` apresentou 96,86% de valores em falta, pelo que foi removida do conjunto de dados. As variáveis `payer_code` e `medical_specialty` apresentaram também percentagens elevadas de ausência, respectivamente 39,56% e 49,08%, e foram removidas nesta fase inicial, por poderem introduzir ruído ou instabilidade no processo de modelação.

As variáveis `race`, `diag_1`, `diag_2`, `diag_3`, `max_glu_serum` e `A1Cresult` foram mantidas. Os seus valores em falta foram preenchidos com a categoria `Unknown`, preservando a informação de ausência como uma categoria explícita.

Esta opção permite evitar a eliminação de observações e manter a dimensão da amostra original, ao mesmo tempo que torna o conjunto de dados adequado para as fases seguintes de preparação e modelação.