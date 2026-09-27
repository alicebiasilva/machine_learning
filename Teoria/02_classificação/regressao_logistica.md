# Regressão logística

### 1. O que é regressão logística e por que ela é usada para classificação?

A regressão logística é um algoritmo de aprendizado supervisionado usado principalmente para problemas de classificação, especialmente quando a variável resposta tem duas classes (mas com adaptações para classes não binárias como em regressão logística multinomial).

Apesar do nome “regressão”, ela não prevê diretamente um valor contínuo como a regressão linear, ele prevê probabilidades e usando um ponto de corte define qual o limite para classificar uma classe como 0 ou 1, como por exemplo usando o ponto de corte de 50% para mesma chance. Assim, se o modelo produzir p = 0,8, podemos interpretar que, de acordo com o modelo, aquela observação tem probabilidade estimada de 80% de pertencer à classe positiva e a classificamos como 1.

---

### 2. Qual é a diferença fundamental entre regressão linear e regressão logística?

A diferença fundamental é o **tipo de resultado** que queremos modelar e **como o modelo transforma as variáveis de entrada** nesse resultado.

Na regressão linear, o target é normalmente contínuo, como salário, preço ou temperatura, e o modelo calcula diretamente um valor numérico por meio de uma combinação linear das variáveis: ŷ = β₀ + β₁X₁ + ... + βₚXₚ. 

Na regressão logística, o objetivo geralmente é classificar uma observação em uma classe, como “fraude” ou “não fraude”, e por isso queremos primeiro estimar uma probabilidade entre 0 e 1.

Ela também começa com uma combinação linear, z = β₀ + β₁X₁ + ... + βₚXₚ, mas passa esse resultado pela função sigmoid, transformando z em uma probabilidade. A partir dessa probabilidade, usamos um threshold para tomar a decisão de classificação. Isso também muda a função de perda e a forma de estimação: na regressão linear, é comum utilizar MSE e mínimos quadrados; na logística, utilizamos normalmente log-loss e máxima verossimilhança. 

Apesar dessas diferenças, ambas são modelos lineares nos parâmetros, porque em ambos os casos começamos com uma combinação linear dos coeficientes e das features.

---

### 3. Por que a regressão logística utiliza a função sigmoid? O que ela resolve?

A regressão logística utiliza a função sigmoid porque precisamos transformar a combinação linear das variáveis em um valor que possa ser interpretado como uma probabilidade. Primeiro, o modelo calcula z = β₀ + β₁X₁ + ... + βₚXₚ. Esse z pode assumir qualquer valor, de menos infinito a mais infinito, então não podemos interpretá-lo diretamente como uma probabilidade, já que uma probabilidade precisa estar entre 0 e 1. 

A sigmoid resolve exatamente esse problema: ela recebe qualquer valor de z e o transforma em um número entre 0 e 1, seguindo a fórmula p = 1/(1 + e⁻ᶻ). Além disso, ela tem uma propriedade importante: valores muito negativos de z produzem probabilidades próximas de 0, valores muito positivos produzem probabilidades próximas de 1 e, quando z = 0, a probabilidade é 0,5. Isso cria uma transição suave entre as duas classes, em vez de simplesmente produzir uma decisão abrupta de 0 ou 1. Essa característica também permite otimizar o modelo utilizando métodos baseados em gradiente e interpretar a saída como uma probabilidade estimada. 

Portanto, a ideia principal é: a combinação linear determina a evidência a favor de uma classe, e a sigmoid transforma essa evidência em uma probabilidade entre 0 e 1, que posteriormente pode ser convertida em uma classe usando um threshold.

---

### 4. Qual o racional do modelo? O que são odds e log-odds?

O racional da regressão logística é modelar a probabilidade de ocorrência de um evento, mas, em vez de tentar modelar diretamente uma probabilidade entre 0 e 1 por meio de uma equação linear, o modelo transforma essa probabilidade em odds e depois em log-odds, que podem assumir qualquer valor real.

