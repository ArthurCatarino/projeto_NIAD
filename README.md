🦡 Classificação Morfométrica de Gambás (Possums)
📌 Motivação do Projeto
O objetivo deste projeto foi aplicar um pipeline analítico completo de Machine Learning para resolver um problema de classificação biológica: prever o sexo de gambás australianos utilizando apenas medições físicas (morfometria).

📊 O Banco de Dados
Os dados utilizados pertencem ao dataset OpenIntro Possum, disponível no Kaggle. 
https://www.kaggle.com/datasets/abrambeyer/openintro-possum/data

O dataset original conta com pouco mais de 100 registros de gambás capturados em diferentes regiões da Austrália. O banco de dados inclui variáveis espaciais e biológicas, sendo o nosso foco as métricas corporais contínuas, tais como:

age: Idade do animal.

hdlngth: Comprimento da cabeça.

skullw: Largura do crânio.

totlngth: Tamanho total.

taill: Comprimento da cauda.

footlgth, earconch, chest, belly: Medidas de patas, orelhas, peito e barriga.

A variável alvo (target) do nosso modelo preditivo foi a coluna sex (Macho/Fêmea).

🧹 Principais Desafios na Limpeza e Pré-processamento
Trabalhar com dados biológicos reais trouxe desafios significativos antes da etapa de modelagem:

Tratamento de Valores Nulos (NaNs): Optou-se pela remoção dessas linhas, ja que eram poucas linhas com dados nulos.

Tratamento de Outliers: Decidiu-se manter os outliers, pois representavam a variação natural e a diversidade da espécie, e não erros de digitação no campo ou valores absurdos e impossíveis.

A "Maldição da Dimensionalidade": Aplicamos Engenharia de Atributos para criar variáveis de proporção corpora, já que de acordo com o Randon Forest as nossas colunas não estavam sendo muito significativas para o modelo. Assim podamos algumas variaveis e mantive-mos apenas as 5 mais importantes, afim de eliminar ruídos.

📈 Modelagem e Interpretação dos Resultados
Foram testados quatro modelos de classificação (Regressão Logística, Random Forest, Gradient Boosting e SVM). O algoritmo Support Vector Machine (SVM) obteve a melhor performance, e seus hiperparâmetros foram otimizados via GridSearchCV (kernel='rbf', C=10, gamma=0.1).

Apesar da otimização, o modelo atingiu uma acurácia de 71.25% na validação cruzada, mas obteve 57% no conjunto de teste final. Embora possa parecer um desempenho baixo à primeira vista, esse resultado revela descobertas cruciais sobre a natureza dos dados:

Baixo Dimorfismo Sexual: As variáveis morfométricas (comprimento da cabeça, formato do tronco) não apresentam discrepâncias significativas entre machos e fêmeas dessa espécie. A ausência de diferenças físicas claras faz com que as classes sejam matematicamente sobrepostas, impedindo que os algoritmos tracem fronteiras de decisão lineares precisas.

Alta Variância e "Small Data": O conjunto de teste contava com apenas 21 indivíduos. Nesse cenário reduzido, o impacto percentual de cada erro é estatisticamente superdimensionado. A queda de 71% para 57% representa, na prática, a classificação incorreta de apenas 3 indivíduos a mais do que no treinamento.

Sobretreino (Overfitting): A combinação da amostra reduzida com a busca rigorosa por hiperparâmetros ótimos fez com que o modelo se ajustasse excessivamente às particularidades do conjunto de treino (memorização), falhando ao tentar generalizar para animais não vistos.

Conclusão
O projeto prova matematicamente que a morfometria clássica (uso de fita métrica e paquímetro) não é um preditor computacional robusto para classificar o sexo desta espécie de gambá. Para atingir acurácias de produção, estudos futuros necessitariam integrar variáveis de naturezas distintas, como marcadores genéticos ou dosagens hormonais.
