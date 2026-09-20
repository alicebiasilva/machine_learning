# KNN 

### 1. O que é KNN e qual é a intuição por trás do algoritmo?

O KNN (K-Nearest Neighbors) é um algoritmo supervisionado que pode ser usado em classificação ou regressão e que é não paramétrico, porque se baseia na premissa de que observações que são parecidas entre si tendem a ter respostas parecidas, e que para determinar a "semelhança" basta calcular a distância entre as amostrar no plano.
A intuição é semelhante a perguntar: “Quais exemplos do meu histórico são mais parecidos com este novo caso e o que aconteceu com eles?”.

O KNN não aprende uma fórmula explícita para relacionar as features ao target, ele basicamente mantém os dados de treinamento e, quando recebe uma nova observação, procura quais são as K observações mais próximas dela de acordo com alguma métrica de distância. 

O parâmetro K controla quanto contexto local usamos para tomar a decisão: um K pequeno torna o modelo mais sensível aos vizinhos individuais e ao ruído, enquanto um K maior produz uma decisão mais suave, mas pode fazer o modelo perder características locais importantes.

---

### 2. Como o KNN decide a classe ou o valor previsto de uma nova observação e como escolher o melhor K?

O KNN decide a previsão de uma nova observação seguindo basicamente três etapas: calcula as distâncias até as observações do conjunto de treinamento, seleciona os K vizinhos mais próximos e usa esses vizinhos para produzir a previsão. 

Em um problema de classificação, o resultado é definido por voto, que pode ser simples ou com pesos e penalizações a depender das premissas do problema. O resultado pode ser a classe ou as probabilidades, e uma abordagem comum é ajustar o corte de probabilidade em um problema com evento raro.

Quando K é um valor par e os vizinhos mais votados forem de classes diferentes, o resultado dependerá da ordm dos dados de treinamento e por isso é uma boa prática adotar K ímpar.

Ainda, em um problema de regressão, em vez de votar em uma classe, o KNN combina os valores dos vizinhos, normalmente calculando a média. Essa previsão também pode ser ponderada pela distância. 

Para escolher o melhor valor de K, um ponto de partida comum e aceito é iniciar pelo valor da raíz de n, (número de amostras), porém não é garantia de melhor desempenho. Na prática, é recomendado testar diferentes valores de K e plotar uma curva relacionando o K com uma métrica de validação, como acurácia, F1-,score ou erro de classificação. A partir dessa curva, pode-se analisar como o desempenho varia conforme K aumenta e escolher um valor que apresente bom desempenho de validação, evitando valores muito baixos, que podem deixar o modelo sensível ao ruído e levar a overfitting, e valores muito altos, que podem deixar as decisões excessivamente generalizadas e levar a underfitting. Idealmente, essa comparação pode ser feita utilizando validação cruzada para que a escolha de K seja mais robusta.

---

### 3. Como escolher a métrica de distância e qual é o impacto de usar Euclidiana, Manhattan ou outras?

A escolha da métrica de distância no KNN é importante porque ela define o que significa duas observações serem “parecidas” e, consequentemente, quais vizinhos serão utilizados para fazer a previsão. 

A distância **Euclidiana** é a mais conhecida e corresponde à distância em linha reta entre dois pontos; ela funciona bem quando temos variáveis numéricas contínuas e faz sentido considerar diferenças em todas as dimensões de forma conjunta. 

A distância **Manhattan** soma as diferenças absolutas entre as coordenadas, sendo mais parecida com percorrer uma cidade seguindo ruas em uma malha; ela pode ser uma alternativa interessante quando queremos **reduzir a influência de diferenças muito grandes em uma determinada dimensão**, embora ainda seja sensível à escala. 