1. As odds representam a razão entre a probabilidade de o evento acontecer e a probabilidade de ele não acontecer, ou seja,(odds = p/1-p). Por exemplo, se a probabilidade de um cliente contratar um produto é 80%, a probabilidade de não contratar é 20%, então as odds são (0,8/0,2)=4, o que significa que o evento é 4 vezes mais provável de acontecer do que de não acontecer. Porém, note que esse valor pode ir de 0 a infinito e não cresce na mesma proporção do aumento da variável, ou seja, não é uma relação linear.

2. Como as odds variam de 0 a infinito, aplicamos o logaritmo, obtendo os log-odds, também chamados de logit log(p/1-p). Essa transformação faz com que o resultado possa variar de menos infinito a mais infinito, permitindo que ele seja modelado por uma combinação linear das variáveis, como beta_0 + beta_1*x1 + ... + beta_p*xp. Ao aplicar o log, transformamos um problema multiplicativo em aditivo, que é linearmente modelável.

3. Depois, aplicamos a função sigmoide, que é a inversa do logit, para transformar esse resultado novamente em uma probabilidade entre 0 e 1.

Portanto, o fluxo conceitual é obtemos a probabilidade → calculamos os odds → aplicamos o logaritmo obtendo log-odds, que são modelados linearmente → aplicamos a sigmoide para obtermos um intervalo entre 0 e 1 → probabilidade.

---

### 5. Por que não podemos modelar as probabilidades diretamente?

Porque uma regressão linear não consegue garantir que a previsão fique entre 0 e 1, que é uma exigência básica de uma probabilidade.

O problema, portanto, não é que seja matematicamente proibido tentar modelar probabilidades diretamente. O problema é que uma função linear não possui naturalmente a restrição 0 <= p <= 1. A regressão logística resolve isso fazendo uma transformação:

Ao invés de modelar p = β0​+β1​x
Ela faz: log(p/1-p) = β0​+β1​x

Note que p não pode ser igual a valores de menos infinito a mais infinito, porque é limitado a valores entre 0 e 1. Mas log(p/1-p) pode, porque é ilimitada. Então mantemos a relação linear β0​+β1​x, apenas igualamos ela a outro conceito que possa ser linearmente modelável. 

---

### 6. Como interpretar os coeficientes de uma regressão logística? O que são log-odds e odds ratio?

Na regressão logística, os coeficientes são interpretados de forma um pouco diferente da regressão linear porque eles atuam sobre os log-odds, e não diretamente sobre a probabilidade.

Como modelamos log(p/1-p) = β0​+β1​x, uma mudança em β indica quanto o log-odds varia quando x aumenta em uma unidade, mantendo as demais variáveis constantes. Se for positivo, aumenta o log-odds e, consequentemente, aumenta a probabilidade do evento. Se for negativo ocorre o contrário.

Porém, note que usar a escala log-odds não é intuitivo e fácil de interpretar. Por isso, frequentemente exponenciamos o coeficiente: e^βj​. O resultado é chamado de odds ratio (OR). O odds ratio mostra por quanto as odds são multiplicadas quando a variável aumenta uma unidade. É simplesmente uma forma de comparar dois odds (“Quantas vezes o odds do evento é maior ou menor em uma situação em comparação com outra?”). É mais fácil de interpretar porque ele analisa o odds antes e depois de uma mudança em uma variável.

---

### 7. Como é feita a estimação dos parâmetros na regressão logística? Por que usamos máxima verossimilhança em vez de mínimos quadrados?

Na regressão logística, os parâmetros são estimados normalmente por máxima verossimilhança, porque o objetivo do modelo é estimar a probabilidade de cada observação pertencer a uma classe, e para isso usamos a função sigmóide que não é uma função quadrática, então não tem uma parábola ou uma forma simples de minimizar.

Primeiro, para cada observação, o modelo calcula z = β₀ + β₁X₁ + ... + βₚXₚ e transforma esse valor em uma probabilidade p usando a sigmoid. Como o target é binário, podemos pensar que, para cada observação, o modelo diz “acredito que existe uma probabilidade p de essa pessoa pertencer à classe 1”. A máxima verossimilhança procura os valores dos coeficientes β que tornam os resultados que realmente observamos o mais prováveis possível. Para isso, construímos a verossimilhança conjunta das observações e normalmente trabalhamos com o log da verossimilhança, que transforma produtos em somas e facilita a otimização; maximizar essa log-verossimilhança é equivalente a minimizar a conhecida log-loss ou cross-entropy. 

