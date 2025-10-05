# 📊 Análise de Dinâmicas Processuais e Perfil de Advogados (TJGO)

## 📄 Visão Geral do Projeto

Este projeto de Ciência de Dados realiza uma análise aprofundada de uma base de processos judiciais do Tribunal de Justiça do Estado de Goiás (TJGO) entre 2022 e 2024. O objetivo principal é entender a dinâmica de atuação dos advogados, com foco especial na distribuição de processos, na tendência de casos sigilosos e na construção de um modelo preditivo para estimar o tempo de resolução dos processos.

---
## 🎯 Problema de Negócio e Objetivos

A organização busca entender se há crescimento ou concentração atípica de processos sigilosos em determinados perfis de advogados, o que poderia impactar a eficiência e a equidade do sistema. O projeto visa transformar essa questão em insights acionáveis através de:

* **Segmentação de advogados** conforme seu perfil de atuação.
* **Análise de séries temporais** para identificar tendências no volume de casos sigilosos.
* **Construção de um modelo de Machine Learning** para prever a duração dos processos com base em suas características e no perfil do advogado responsável.

---
## 💾 Base de Dados

A base de dados utilizada consiste em múltiplos arquivos CSV, cada um contendo registros de processos judiciais de um ano específico (2022, 2023, 2024). As principais colunas selecionadas para a análise foram:
* `processo`
* `data_distribuicao`
* `is_segredo_justica`
* `oab`
* `comarca`, `serventia`, `codg_classe`

---
## 🛠️ Metodologia e Fluxo de Trabalho

O projeto seguiu um fluxo estruturado de análise de dados, desde a preparação até a modelagem e avaliação.

### 1. Preparação dos Dados
* Os datasets anuais foram unificados em um único DataFrame Pandas.
* Foi realizado um rigoroso tratamento de dados, incluindo a **validação de OABs via Regex** para remover registros inválidos, e a conversão de colunas de data para o formato `datetime`.
* Foram criadas novas variáveis (`feature engineering`), como o `ano_distribuicao` e métricas agregadas por advogado.

### 2. Análise Estatística Exploratória (EDA)
A exploração revelou padrões não triviais sobre o comportamento dos dados:
* **Distribuição de Processos:** A quantidade de processos por advogado segue um padrão de **Lei de Potência (Pareto/Log-Normal)**, indicando uma forte desigualdade onde poucos advogados concentram um volume massivo de casos. Outliers foram mantidos, pois representam um comportamento real do mercado.
* **Tendência Temporal:** Foi identificada uma **tendência clara e acelerada de crescimento** na proporção de processos sigilosos entre 2022 e 2024.
* **Perfil de Especialização:** A análise aprofundada mostrou que advogados com menor volume total de casos ("Outros 90%") tendem a ter uma proporção maior de processos sigilosos em seus portfólios, sugerindo uma especialização de nicho.

### 3. Modelagem de Machine Learning (Modeling)
* **Tarefa:** Foi definido um problema de **Classificação** para prever a duração de um processo em três categorias: 'Curto Prazo', 'Médio Prazo' e 'Longo Prazo'.
* **Engenharia de Features para o Modelo:** A descoberta mais importante da EDA foi utilizada para criar features de perfil do advogado (`total_processos` e `percentual_sigilo_advogado`), que foram adicionadas ao modelo.
* **Algoritmos:** Foram comparados três modelos: **Random Forest**, **XGBoost** e **LightGBM**.

### 4. Avaliação (Evaluation)
* **Métricas:** A performance foi avaliada usando Acurácia, Precisão, Recall, F1-Score e a Matriz de Confusão.
* **Resultados:** O XGBoost alcançou a maior acurácia (75%). No entanto, a análise detalhada mostrou que o Random Forest foi mais equilibrado, por não ignorar completamente a classe minoritária ("Longo Prazo"), que era um desafio devido ao desbalanceamento dos dados.

---
## 💡 Principais Descobertas e Insights

1.  **O Perfil do Advogado é o Fator Mais Preditivo:** A análise de importância de features do modelo provou que o volume total de casos e a especialização de um advogado (`total_processos`, `percentual_sigilo_advogado`) são mais importantes para prever a duração de um processo do que as características do caso em si (comarca ou tipo).
2.  **Mercado de Extrema Desigualdade:** O ecossistema jurídico analisado é caracterizado por uma distribuição de trabalho altamente concentrada, um insight validado tanto pela análise estatística (distribuição de Pareto) quanto pelo modelo de ML.
3.  **Crescimento de um Nicho Estratégico:** A proporção de casos sigilosos está em crescimento acelerado, e este nicho parece ser mais explorado por advogados especialistas do que pelos grandes players de volume.

---
## 🚧 Limitações e Recomendações Futuras

* **Limitações:** A principal limitação foi o forte **desbalanceamento de classes**, que prejudicou a performance dos modelos na previsão de casos de "Longo Prazo".
* **Recomendações:** Para evoluir o projeto, sugere-se a aplicação de técnicas de balanceamento como **SMOTE**, o teste de novos algoritmos e o ajuste fino de hiperparâmetros.

---
## 🏁 Conclusão Final

O projeto alcançou com sucesso seu objetivo de analisar e extrair insights valiosos da base de dados. Foi possível não apenas caracterizar a estrutura de desigualdade do mercado e identificar tendências importantes, mas também construir um modelo preditivo que validou a principal hipótese do estudo: **o perfil do advogado é a chave para entender a dinâmica do processo**.