Já a distância **Minkowski** generaliza Euclidiana e Manhattan. Note que como a fórmula da distância Euclidiana envolve elevar as difereças ao quadrado (distância = raiz[(x1-y1)^2 + (x2-y2)^2], e a distância de Manhattan envolve apenas módulos (distância = |x1-y1| + |x2,y2|), a distância Euclidiana penaliza as diferenças ao quadrado, dando maior importância para elas, e podendo ser mais sensível em presença de outliers.

Por fim, a distância **Hamming** é usada para variáveis categóricas. Em termos simples, ela nos diz se duas variáveis categóricas são iguais ou não, em relação ao número deposições em que os caracteres correspondentes são diferentes (só funciona quando há strings ou arrays de mesmo comprimento).

Um ponto ainda mais importante é que a escolha da distância não pode ser separada do pré-processamento: se uma feature varia de 0 a 1 e outra de 0 a 100.000, a segunda pode dominar completamente a distância Euclidiana ou Manhattan. Por isso, normalmente aplicamos padronização ou outra transformação de escala antes do KNN. 

---

### 4. Qual é o impacto da escolha de K? O que acontece com K muito pequeno ou muito grande?

O K determina quantos vizinhos o KNN considera para fazer uma previsão e, portanto, controla o quanto o modelo é sensível aos exemplos individuais. 

Quando usamos um **K muito pequeno**, como K = 1, o modelo olha para pouquíssimos vizinhos e fica extremamente sensível à estrutura local dos dados: uma única observação ruidosa ou um outlier pode determinar a previsão. Isso tende a gerar um modelo com **baixo viés e alta variância**, ou seja, muito flexível e capaz de se adaptar excessivamente aos dados de treinamento, aumentando o risco de overfitting. 

Por outro lado, quando usamos um **K muito grande**, o modelo considera uma região muito maior do espaço e a decisão começa a ser influenciada por muitas observações que podem não ser realmente semelhantes à nova observação. O modelo fica mais suave e menos sensível ao ruído, reduzindo a variância, mas pode perder padrões locais importantes e **sofrer underfitting, caracterizando uma situação de maior viés e menor variância**. 

Em classificação, existe ainda um detalhe: se as classes estiverem desbalanceadas, um K muito grande pode fazer com que a classe majoritária domine o voto simplesmente porque ela aparece com muito mais frequência na vizinhança. 

Por isso, não existe um K universalmente ideal; normalmente **escolhemos esse hiperparâmetro usando validação cruzada, testando diferentes valores e avaliando o desempenho no conjunto de validação.** 

---

### 5. Por que o escalonamento das features é especialmente importante no KNN?

Porque o algoritmo toma suas decisões diretamente com base na distância entre as observações. Imagine que temos duas variáveis para comparar clientes: idade, que varia aproximadamente de 0 a 100, e renda, que pode variar de 0 a 100.000. Se utilizarmos distância Euclidiana sem nenhum tratamento, a diferença de renda pode ser milhares de vezes maior numericamente do que a diferença de idade e, consequentemente, a renda pode praticamente dominar o cálculo da distância. Isso significa que duas pessoas com rendas parecidas podem ser consideradas “próximas”, mesmo que sejam muito diferentes em idade, simplesmente porque a variável renda está em uma escala maior. 

O problema não é que renda seja necessariamente mais importante; é que a unidade de medida está influenciando artificialmente a noção de similaridade do algoritmo. Ao aplicar, por exemplo, **padronização**, colocamos as features em escalas comparáveis, **fazendo com que uma diferença de um desvio-padrão em idade tenha uma influência semelhante à diferença de um desvio-padrão em renda.** Depois disso, a distância passa a refletir melhor a combinação das características, em vez de ser dominada pela variável numericamente maior. 

No KNN, a escala pode literalmente mudar quais observações são consideradas vizinhas, alterando diretamente a previsão. Por isso, em um pipeline de KNN, eu normalmente verificaria as escalas das variáveis e aplicaria uma transformação adequada, como StandardScaler ou Min-Max, antes de calcular as distâncias, sempre ajustando o scaler somente no conjunto de treinamento para evitar data leakage. 

---

### 6. Como o KNN se comporta em datasets com muitas dimensões? Explique a maldição da dimensionalidade.

O KNN tende a ter dificuldades em datasets com muitas dimensões por causa do que chamamos de maldição da dimensionalidade. A intuição é que, à medida que adicionamos mais features, o espaço de possibilidades cresce muito rapidamente e os dados ficam cada vez mais pertos um do outro no espaço e a noção de "vizinhos" perde o sentido, fazendo com queo algoritmo perca desempenho.

Imagine que temos pontos em um espaço de uma dimensão: é relativamente fácil encontrar observações próximas. Em duas dimensões, ainda conseguimos visualizar regiões de proximidade; mas, conforme adicionamos dezenas ou centenas de dimensões, torna-se muito mais difícil encontrar pontos realmente próximos em todas as características simultaneamente. Isso é especialmente problemático para o KNN porque o algoritmo depende justamente da ideia de que distâncias representam bem a similaridade. 

Em alta dimensionalidade, as distâncias entre diferentes observações tendem a ficar mais parecidas entre si, um fenômeno chamado **concentração das distâncias**; o vizinho mais próximo pode deixar de ser significativamente mais próximo do que os demais. 

Além disso, cada nova dimensão aumenta o custo computacional para calcular as distâncias (O(n*d) onde n = amostrar e d = variáveis) e pode introduzir features irrelevantes ou ruidosas que passam a influenciar a definição dos vizinhos. 

Por isso, em problemas de alta dimensionalidade, é recomendado utilizar **seleção de features ou redução de dimensionalidade, como PCA, além de avaliar se KNN realmente é o algoritmo mais adequado**.

---

### 7. Quais são as principais vantagens, limitações e custos computacionais do KNN? Em que situações você escolheria ou evitaria esse modelo?

O KNN tem como principal vantagem a simplicidade: ele é intuitivo, não exige uma hipótese explícita sobre a forma da relação entre features e target e consegue capturar padrões locais e fronteiras de decisão bastante complexas. Além disso, como é um modelo não paramétrico e baseado em instâncias, praticamente não existe uma etapa tradicional de treinamento; os dados são armazenados e o trabalho mais pesado acontece quando precisamos fazer uma previsão.

Por outro lado, essa característica também traz uma importante limitação: a inferência pode ser cara, porque, para uma nova observação, precisamos calcular sua distância em relação a muitas ou até todas as observações do conjunto de treinamento e então encontrar os K vizinhos mais próximos. De forma simplificada, para n observações de treino, d features e m novas observações, uma busca ingênua pode ter custo próximo de O(m × n × d), embora estruturas de busca e técnicas de aproximação possam reduzir esse custo em alguns cenários. 

O KNN também pode consumir bastante memória, porque precisa manter os dados de treinamento disponíveis para realizar novas consultas.

Outra limitação importante é sua sensibilidade à escala das features, à escolha da métrica de distância e à maldição da dimensionalidade; em muitas dimensões ou com muitas features irrelevantes, a noção de proximidade pode perder significado. Além disso, o desempenho pode ser prejudicado por ruído, outliers e distribuições muito desbalanceadas. 

Eu consideraria KNN quando tenho um dataset de tamanho moderado, poucas ou moderadas dimensões, uma noção de similaridade bem definida e acredito que observações próximas realmente tenham comportamentos semelhantes; ele também pode ser útil como baseline por sua simplicidade. Eu evitaria KNN quando tenho milhões de observações, alta dimensionalidade, necessidade de baixa latência na inferência ou quando a distância não representa bem a similaridade entre as observações. 

---

### 8. Como o modelo lida com outliers e valores faltantes?

O KNN é sensível tanto a outliers quanto a valores faltantes, porque sua previsão depende diretamente da distância entre as observações. 

Em relação aos outliers, uma observação extrema pode ficar muito distante das demais e, portanto, geralmente não será escolhida como vizinha; porém, ela também pode **distorcer a escala das features e alterar as distâncias de outras observações**, principalmente quando usamos métricas como a Euclidiana. 

Já os valores faltantes são um problema mais direto, porque, se uma feature está ausente, não conseguimos calcular corretamente a distância entre aquela observação e as demais. O KNN tradicional, portanto, não sabe lidar naturalmente com NaN: precisamos tratar os missings antes da etapa de cálculo das distâncias. 

Uma possibilidade é utilizar imputação, como mediana para variáveis numéricas ou moda para categóricas, mas a escolha deve considerar o contexto e o mecanismo de ausência dos dados. Também podemos utilizar métodos de imputação mais sofisticados, inclusive **KNN Imputer, que estima um valor ausente com base em observações semelhantes**, embora isso exija cuidado para evitar leakage e problemas de circularidade no pipeline. É importante também que qualquer transformação, como scaling ou imputação baseada nos dados, seja ajustada somente no conjunto de treinamento e depois aplicada aos dados de validação e teste. 

---

### 9. O modelo necessita de algum tipo de pré processamento dos dados?

Sim, o KNN normalmente exige mais atenção ao pré-processamento do que muitos outros algoritmos, principalmente porque ele toma suas decisões com base na distância entre as observações. 

O primeiro ponto é o **escalonamento** das features: se uma variável estiver em uma escala muito maior que outra, ela pode dominar o cálculo da distância, então normalmente aplicamos padronização ou normalização para colocar as variáveis em escalas comparáveis. 

Também precisamos tratar **valores faltantes**, porque o KNN tradicional não consegue calcular corretamente a distância quando existem NaN; podemos utilizar técnicas de imputação adequadas ao contexto. 

Para **variáveis categóricas**, precisamos transformá-las em uma representação numérica compatível com a métrica de distância, mas devemos tomar cuidado, porque simplesmente atribuir números como 0, 1, 2 pode criar uma relação de ordem e distância que não existe. Dependendo do caso, podemos usar one-hot encoding ou uma métrica específica para dados categóricos. 

Também devemos avaliar **outliers**, porque eles podem distorcer as distâncias e a definição das vizinhanças, embora não devam ser removidos automaticamente sem entender se representam erros ou observações legítimas. 

Outro ponto importante é a **seleção de features**: como o KNN sofre com a maldição da dimensionalidade, remover variáveis irrelevantes ou redundantes pode melhorar bastante o desempenho. 

Depois do pré-processamento, precisamos **escolher hiperparâmetros** como K, métrica de distância e eventualmente pesos por distância, normalmente utilizando validação cruzada. 

---

### 10. O modelo faz alguma pré suposição sobre os dados?

O KNN não impõe fortes premissas estatísticas sobre a distribuição dos dados, mas faz uma premissa fundamental de localidade: pontos próximos devem ter respostas semelhantes. A qualidade do modelo depende justamente de essa noção de proximidade ser representativa do problema.

---

### 11. Quais os tipos de voto dos vizinhos?

No KNN, existem três principais formas de definir o voto dos vizinhos:

1. Voto majoritário: cada vizinho tem a mesma importância e vence a classe que aparecer com maior frequência entre os K vizinhos. É simples e funciona bem quando os vizinhos têm distâncias semelhantes.
2. Voto ponderado pela distância: vizinhos mais próximos recebem mais peso, normalmente usando uma função inversamente proporcional à distância. 
3. Voto baseado em pesos: pode ser aplicada em classes desbalanceadas por exemplo, para que classes menos frequentes tenham um peso para compensar a falta de representatividade.

---

### 12. Por que a padronização das variáveis é importante e quais os tipos de abordagem?

A padronização é especialmente importante porque o algoritmo calcula a distância enter as observaçoes e, portanto, variáveis em escalas diferentes podem fazer com que uma variável domine o cálculo da distância.

O **MinMaxScaler** transforma os valores entre 0 e 1, sendo interessante quando deseja-se colocar todas as variáveis em uma escala limitada e os dados não possuem outliers.

O **SantandScaler** transforma os dados para terem média 0 e desvio padrão 1, sendo uma boa escolha quando as variáveis têm distribuições aproximadamente simétricas e não apresentam muitos valores extremos.

Já o **RobustScaler** utiliza estatísticas mais resistentes a outliers, como mediana e intervalo interquartil, com o objetivo de mitigar a distorção causada por esses outliers.

---

### 13. Como o algoritmo lida com dados desbalanceados?

---

### 14. Como podemos tornar a inferência do algoritmo mais rápida?

O gargalo do algoritmo é a busca por vizinhos mais próximos. Como KNN é um lazzy learner, ele não "treina" um modelo paramétrico: toda a carga computacional está na fase de predição, quando precisa calcular distâncias enter o ponto de teste e os pontos do dataset.

Na forma mais simples, chamada **brute force**, o KNN calcula a distância do  ponto de teste para todos os pontos de treino. Isso funciona bem para datasets pequenos, mas escala muito mal: o custo cresce lineramente com o número de observações e de dimensões, o que rapidamente torna o modelo inviável na prática.

Alterativas:

* KD-Tree (k-dimensional tree): divide o espaço em regiões menores para facilitar buscas no processo de encontrar os vizinhos mais próximos de um ponto. Durante a busca, apenas as regiões relevantes são exploradas, eliminando regiões irrelevantes.

* Ball-Tree: organiza os dados em regiões esféricas em vez de cortes alinhados aos eixos (KD-Tree). Isso torna essa abordagem mais flexível em espaços de maior dimensionalidade ou quando as distribuições de dados não se alinham bem aos eixos. Em cenários de alta dimensão, tende a performar melhor que KD-Tree.

* Approximante Nearest Neightbors: abre-se mão de encontrar os k-vizinhos ótimos em troca de uma resposta muito mais rápida, o que é bastante útil em sistemas de recomendação, aplicações em tempo real ou datasets massivos.

---

### 15. Fale sobre técnicas de tratamento de variáveis categorias.

Para tratar variáveis categóricas, deve-se escolher a técnica de acordo principalmente com a natureza da variável, a quantidade de categorias e o algoritmo utilizado. 

Para variáveis nominais com poucas categorias, uma abordagem comum é o One-Hot Encoding, que cria uma variável binária para cada categoria e evita estabelecer uma ordem artificial entre elas. 

Para variáveis ordinais, em que existe uma ordem natural entre as categorias, como baixo, médio e alto, pode-se utilizar uma codificação ordinal, atribuindo valores que representem essa ordem. 

Quando temos variáveis categóricas com alta cardinalidade, alternativas como Target Encoding podem ser utilizadas, substituindo cada categoria por uma estatística relacionada à variável alvo, mas é necessário ter cuidado com vazamento de informação e realizar o encoding adequadamente dentro da validação. 

Também existem embeddings, que representam as categorias como vetores numéricos aprendidos, sendo bastante utilizados em modelos de redes neurais e situações com muitas categorias.

---

### 16. Em um problema de classificação com a variável alvo desbalanceada, por exemplo, com 90% das observações pertencendo à classe majoritária e apenas 10% à classe minoritária, quais métricas você utilizaria para avaliar o modelo e como decidiria qual delas é mais adequada ao problema?

Em um problema desbalanceado, eu evitaria utilizar apenas a acurácia, porque ela pode dar uma falsa impressão de bom desempenho. Por exemplo, se 90% dos dados pertencem à classe majoritária, um modelo que sempre previsse essa classe teria 90% de acurácia, mas não identificaria nenhum caso da classe minoritária. Por isso, analisaria a matriz de confusão e métricas como precision, recall e F1-score. Se o objetivo principal for não deixar passar casos da classe minoritária, eu priorizaria o recall, porque ele mede a proporção dos positivos reais que foram identificados pelo modelo. Se for mais importante que os casos classificados como positivos sejam realmente positivos, daria maior atenção à precision. Quando existe um equilíbrio entre precision e recall, utilizaria o F1-score, que é a média harmônica entre os dois. Também poderia analisar a PR-AUC, especialmente quando a classe positiva é muito rara, porque ela permite avaliar o trade-off entre precision e recall em diferentes limiares. Portanto, a escolha da métrica deve estar relacionada ao custo dos diferentes tipos de erro e ao objetivo do problema.

---

### 17. Em um problema de classificação utilizando KNN, você identifica que as classes estão desbalanceadas. Quais técnicas você poderia utilizar para tratar esse desbalanceamento, como undersampling, oversampling e SMOTE, e quais cuidados teria ao aplicar essas técnicas especificamente no KNN?

Em um problema de KNN com classes desbalanceadas, eu poderia utilizar técnicas como undersampling, que reduz a quantidade de observações da classe majoritária, oversampling, que aumenta a representação da classe minoritária, ou SMOTE, que cria novas observações sintéticas da classe minoritária a partir de seus vizinhos. No KNN, esse tratamento é especialmente relevante porque o modelo toma sua decisão com base nos vizinhos mais próximos, então uma classe majoritária muito mais representada pode dominar o voto e prejudicar a identificação da classe minoritária. Por outro lado, o oversampling pode aumentar a influência de observações repetidas e o SMOTE pode criar pontos sintéticos que alteram a distribuição local dos dados, o que é particularmente importante em um algoritmo baseado em distância. Por isso, eu compararia essas estratégias utilizando validação cruzada estratificada e métricas como recall, precision e F1-score, escolhendo a abordagem de acordo com o custo dos erros no problema. Além disso, faria o balanceamento apenas nos dados de treinamento de cada fold, evitando aplicar oversampling ou SMOTE antes da divisão dos dados, pois isso poderia causar vazamento de informação.