O passo a passo é:

1. Inicializamos os betas;
2. Calculamos os log-odds;
3. Aplicamos a sigmoide;
4. Calculamos a verossimilhança;
5. Multiplicamos todas as probabilidades (pressupõe independência);
6. Como multiplicar muitas probabilidades gera um número muito pequeno, aplicamos o log;
7. Mas em Machine Learning normalmente formulamos o problema como minimização de uma função de custo. Então multiplicamos por -1. E isso é justamente o Log-Loss, também chamado de Binary Cross-Entropy;
8. Ajustamos os betas para minimizar a log-likelihood, usando o gradiente da função log-likelihood. Queremos maximizar isso --> beta_novo = beta_atual + alfa * gradiente;
9. Repetimos até convergir.

---

### 8. O que é a função de custo/log-loss da regressão logística e por que ela é adequada para classificação?

A log-loss, também chamada de cross-entropy loss, é a função de custo normalmente utilizada na regressão logística para medir o quão boas são as probabilidades produzidas pelo modelo. Como a regressão logística prevê uma probabilidade p de uma observação pertencer à classe 1, queremos não apenas acertar a classe, mas também **avaliar o quanto o modelo estava confiante nessa previsão**. Em outras palavras, a log-loss recompensa o modelo quando ele atribui alta probabilidade ao resultado que realmente ocorreu e penaliza fortemente quando ele atribui baixa probabilidade ao resultado observado, tornando-a adequada para treinar um modelo que precisa produzir probabilidades para classes.

Para uma observação, a log-loss é −[y log(p) + (1−y) log(1−p)], em que y é o valor real, 0 ou 1, e p é a probabilidade prevista para a classe 1. Se y = 1, a fórmula vira −log(p): se o modelo prevê 0,9, a penalização é pequena, mas se prevê 0,01, a penalização é enorme, porque o modelo atribuiu uma probabilidade quase zero a um evento que realmente aconteceu. Da mesma forma, quando y = 0, a penalização depende de −log(1−p), então prever 0,01 é uma boa previsão, enquanto prever 0,99 é fortemente penalizado. Isso é adequado para classificação porque a função avalia diretamente a qualidade das probabilidades previstas e penaliza de maneira muito forte previsões erradas e excessivamente confiantes. 

Além disso, a log-loss está diretamente relacionada à máxima verossimilhança da distribuição Bernoulli: minimizar a log-loss é matematicamente equivalente a maximizar a probabilidade de observar os dados que realmente ocorreram segundo as probabilidades estimadas pelo modelo. Por isso, ela não foi escolhida arbitrariamente; ela é naturalmente derivada do pressuposto probabilístico da regressão logística. 

---

### 9. Como funciona o threshold de classificação? O que muda se eu passar de um threshold de 0,5 para 0,8?

O threshold é o ponto de corte que usamos para transformar a probabilidade produzida pela regressão logística em uma classe. A regressão logística não diz diretamente “é classe 0” ou “é classe 1”; ela produz algo como “existe 70% de probabilidade de ser classe 1”. Se o threshold for 0,5, por exemplo, classificamos como classe 1 todas as observações com probabilidade maior ou igual a 50% e como classe 0 as demais. Se aumentarmos o threshold para 0,8, ficamos muito mais exigentes para classificar alguém como classe 1: uma previsão de 70%, que antes seria classe 1, agora passa a ser classe 0. 

Como consequência, normalmente teremos menos previsões positivas, o que tende a reduzir os falsos positivos e aumentar a especificidade, mas pode aumentar os falsos negativos e reduzir o recall ou sensibilidade. Imagine um modelo para detectar fraude: com threshold 0,5, uma transação com 60% de probabilidade de ser fraude seria bloqueada; com threshold 0,8, essa mesma transação seria considerada normal, porque o modelo não está suficientemente confiante.

A escolha do threshold deve depender do custo relativo dos erros: se perder um caso positivo for muito mais grave do que gerar um falso alarme, podemos escolher um threshold mais baixo para aumentar o recall; se falsos positivos forem muito custosos, podemos aumentar o threshold. 

---

