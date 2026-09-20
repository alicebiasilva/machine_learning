# Naive Bayes

### 1. O que é Naive Bayes? Por que o algoritmo é chamado de “Naive”?

O Naive Bayes é um algoritmo supervisionado, paramétrico e de classificação probabilístico baseado no Teorema de Bayes. A ideia central é calcular, para uma nova observação, a probabilidade de ela pertencer a cada uma das classes possíveis e, ao final, classificá-la na classe com maior probabilidade.

A ideia é que com base no histórico é possível obter a probabilidade à prori para a classe de interesse, como por exemplo 30% para a classe A e 70% para a classe B. Mas, para não indicar sempre a classe com maior probabilidade, ao receber um novo registro tenta-se retirar informações dele na intenção de encontrar características que aumentem a chance de A ou de B. Essas informações novas da amostra funcionam como evidências que podem favorecer uma classe em relação à outra. Isso significa que o algoritmo usa a probabilidade a priori e as evidências observadas na nova amostra para calcular a probabilidade posterior.

> "Eu tinha uma probabilidade inicial para cada classe. Agora que observei as características dessa nova entrada, vou usar essas características para atualizar essa probabilidade de esse novo registro em especial ser da classe A ou não."

Ele calcula algo como 
> P(A)×P(feature_1∣A)×P(feature_2|A)
> P(B)×P(feature_1∣B)×P(feature_2|B)

E escolhe o resultado de maior probabilidade.

O nome “Naive” vem da principal simplificação feita pelo algoritmo: ele assume que as features são condicionalmente independentes umas das outras dado o conhecimento da classe, o que nos permite simplificar o cálculo das probabilidades combinadas apenas multiplicando-as pelo princípio de independência. Apesar dessa hipótese simplificadora, o Naive Bayes pode funcionar muito bem em problemas de classificação, especialmente em classificação de textos, porque consegue trabalhar com muitas features e poucos dados de treinamento de forma extremamente eficiente. 

Por fim, ele é considerado um algoritmo paramétrico porque assume determinada estrutura probabilística para os dados e estima um conjunto de parâmetros a partir do treinamento. Por exemplo, no Gaussian Naive Bayes, para cada feature e para cada classe, o modelo estima os parâmetros de uma distribuição normal: média e variância.

---

### 2. Como o algoritmo calcula P(classe | features) na prática? Explique o papel de P(classe) e P(feature | classe).

Na prática, o Naive Bayes utiliza o Teorema de Bayes para calcular a probabilidade de uma observação pertencer a uma determinada classe dado o conjunto de features, ou seja, P(classe | features) = P(features | classe) × P(classe) / P(features). 

O P(classe) é o prior, que representa a probabilidade daquela classe antes de observarmos as features; por exemplo, se 20% dos e-mails do histórico são spam, podemos estimar P(spam) = 0,20. 

Já P(feature | classe) representa a probabilidade de observar aquela característica sabendo que estamos naquela classe; por exemplo, podemos descobrir que a palavra “promoção” aparece em 70% dos e-mails que são spam, então P(promoção | spam) = 0,70. Quando temos várias features, entra a hipótese “naive”: assumimos que elas são condicionalmente independentes dado a classe, então P(features | classe) pode ser decomposto em um produto, como P(X₁ | classe) × P(X₂ | classe) × ... × P(Xₚ | classe). Imagine então que queremos classificar um e-mail que contém “promoção” e “desconto”: para a classe spam, calculamos P(spam) × P(promoção | spam) × P(desconto | spam); fazemos o mesmo para a classe não spam e comparamos os resultados. 

O termo P(features) funciona como um fator de normalização comum às classes, então, para classificar, não precisamos necessariamente calculá-lo: basta comparar P(classe) × P(features | classe) entre as classes e escolher a maior. Por exemplo, se depois dos cálculos tivermos um valor de 0,14 para spam e 0,04 para não spam, o spam será a classe escolhida; se quisermos as probabilidades propriamente ditas, aí sim precisamos normalizar esses valores pela soma. 

Portanto, a lógica é: **o prior diz quão comum a classe é antes das evidências; P(feature | classe) diz quão compatível cada feature é com aquela classe; o algoritmo combina essas informações para determinar qual classe é mais provável dado o conjunto de features.**

---

### 3. Quais são as principais variantes do Naive Bayes, como Gaussian, Multinomial e Bernoulli, e quando você utilizaria cada uma?

As principais variantes do Naive Bayes diferem principalmente pela distribuição probabilística que assumimos para as features, e por isso a escolha depende do tipo de dado que estamos modelando. 

