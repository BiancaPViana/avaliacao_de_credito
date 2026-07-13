# avaliacao_de_credito
Modelo de ML para classificação de crédito comparando random forest, gradiente boost e XGBoost
Relatório Executivo: Classificação de Score de Crédito

Autoria: Bianca (Data Science Team - Data Girls Finance)

1. Objetivo do Projeto

 O presente projeto teve como foco substituir análises manuais e demoradas por um modelo de Machine Learning capaz de classificar automaticamente o Score de Crédito dos clientes nas categorias: Good (Bom), Standard (Regular) e Poor (Ruim).

2. Etapas da Análise e Pipeline de Dados

Para atingir o objetivo, foram realizadas as segintes etapas:

Limpeza e Tratamento: Foram identificadas que variáveis financeiras e numéricas (como Outstanding_Debt e Total_EMI_per_month) foram exportadas erroneamente como texto devido à presença de caracteres especiais (ex: _).Utilizei para limpexa dos dados Expressões Regulares (Regex) para remover esses caracteres e forçamos a conversão de tipagem para números reais ou seja que possuem casas decimais (float), garantindo a integridade dos cálculos matemáticos do algoritmo.

Engenharia de Recursos (Feature Engineering): : A coluna de histórico de crédito (Credit_History_Age) estava em um formato descritivo que não poderiam ser lidos para cáculos matemáticos pelo modelo (ex: "22 Years and 1 Months"). Desenvolvemos um algoritmo de extração que converteu essa string para uma variável contínua padronizada: Credit_History_Months (ex: 265 meses). Permitindo assim o cálculo de tempo de mercado do cliente

Imputação de Nulos:Para lidar com clientes que possuíam dados nulos (NaN) e valores discrepantes irreais (como idades registradas como 5000 anos) e não descartar as linhas e perder volume de dados valiosos, optamos pela imputação de informações utilizando a Mediana. A mediana foi escolhida estrategicamente por ser imune a valores extremos (outliers), garantindo que não distorceríamos o perfil financeiro médio da base de clientes.

3. O Duelo dos Modelos

Foram testados três algoritmos de classificação em uma base de validação (20% dos clientes):

Random Forest (Vencedor): 79.62% de Acurácia.Tunning de baleceamento de classes não apresentou um aumento significatvi na eficácia do modelo

Gradient Boosting: 77.09% de Acurácia (após tuning de 5000 iterações).

XGBoost Tunado: 73.08% de Acurácia.

O Random Forest apresentou a maior robustez aos dados tabulares, lidando melhor com a natureza das variáveis da empresa sem apresentar overfitting.
Validação Cruzada (Cross-Validation): Para garantir que a acurácia do Random Forest não foi obra do acaso ou de uma divisão "sortuda" dos dados de treino/teste, submetemos o modelo a um  teste de K-Fold Cross-Validation com 5 partições.. A baixa variação (desvio padrão) entre as notas de cada teste comprovou matematicamente que o modelo é capaz de generalizar bem para dados inéditos

4. Principais Achados e Insights de Negócio

Durante a Análise Exploratória (EDA) e a extração da Inteligência do Modelo (Feature Importance), respondemos a perguntas cruciais do negócio:

Quais variáveis têm maior relação com a capacidade de pagamento?
A análise apontou 3 pilares principais para a tomada de decisão:

Outstanding_Debt (18.84%): A Dívida Pendente é o fator principal. Clientes "Good" possuem dívidas medianas em torno de US$ 750, enquanto clientes "Poor" concentram-se na faixa de US$ 1.900 a US$ 2.600.

Credit_History_Months (14.83%): A maturidade do histórico de crédito dita a estabilidade do cliente.

Interest_Rate (13.01%): A taxa de juros a qual o cliente está exposto hoje reflete o seu risco sistêmico.

Qual o perfil de maior probabilidade para o score "Poor"?
Clientes com dívidas pendentes superiores a US$ 1.400, histórico de crédito muito recente (poucos meses de mercado) e que possuem histórico de atrasos e altas taxas de juros ativas.

5. Recomendações Práticas e Aplicação

Como este modelo pode apoiar a equipe de crédito amanhã?

Fila de Priorização Rápida (Fast-Track): Clientes classificados pelo modelo como "Good" podem passar por um processo de aprovação menos burocrático e automatizado, liberando a análise humana para situações onde o modelo tem uma eficácia menor e onde há maior risco financeiro

Revisão de Limites: A Matriz de Confusão nos mostrou o risco de Falsos Positivos. Clientes classificados como "Poor" (especialmente os que estão na margem do limite da dívida) não devem ter o crédito negado sumariamente, mas sim encaminhados para a análise humana detalhada, focando os esforços dos especialistas onde o risco de inadiplência é maior.

6. Cuidados Éticos

É impresicndivel garantir que as variáveis utilizadas para negar crédito não contenham vieses discriminatórios diretos ou indiretos (como CEPs específicos que penalizem populações vulneráveis). O modelo deve agir como uma ferramenta de Apoio à Decisão e manter mecanismos de explicabilidade para que qualquer cliente rejeitado possa receber uma justificativa honesta, transparente, respeitando as leis de proteção ao consumidor e de dados (LGPD).

Implementação do SHAP Framework: Para garantir total transparência e aderência à Lei Geral de Proteção de Dados (LGPD), foi feita a integração a biblioteca SHAP (SHapley Additive exPlanations), para explicar o modelo, gerarando uma explicação masi transparente das variáveis utilizadas pra as decisões tomadas.  Se o modelo classificar alguém como "Poor", o SHAP nos mostra exatamente o peso de cada variável naquela decisão específica (ex: "Score baixo pois a dívida supera US$ 2.000"). Dessa forma, o algoritmo atua como uma ferramenta de apoio à decisão justa, auditável e rastreável.