### 10. Como lidar com classes desbalanceadas em uma regressão logística?

Em uma regressão logística com classes desbalanceadas, o principal problema é que o modelo pode aprender a favorecer a classe majoritária, justamente por trabalhar com probabilidades. 

Por isso, eu começaria avaliando métricas mais adequadas ao objetivo, como precision, recall, F1-score, PR-AUC e, dependendo do caso, ROC-AUC. 

No treinamento da regressão logística, uma abordagem comum é utilizar class weights, atribuindo um peso maior aos erros cometidos na classe minoritária; dessa forma, o modelo passa a considerar mais importante acertar os exemplos de fraude, por exemplo. 

Outra possibilidade é fazer oversampling da classe minoritária ou undersampling da classe majoritária, embora essas técnicas devam ser aplicadas com cuidado e exclusivamente no conjunto de treinamento para evitar data leakage. 

Também podemos ajustar o threshold de classificação: em um problema de fraude, por exemplo, podemos reduzir o threshold de 0,5 para aumentar o recall e capturar mais fraudes, aceitando potencialmente mais falsos positivos. 

---

### 11. Quais são as principais premissas do modelo?

1. Variável resposta binária — na regressão logística binária, o target deve representar duas classes, geralmente 0 e 1.

2. Independência das observações — as observações devem ser independentes entre si. Por exemplo, se temos várias linhas correspondentes ao mesmo cliente, essa independência pode ser violada.

3. Relação linear entre as variáveis explicativas e o log-odds — essa é uma das premissas mais importantes. A regressão logística não exige que a variável explicativa tenha relação linear com a probabilidade. O que deve ser aproximadamente linear é o log-odds. Por exemplo, se aumentar \(X\) em uma unidade sempre produz o mesmo efeito sobre o log-odds, essa relação é linear nessa escala.

4. Ausência de multicolinearidade severa — as variáveis explicativas não devem ser excessivamente correlacionadas entre si. Multicolinearidade pode tornar os coeficientes instáveis e dificultar sua interpretação.

5. Ausência de separação perfeita ou quase perfeita — não deve existir uma combinação das variáveis que consiga separar perfeitamente as classes. Quando isso acontece, os coeficientes podem crescer indefinidamente e a estimação fica problemática.

6. Tamanho de amostra adequado — é necessário ter quantidade suficiente de observações, principalmente da classe positiva, para estimar os parâmetros de forma estável.

Uma coisa que não é uma premissa é a normalidade das variáveis explicativas. A regressão logística não exige que os X sejam normalmente distribuídos.

Também não é necessário que haja homocedasticidade, outra premissa comum da regressão linear.

---

### 12. Quais as vantagens e desvantagens do modelo?

A regressão logística tem como principal vantagem ser um modelo simples, rápido e relativamente interpretável. Ela produz probabilidades, além da classificação final, e seus coeficientes podem ser interpretados por meio dos odds ratio, permitindo entender como cada variável está associada à ocorrência do evento. Também funciona bem quando a relação entre as variáveis explicativas e o log-odds é aproximadamente linear, possui baixo custo computacional e permite o uso de regularização, como L1 e L2, para controlar overfitting e lidar com muitas variáveis.

Por outro lado, uma de suas principais limitações é justamente essa hipótese de linearidade no log-odds: quando a relação entre as variáveis e o target é fortemente não linear, o modelo pode não capturar adequadamente o padrão dos dados. Além disso, pode ser sensível à multicolinearidade e a problemas como separação perfeita, e seu desempenho pode ser prejudicado quando existem relações ou interações complexas entre as variáveis que não foram explicitamente incluídas no modelo. Em problemas com padrões muito não lineares, modelos como árvores, Random Forest ou Gradient Boosting podem capturar estruturas mais complexas sem precisar especificá-las manualmente.

---

### 13. Como Ridge e Lasso podem ser utilizados em regressão logística?

Podem ser utilizados na regressão logística como técnicas de regularização, adicionando uma penalização à função de custo para evitar que os coeficientes fiquem excessivamente grandes e para reduzir o risco de overfitting. 