O **Gaussian Naive Bayes** assume que as features **numéricas contínuas** seguem uma distribuição normal dentro de cada classe, então é uma opção natural quando temos variáveis contínuas, como idade, temperatura ou renda. Usa a máxima verossimilhança "quão plausível é eu ver o que eu vi?".

O **Multinomial Naive Bayes** é muito utilizado quando as features representam **contagens ou frequências**, sendo especialmente comum em classificação de textos; por exemplo, podemos representar um documento pela quantidade de vezes que cada palavra aparece e estimar a probabilidade dessas contagens para cada classe. ("quantas vezes a palavra x aparece no texto?")

Já o **Bernoulli Naive Bayes** é adequado quando as **features são binárias**, indicando presença ou ausência de uma característica; em classificação de textos, por exemplo, podemos representar cada palavra como 1 se ela aparece no documento e 0 se não aparece, independentemente de quantas vezes apareceu. 

---

### 4. Qual é o problema de probabilidade zero e como o Laplace Smoothing resolve isso?

O problema de probabilidade zero acontece quando usamos Naive Bayes e uma determinada feature nunca apareceu em uma determinada classe durante o treinamento. 

Imagine um classificador de spam em que a palavra “bitcoin” aparece em vários e-mails que são spam, mas nunca aparece em nenhum e-mail que não é spam; então, para a classe não spam, podemos estimar P(bitcoin | não spam) = 0. 
**Como o Naive Bayes multiplica as probabilidades das features, se uma única delas for zero, todo o produto daquela classe se torna zero**, fazendo o modelo considerar que a probabilidade daquela classe é zero, mesmo que todas as outras evidências sejam favoráveis. 

O Laplace Smoothing, ou suavização de Laplace, resolve isso adicionando uma pequena quantidade, normalmente 1, às contagens antes de calcular as probabilidades. 

Isso evita que uma ocorrência nunca observada no treinamento seja interpretada como impossível no mundo real. É importante entender que a suavização não está “inventando” que aquela palavra realmente apareceu; ela está dizendo que, com dados finitos, não observar um evento não significa necessariamente que sua probabilidade verdadeira seja zero. 

--- 

### 5. Quais são as principais vantagens e limitações do Naive Bayes? Em quais tipos de problema ele costuma funcionar particularmente bem?

O Naive Bayes tem como principais vantagens a simplicidade, velocidade e eficiência, especialmente quando temos muitas features e uma quantidade relativamente pequena de dados:

* O treinamento costuma ser muito rápido e também barato computacionalmente, além de exigir relativamente poucos dados para começar a produzir bons resultados;
* O algoritmo funciona bem em problemas com alta dimensionalidade, como classificação de textos, porque conseguimos ter milhares ou milhões de features — por exemplo, palavras — sem necessariamente tornar o modelo inviável;
* Ele pode funcionar bem mesmo quando a hipótese de independência não é perfeitamente verdadeira; a suposição é simplificadora, mas não necessariamente impede boas previsões. 

Por outro lado, algumas limitações são:

* Quando existe forte dependência entre as features o modelo pode contar a mesma evidência várias vezes e produzir probabilidades mal calibradas, mesmo que a classificação final seja razoável; 
* O desempenho depende das hipóteses de distribuição adotadas, como Gaussian para features contínuas ou Multinomial para contagens, e essas hipóteses podem não representar adequadamente os dados;
* O modelo tende a ter dificuldade em capturar relações complexas e interações entre features, porque sua estrutura é bastante simples;
* Deve-se ter cuidado com probabilidades zero, resolvidas normalmente com técnicas como Laplace Smoothing, e com a interpretação das probabilidades quando há forte dependência entre variáveis. 

Na prática, o Naive Bayes costuma funcionar particularmente bem em classificação de textos, como spam detection, análise de sentimento e categorização de documentos, além de problemas em que temos muitas features, pouco dado de treinamento e necessidade de um modelo extremamente rápido. 

---

### 6. Como lidar com valores ausentes em um modelo Naive Bayes?

O Naive Bayes matematicamente consegue fazer o raciocínio sem uma feature que esteja faltando: simplesmente não usa aquela evidência para aquela observação. Mas uma implementação específica pode não aceitar NaN diretamente e gerar erro no treinamento ou na predição. Por isso, em uma aplicação real, não devemos assumir que o algoritmo vai automaticamente ignorar o missing; precisamos verificar a implementação e, frequentemente, fazer uma etapa de tratamento dos valores faltantes antes do Naive Bayes.

