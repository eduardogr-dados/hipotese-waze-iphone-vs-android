# **Teste de Hipótese: Uso do Waze em iPhone vs. Android**

## Visão Geral do Projeto

Este projeto teve como objetivo principal investigar se existe uma diferença estatisticamente significativa na frequência de uso do aplicativo Waze, medida pelo número médio de viagens (`drives`) por mês, entre usuários de dispositivos iPhone e Android. A análise foi motivada por uma solicitação da equipe de liderança da Waze para entender melhor o comportamento dos usuários em diferentes plataformas, como parte de um esforço maior para aumentar a retenção e o crescimento.

O escopo do projeto abrangeu as seguintes etapas:
1.  **Carregamento e Exploração Inicial dos Dados:** Compreensão da estrutura do dataset sintético fornecido (`waze_dataset.csv`).
2.  **Limpeza e Preparação dos Dados:** Verificação e tratamento de valores ausentes e duplicatas, com foco nas variáveis de interesse (`drives` e `device`).
3.  **Análise Exploratória de Dados (EDA):** Utilização de estatísticas descritivas e visualizações (histogramas, box plots) para entender a distribuição da variável `drives` e compará-la visualmente entre os grupos de dispositivos.
4.  **Teste de Hipótese:** Aplicação de um teste t de duas amostras independentes (especificamente o teste de Welch) para avaliar formalmente a diferença entre as médias de `drives` dos usuários de iPhone e Android.

As ferramentas utilizadas foram **Python**, com as bibliotecas **Pandas** para manipulação de dados, **Matplotlib** e **Seaborn** para visualização, e **SciPy** para a execução do teste estatístico. O foco central foi a aplicação de técnicas de análise estatística para derivar insights acionáveis a partir dos dados.

## Compreensão dos Dados

Os dados utilizados neste projeto originam-se do arquivo `waze_dataset.csv`, um conjunto de dados sintético contendo informações de 14.999 usuários fictícios do Waze. O dataset inclui 13 colunas, como `ID` do usuário, `sessions` (número de sessões no mês), `drives` (número de viagens > 1km no mês), `device` (iPhone ou Android), e outras métricas de uso e tempo de vida do usuário.

Durante a fase de exploração e limpeza, foram identificados os seguintes pontos:
*   **Valores Ausentes:** A coluna `label` (indicadora de retenção/churn) continha 700 valores ausentes. Uma análise comparativa mostrou que as características gerais dos usuários com dados ausentes eram muito similares às dos usuários sem dados ausentes, e a distribuição por `device` nesses casos era consistente com a distribuição geral. Como a coluna `label` não era o foco desta análise específica, e as colunas cruciais (`drives` e `device`) estavam completas, esses valores ausentes não impactaram diretamente o teste de hipótese.
*   **Duplicatas:** Nenhuma linha duplicada foi encontrada no conjunto de dados, considerando as colunas relevantes.
*   **Outliers e Distribuição:** A análise da variável `drives` revelou uma distribuição fortemente assimétrica à direita (média de 67.3, mediana de 48.0), indicando que a maioria dos usuários realiza menos viagens, mas alguns são muito ativos. O método IQR identificou aproximadamente 4.9% dos valores como potenciais outliers superiores (> 202.5 viagens), o que é consistente com a natureza assimétrica dos dados de uso. Esses outliers foram mantidos na análise, pois representam comportamento real (ainda que extremo) dos usuários.

**Limitações:** A principal limitação é que os dados são sintéticos, o que significa que podem não refletir as nuances do comportamento real dos usuários do Waze. Além disso, esta análise focou especificamente na média de `drives`, e outros aspectos do comportamento do usuário podem diferir entre as plataformas.

## Análise

A análise foi conduzida para responder à questão central: *A média de viagens (`drives`) difere significativamente entre usuários de iPhone e Android?*

**Análise Exploratória de Dados (EDA):**
*   A EDA visual (histogramas e box plots comparativos) mostrou que as distribuições de `drives` para usuários de iPhone e Android eram muito semelhantes em forma, tendência central e dispersão.
*   As medianas eram quase idênticas (iPhone: 48.0, Android: 47.0).
*   As médias também eram muito próximas (iPhone: 67.86, Android: 66.23).
*   Essa similaridade visual e numérica sugeriu, preliminarmente, que não haveria uma diferença significativa.

**Teste de Hipótese:**
Foi realizado um teste t de duas amostras independentes para comparar formalmente as médias.
*   **Hipótese Nula (H₀):** Não há diferença estatisticamente significativa entre a média de `drives` de usuários de iPhone e a média de `drives` de usuários de Android (μ_iphone = μ_android).
*   **Hipótese Alternativa (H₁):** Existe uma diferença estatisticamente significativa entre as médias de `drives` (μ_iphone ≠ μ_android).
*   **Nível de Significância (α):** Foi definido em 0.05 (5%).
*   **Teste Utilizado:** Teste t de Welch (via `scipy.stats.ttest_ind` com `equal_var=False`), que é robusto a diferenças nas variâncias entre os grupos.
*   **Resultado:** O teste produziu um **valor-p de aproximadamente 0.1434**.

Como o valor-p (0.1434) é maior que o nível de significância (0.05), **falhamos em rejeitar a hipótese nula (H₀)**.

## Conclusão

O principal resultado desta análise é que não há evidência estatística suficiente para concluir que existe uma diferença na **média de viagens mensais** (`drives`) entre usuários de Waze que utilizam **iPhone** e aqueles que utilizam **Android**, ao nível de significância de 5%. A pequena diferença observada nas médias amostrais (iPhone ligeiramente superior) é provavelmente devida à variabilidade amostral aleatória e não a uma diferença real nas populações de usuários.

Este projeto demonstra a aplicação de testes de hipótese para validar observações da análise exploratória e fornecer respostas baseadas em evidências a questões de negócio. O resultado num contexto real evitaria investimentos em estratégias potencialmente desnecessárias (como campanhas de marketing distintas por dispositivo baseadas apenas na frequência de viagens) e direcionaria a atenção para áreas de investigação mais promissoras.