Na regressão logística tradicional, buscamos os coeficientes que minimizam a log-loss; com Ridge, adicionamos uma penalização L2, proporcional à soma dos quadrados dos coeficientes, ficando conceitualmente Log-loss + λΣβ², enquanto no Lasso adicionamos uma penalização L1, proporcional à soma dos valores absolutos dos coeficientes, Log-loss + λΣ|β|. 

O parâmetro λ controla a intensidade da regularização: quanto maior ele for, maior será a pressão para reduzir os coeficientes. A diferença mais importante é o comportamento dos coeficientes: Ridge tende a diminuir todos os coeficientes em direção a zero, mas normalmente não os torna exatamente zero, enquanto Lasso pode levar alguns coeficientes exatamente a zero, funcionando também como uma forma de seleção de variáveis. Isso pode ser especialmente útil quando temos muitas features ou features correlacionadas, embora o Lasso possa ter um comportamento instável na escolha entre variáveis altamente correlacionadas. 

Na prática, o λ é tratado como hiperparâmetro, sendo escolhido normalmente por validação cruzada, enquanto os coeficientes são aprendidos durante o treinamento. Portanto, Ridge e Lasso não mudam a ideia fundamental da regressão logística — continuamos estimando probabilidades com a sigmoid e utilizando log-loss —, mas modificam a função objetivo para controlar a complexidade do modelo e melhorar sua capacidade de generalização.

---

### 14. A regressão logística é um modelo linear ou não linear? Explique.

A regressão logística é considerada um modelo linear, mas existe uma sutileza importante: ela é linear em relação aos parâmetros e aos log-odds, **e não diretamente em relação à probabilidade**. 

O modelo primeiro calcula uma combinação linear das features, z = β₀ + β₁X₁ + ... + βₚXₚ; portanto, se alterarmos uma feature em uma unidade, seu efeito sobre z é determinado pelo seu coeficiente, mantendo as demais constantes. 

Depois aplicamos a função sigmoid para transformar esse z em uma probabilidade entre 0 e 1. Como a **sigmoid é uma função não linear**, a relação entre uma feature e a probabilidade não é linear: por exemplo, aumentar uma variável pode alterar pouco a probabilidade quando ela já está próxima de 0 ou 1, e alterar mais quando está próxima de 0,5. 

Entretanto, se voltarmos para a escala de log-odds, temos log(p/(1-p)) = β₀ + β₁X₁ + ... + βₚXₚ, que é uma relação linear. É por isso que chamamos a regressão logística de modelo linear de classificação: **a fronteira de decisão entre as classes é linear no espaço das features**. 

Em um problema com duas variáveis, por exemplo, o modelo pode separar as classes por uma reta; com três variáveis, por um plano; e em dimensões maiores, por um hiperplano. Portanto, apesar de utilizar uma transformação não linear, a regressão logística continua sendo um modelo linear, porque sua estrutura fundamental é uma combinação linear das features e dos parâmetros, e sua fronteira de decisão também é linear.

---

### 15. Como o algoritmo lida com missings e outliers? 

Missings e outliers não são tratados automaticamente pelo algoritmo de uma forma que garanta um bom resultado, então precisamos decidir como lidar com eles antes ou durante o pipeline de modelagem. 

Para valores ausentes, uma abordagem comum é fazer imputação, por exemplo usando mediana para variáveis numéricas ou moda para categóricas, mas a escolha depende do mecanismo e do contexto do missing; em alguns casos, podemos adicionar uma variável indicadora informando que aquele valor estava ausente, porque a ausência em si pode carregar informação. Também podemos remover observações ou utilizar modelos que tratem missing de outra maneira, mas devemos evitar simplesmente substituir tudo por zero sem entender o significado desse zero. 

Para outliers, a situação é diferente: a regressão logística não assume normalidade dos preditores, mas observações extremas podem ter grande **influência sobre os coeficientes**, principalmente quando possuem alta alavancagem ou estão associadas à variável target de forma muito diferente do restante dos dados. Por isso, eu primeiro investigaria se o outlier representa um erro de qualidade dos dados ou uma observação legítima; se for erro, podemos corrigi-lo ou removê-lo, enquanto, se for legítimo, talvez seja melhor mantê-lo e considerar transformações, tratamento de valores extremos, regularização ou modelos mais robustos. 

---