Para mitigar esse problema, deve-se avaliar o tipo da variável e o significado da ausência. Para variáveis numéricas, uma abordagem comum é utilizar imputação pela mediana ou média, enquanto para categóricas podemos utilizar a moda ou criar uma categoria específica, como “MISSING”. Porém, é importante verificar se o fato de o valor estar ausente carrega alguma informação; nesse caso, além da imputação, podemos criar uma variável indicadora informando que aquele valor estava originalmente ausente. Dependendo da implementação, também é possível não considerar aquela feature no cálculo da probabilidade para uma observação específica, mas, em um pipeline tradicional, eu trataria os valores faltantes antes de aplicar o Naive Bayes, escolhendo a estratégia de acordo com a natureza da variável e do problema.

---

### 7. Como o modelo se comporta com outliers?

O comportamento do Naive Bayes diante de outliers depende bastante da variante utilizada e da distribuição assumida para as features. 

No caso do Gaussian Naive Bayes, por exemplo, estimamos para cada feature, dentro de cada classe, parâmetros como média e variância; portanto, um outlier pode distorcer esses parâmetros, principalmente a média e o desvio-padrão, fazendo com que a distribuição estimada deixe de representar bem os dados daquela classe e alterando as probabilidades calculadas pelo modelo. 

No Multinomial ou Bernoulli Naive Bayes, o conceito de outlier é diferente, porque trabalhamos principalmente com contagens ou variáveis binárias; nesses casos, uma observação extrema pode aparecer como uma contagem muito elevada ou uma combinação rara de features e também influenciar as estimativas, mas o problema não ocorre exatamente da mesma forma que no Gaussian. 

Portanto, eu não removeria automaticamente os outliers. Primeiro investigaria se são erros de dados ou observações legítimas; se forem erros, podemos corrigi-los ou removê-los, enquanto, se forem legítimos, podemos considerar transformações, clipping, técnicas robustas ou uma distribuição mais adequada aos dados. No caso de Gaussian Naive Bayes, por exemplo, uma transformação logarítmica pode ser útil para uma variável muito assimétrica, ou podemos considerar uma abordagem cuja distribuição seja mais compatível com os dados. 

O ponto principal é que Naive Bayes não é automaticamente robusto a outliers: quando os outliers afetam a estimativa das distribuições condicionais, eles podem alterar significativamente as probabilidades e, consequentemente, as classificações. Por isso, o tratamento deve ser feito considerando a variante do Naive Bayes, a distribuição assumida e o significado dos valores extremos no contexto do problema.

---

### 8. O modelo necessita de pré processamento como padronização ou normalização?

Não necessariamente. O Naive Bayes geralmente não necessita de padronização ou normalização, porque, diferentemente de algoritmos como KNN e SVM, ele não calcula distâncias entre observações. No caso do Gaussian Naive Bayes, por exemplo, cada variável contínua é modelada por uma distribuição normal dentro de cada classe, estimando sua própria média e variância; portanto, variáveis em escalas diferentes não são, por si só, um problema. Ainda assim, o pré-processamento pode ser necessário por outros motivos, como tratamento de valores faltantes, codificação de variáveis categóricas ou transformação de variáveis quando a distribuição dos dados não é adequada à hipótese do modelo.

---

### 9. Quais suposições o modelo faz?

1. Idependência de recursos
2. Os recursos contínuos são normalmente distribuídos
3. Os recursos discretos têm distrbuições multinomiais
4. Os recursos são igualmente importantes
5. Não há dados ausentes

Já os tipos de modelos diferem pela suposição que fazem em relação à distribuição P(A/B).
Ainda, mesmo com essas suposições sendo difícies de serem verdadeiras no mundo real, o modelo funciona bem em diversas aplicações.

---

### 10. O algoritmo tende a overfitting ou underfitting?

O Naive Bayes tende a ter viés relativamente alto e variância relativamente baixa, principalmente por causa da forte hipótese de independência condicional entre as features. Essa hipótese simplifica bastante o modelo: ele não tenta aprender relações complexas ou interações entre as variáveis. Isso reduz a capacidade de se adaptar excessivamente aos dados de treinamento, fazendo com que seja menos propenso a overfitting e tenha baixa variância. Por outro lado, essa simplificação pode fazer com que o modelo não represente bem a realidade quando as features possuem relações importantes entre si, gerando maior viés e podendo levar a underfitting. Portanto, de forma geral, eu diria que o Naive Bayes está mais associado ao cenário de **alto viés e baixa variância**, embora isso dependa do conjunto de dados, da variante utilizada e das hipóteses assumidas pelo modelo.

---

### 11. O que é Complement Naive Bayes (CNB) e qual problema ele tenta resolver?

O Complement Naive Bayes (CNB) é uma variação do Naive Bayes criada principalmente para melhorar o desempenho em problemas de classificação de textos, especialmente quando as **classes são desbalanceadas**. A principal diferença é que, em vez de estimar as probabilidades das palavras usando apenas os documentos de uma determinada classe, o Complement Naive Bayes calcula essas estatísticas usando os documentos que pertencem às outras classes, ou seja, ao “complemento” daquela classe. Por exemplo, para calcular os pesos associados à classe Spam, ele considera a distribuição das palavras nos documentos que não são Spam. A ideia é reduzir o impacto que uma classe dominante pode causar nas estimativas e tornar o modelo mais robusto ao desbalanceamento. Depois de calcular essas estatísticas, o CNB utiliza uma regra de classificação baseada nesses pesos para escolher a classe. Na prática, ele é particularmente conhecido por funcionar bem com dados textuais de alta dimensionalidade e classes desbalanceadas, sendo uma alternativa ao Multinomial Naive Bayes nesses cenários.

---

### 12. Qual o custo computacional de treinamento e inferência do modelo?

O Naive Bayes possui baixo custo computacional tanto no treinamento quanto na inferência. No treinamento, o modelo percorre os dados para estimar as probabilidades das classes e das características, resultando aproximadamente em uma complexidade de O(n · d), considerando n observações e d características. Na inferência, para cada nova observação, ele calcula a probabilidade de cada uma das k classes a partir das suas d características, ficando aproximadamente em O(d · k). Além disso, o modelo armazena apenas os parâmetros estimados, não precisando guardar todo o conjunto de treinamento. Por isso, é um algoritmo bastante eficiente, inclusive para bases grandes e problemas de alta dimensionalidade, como classificação de textos.

---

### 13. Há necessidade de pré-processamento de dados?

O Naive Bayes não exige necessariamente normalização ou padronização das variáveis, porque, diferentemente de algoritmos baseados em distância, ele não é afetado diretamente pela escala das features. 

Para dados contínuos, é importante **escolher uma variante adequada**, como o Gaussian Naive Bayes, que assume uma distribuição aproximadamente normal e estima média e variância de cada variável dentro de cada classe; portanto, transformações como log ou outras técnicas podem ser úteis quando a distribuição é muito assimétrica. 

Para variáveis categóricas, é necessário **utilizar uma representação compatível com o modelo**, como contagens ou frequências no Multinomial Naive Bayes, ou indicadores binários no Bernoulli Naive Bayes. 

Em relação a valores ausentes, embora conceitualmente seja possível não considerar uma característica ausente no cálculo da probabilidade, na prática é comum **realizar imputação** antes do treinamento. 

Já a multicolinearidade não costuma ser um problema da mesma forma que em modelos como regressão linear, pois o Naive Bayes não estima coeficientes correlacionados; porém, variáveis altamente correlacionadas **violam a hipótese de independência condicional** e podem fazer com que a mesma informação seja considerada várias vezes, afetando principalmente as probabilidades estimadas. 

Portanto, o pré-processamento deve focar principalmente no tratamento de valores ausentes, na representação adequada das variáveis e na adequação das distribuições às premissas da variante escolhida, enquanto normalização não é normalmente necessária.

---

### 14. o que é estouro negativo?

Estouro negativo, ou underflow, é um problema numérico que ocorre quando um valor positivo é tão pequeno que o computador não consegue representá-lo adequadamente e acaba tratando-o como zero. No Naive Bayes, isso pode acontecer porque o modelo multiplica várias probabilidades condicionais, e, quando temos muitas features, esse produto pode se tornar extremamente pequeno. Para evitar esse problema, podemos trabalhar com log-probabilidades, transformando as multiplicações em somas. Dessa forma, evitamos que o resultado numérico fique pequeno demais e seja convertido para zero.

---

### 15. Quais parâmetros podem ajudar a melhorar o desempenho do algoritmo?

O **min_count** é comum em técnicas de construção de vocabulário e embeddings, como Word2Vec, e define a frequência mínima para uma palavra ser considerada; palavras que aparecem menos vezes são descartadas, reduzindo dimensionalidade e ruído.

Já **stemmer()** não é um parâmetro do Naive Bayes, mas uma etapa de pré-processamento que reduz palavras às suas raízes ou stems, fazendo com que termos como “conectar”, “conectado” e “conectando” possam ser tratados como relacionados