### 16. É necessário aplicar pré processamento de dados como padronização e normalização?

Não é obrigatoriamente necessário, mas em muitos casos é altamente recomendado, principalmente quando usamos regressão logística com regularização ou quando queremos uma otimização mais estável. 

A regressão logística em si não exige que as variáveis estejam na mesma escala: se uma variável está entre 0 e 1 e outra entre 0 e 100.000, o modelo ainda pode, em princípio, aprender os coeficientes adequados. O problema aparece principalmente na otimização: como métodos como Gradiente Descendente usam o gradiente para atualizar os parâmetros, features em escalas muito diferentes podem gerar uma função de custo mal condicionada e tornar a convergência mais lenta ou instável. 

Além disso, quando usamos Ridge ou Lasso, o scaling se torna ainda mais importante, porque a penalização é aplicada aos coeficientes; sem padronização, uma variável pode ser penalizada de maneira efetivamente diferente de outra apenas por estar em uma escala diferente. Para variáveis categóricas transformadas em dummies, normalmente não há necessidade de padronização, enquanto para variáveis numéricas podemos utilizar, por exemplo, standardization, transformando cada variável para média 0 e desvio-padrão 1. 

---

### 17. O modelo tende a overfitting ou underfitting?

A regressão logística tende a apresentar maior viés e menor variância, principalmente quando comparada a modelos mais flexíveis, porque assume uma relação linear entre as variáveis e o log-odds da classe. Essa hipótese restringe a complexidade do modelo: por um lado, reduz a sensibilidade a pequenas variações nos dados e, consequentemente, o risco de overfitting; por outro, pode levar a underfitting quando a relação real entre as variáveis e o resultado é muito complexa ou não linear. Portanto, de forma geral, podemos associar a regressão logística a **viés relativamente alto e variância relativamente baixa**, mas isso não significa que ela nunca sofra overfitting — com muitas variáveis, features muito correlacionadas ou pouca regularização, por exemplo, isso pode acontecer. A regularização L1 ou L2 pode ser utilizada para controlar a complexidade e reduzir a variância.

---

### 18. Como o modelo lida com classes não binárias?

Quando a variável target possui mais de duas classes, a regressão logística pode ser adaptada para um problema multiclasse. Uma abordagem é o One-vs-Rest (OvR), em que treinamos um classificador binário para cada classe, tratando aquela classe como positiva e todas as outras como negativas. Para uma previsão, cada modelo produz uma probabilidade e podemos selecionar a classe associada à maior probabilidade. 

Outra abordagem é a regressão logística multinomial, que utiliza a função softmax para calcular simultaneamente a probabilidade de cada classe, fazendo com que as probabilidades sejam não negativas e somem 1. Assim, diferentemente do caso binário, em que utilizamos a sigmoide para obter a probabilidade de uma classe, no problema multiclasse podemos utilizar vários classificadores binários ou um modelo multinomial com softmax.

---

### 19. O que é o parâmetro "C"?

O parâmetro (C), utilizado por exemplo no scikit-learn, controla a intensidade da regularização da regressão logística. Ele é inversamente relacionado à força da regularização: valores menores de (C) correspondem a uma regularização mais forte, fazendo com que os coeficientes sejam mais penalizados e o modelo fique mais simples; valores maiores de (C) correspondem a uma regularização mais fraca, permitindo que os coeficientes assumam valores maiores e que o modelo se ajuste mais aos dados de treinamento. Portanto, (C) controla o trade-off entre ajuste aos dados e complexidade do modelo e normalmente é escolhido por validação cruzada.

* C pequeno → regularização forte → os coeficientes são mais penalizados → modelo mais simples.
* C grande → regularização fraca → os coeficientes podem assumir valores maiores → modelo mais próximo do ajuste sem regularização.

--- 

### 20. Qual a diferença entre probabilidade e máxima verossimilhança?

A diferença principal é que probabilidade e verossimilhança usam a mesma expressão matemática, mas fazem perguntas diferentes.

* Probabilidade: os parâmetros são conhecidos e queremos saber a chance de observar determinado resultado.
* Já na máxima verossimilhança, fazemos o contrário: temos os dados observados e queremos descobrir qual valor do parâmetro torna esses dados mais plausíveis.