# Regressão logística

### 1. O que é regressão logística e por que ela é usada para classificação?

A regressão logística é um modelo estatístico, paramétrico e usado em problemas de classificação supervisionados quando queremos prever a probabilidade de uma observação pertencer a uma determinada classe, sendo a classe binária na forma tradicional do modelo, mas com adaptações para classes não binárias como em regressão logística multinomial.

Apesar do nome “regressão”, ela não prevê diretamente um valor contínuo como a regressão linear; primeiro ela calcula uma combinação linear das variáveis de entrada, por exemplo z = β₀ + β₁X₁ + ... + βₚXₚ, e depois aplica a função logística (sigmoid), que transforma esse valor em uma probabilidade entre 0 e 1. Assim, se o modelo produzir p = 0,8, podemos interpretar que, de acordo com o modelo, aquela observação tem probabilidade estimada de 80% de pertencer à classe positiva. 

Para transformar essa probabilidade em uma classe, definimos um threshold, que frequentemente começa em 0,5: probabilidades acima desse valor são classificadas como classe 1 e abaixo como classe 0, embora esse threshold possa ser ajustado de acordo com a necessidade e comportamento dos dados.

A regressão logística é particularmente útil porque, além de produzir probabilidades, seus coeficientes podem ser interpretados em termos de odds e log-odds, permitindo entender como cada variável está associada à probabilidade do evento.

---

### 2. Qual é a diferença fundamental entre regressão linear e regressão logística?

A diferença fundamental é o tipo de resultado que queremos modelar e como o modelo transforma as variáveis de entrada nesse resultado. Na regressão linear, o target é normalmente contínuo, como salário, preço ou temperatura, e o modelo calcula diretamente um valor numérico por meio de uma combinação linear das variáveis: ŷ = β₀ + β₁X₁ + ... + βₚXₚ. Na regressão logística, o objetivo geralmente é classificar uma observação em uma classe, como “fraude” ou “não fraude”, e por isso queremos primeiro estimar uma probabilidade entre 0 e 1. 

Ela também começa com uma combinação linear, z = β₀ + β₁X₁ + ... + βₚXₚ, mas passa esse resultado pela função sigmoid, transformando z em uma probabilidade. A partir dessa probabilidade, usamos um threshold para tomar a decisão de classificação. Isso também muda a função de perda e a forma de estimação: na regressão linear, é comum utilizar MSE e mínimos quadrados; na logística, utilizamos normalmente log-loss e máxima verossimilhança. 

Portanto, uma forma simples de resumir é: a regressão linear estima diretamente um valor contínuo, enquanto a regressão logística estima a probabilidade de uma classe e usa essa probabilidade para realizar a classificação. Apesar disso, ambas são modelos lineares nos parâmetros, porque em ambos os casos começamos com uma combinação linear dos coeficientes e das features.

---

### 3. Por que a regressão logística utiliza a função sigmoid? O que ela resolve?

A regressão logística utiliza a função sigmoid porque precisamos transformar a combinação linear das variáveis em um valor que possa ser interpretado como uma probabilidade. Primeiro, o modelo calcula z = β₀ + β₁X₁ + ... + βₚXₚ. Esse z pode assumir qualquer valor, de menos infinito a mais infinito, então não podemos interpretá-lo diretamente como uma probabilidade, porque uma probabilidade precisa estar entre 0 e 1. 

A sigmoid resolve exatamente esse problema: ela recebe qualquer valor de z e o transforma em um número entre 0 e 1, seguindo a fórmula p = 1/(1 + e⁻ᶻ). Além disso, ela tem uma propriedade importante: valores muito negativos de z produzem probabilidades próximas de 0, valores muito positivos produzem probabilidades próximas de 1 e, quando z = 0, a probabilidade é 0,5. Isso cria uma transição suave entre as duas classes, em vez de simplesmente produzir uma decisão abrupta de 0 ou 1. Essa característica também permite otimizar o modelo utilizando métodos baseados em gradiente e interpretar a saída como uma probabilidade estimada. Portanto, a ideia principal é: a combinação linear determina a evidência a favor de uma classe, e a sigmoid transforma essa evidência em uma probabilidade entre 0 e 1, que posteriormente pode ser convertida em uma classe usando um threshold.

---

### 4. Como interpretar os coeficientes de uma regressão logística? O que são log-odds e odds ratio?

Na regressão logística, o coeficiente de uma variável representa uma mudança nos log-odds do evento, e para entender isso precisamos separar três conceitos: probabilidade, odds e odds ratio. Esses conceitos são formas diferentes de representar a mesma informação sobre a chance de um evento acontecer, mas cada uma é útil para uma finalidade diferente:

* A probabilidade é simplesmente a chance de o evento acontecer; por exemplo, se um cliente tem 20% de probabilidade de cancelar, temos p = 0,20.

* O odds transforma essa probabilidade em uma relação entre a chance de o evento acontecer e a chance de ele não acontecer: odds = p/(1-p), então, nesse caso, 0,20/0,80 = 0,25. O ponto é que a regressão logística não modela diretamente essa probabilidade, porque ela é sempre positiva e em uma modelagem podem existir variáveis que impactem inversamente a variável resposta, e porque a relação entre as variáveis e a probabilidade não é naturalmente linear. Para mitigar esse problema, o algoritmo modela o logaritmo do odds, chamado de log-odds ou logit, estabelecendo uma relação linear entre as variáveis e o resultado! 

* A regressão logística modela o logaritmo desse odds, chamado de log-odds ou logit: log(p/(1-p)) = β₀ + β₁X₁ + ... + βₚXₚ. Isso permite que os coeficientes β sejam interpretados de forma direta: um aumento de uma unidade em (X_j) aumenta ou reduz o log-odds em βj, mantendo as demais variáveis constantes.

* Como o log-odds é menos intuitivo, podemos transformar o coeficiente em odds ratio, calculando e^βj. Assim, se e^βj=1.5, por exemplo, um aumento de uma unidade em (Xj) multiplica as odds do evento por 1.5, ou seja, aumenta as odds em 50%, mantendo as demais variáveis constantes. 

Você pode pensar na saída da regressão logística em três níveis diferentes: o modelo calcula o log-odds, que pode ser convertido em odds e, finalmente, em probabilidade. Na prática, para interpretar o resultado para uma observação, normalmente olhamos a probabilidade prevista: por exemplo, p=0,8 significa que o modelo estima 80% de probabilidade de ocorrência do evento. Já para interpretar as variáveis, olhamos os coeficientes: um β >0 indica que o aumento da variável aumenta o log-odds do evento, enquanto β<0 indica redução. Para tornar essa interpretação mais intuitiva, calculamos e^β, o odds ratio: se e^β=2, um aumento de uma unidade na variável multiplica as odds do evento por 2; se e^β=0,5, as odds são reduzidas pela metade. Portanto, probabilidade é usada para interpretar a previsão de cada observação; coeficientes e odds ratio são usados para interpretar o efeito das variáveis.

---

### 5. Como a regressão logística transforma os coeficientes e features em uma probabilidade?

A regressão logística transforma as features em uma probabilidade em duas etapas. Primeiro, ela calcula uma combinação linear das features usando os coeficientes aprendidos pelo modelo: z = β₀ + β₁X₁ + β₂X₂ + ... + βₚXₚ. Esse z é um score que pode assumir qualquer valor, positivo ou negativo, e representa os log-odds do evento. Por exemplo, imagine um modelo que prevê se um cliente vai cancelar um serviço e que, para determinado cliente, a combinação das características resulta em z = 2. Esse valor ainda não é uma probabilidade. Então aplicamos a função sigmoid, p = 1/(1 + e⁻ᶻ), que transforma qualquer valor de z em um número entre 0 e 1. 

Nesse caso, com z = 2, a probabilidade seria aproximadamente 88%. Se z = 0, a probabilidade é 50%; se z for negativo, a probabilidade fica abaixo de 50%; e quanto mais positivo for z, mais a probabilidade se aproxima de 100%. 

Portanto, os coeficientes e as features primeiro determinam o score z, e a sigmoid transforma esse score em probabilidade. É importante perceber que a sigmoid não decide a classe: ela apenas produz a probabilidade. A decisão, como classificar o cliente como “vai cancelar” ou “não vai cancelar”, acontece depois, quando aplicamos um threshold, que pode ser 0,5 ou outro valor escolhido de acordo com o objetivo do problema.

---

### 6. Como é feita a estimação dos parâmetros na regressão logística? Por que usamos máxima verossimilhança em vez de mínimos quadrados?

Na regressão logística, os parâmetros são estimados normalmente por máxima verossimilhança, porque o objetivo do modelo é estimar a probabilidade de cada observação pertencer a uma classe. Primeiro, para cada observação, o modelo calcula z = β₀ + β₁X₁ + ... + βₚXₚ e transforma esse valor em uma probabilidade p usando a sigmoid. Como o target é binário, podemos pensar que, para cada observação, o modelo diz “acredito que existe uma probabilidade p de essa pessoa pertencer à classe 1”. A máxima verossimilhança procura os valores dos coeficientes β que tornam os resultados que realmente observamos o mais prováveis possível. Para isso, construímos a verossimilhança conjunta das observações e normalmente trabalhamos com o log da verossimilhança, que transforma produtos em somas e facilita a otimização; maximizar essa log-verossimilhança é equivalente a minimizar a conhecida log-loss ou cross-entropy. 

A diferença para a regressão linear é que, nela, assumimos tipicamente que o target contínuo pode ser modelado como uma combinação linear mais um erro aproximadamente normal, e os mínimos quadrados surgem naturalmente desse pressuposto. Na regressão logística, o target é binário e modelamos uma variável aleatória Bernoulli, então a função de verossimilhança da distribuição Bernoulli é mais adequada.

Na prática, como a solução não possui uma fórmula fechada simples como o OLS, usamos algoritmos iterativos de otimização, como Gradiente Descendente, Newton-Raphson ou métodos quasi-Newton, para encontrar os coeficientes que maximizam a verossimilhança. Portanto, a ideia principal é: a regressão logística escolhe os coeficientes que tornam os rótulos observados mais prováveis segundo o modelo; maximizar a verossimilhança da Bernoulli é equivalente a minimizar a log-loss.

---

### 7. O que é a função de custo/log-loss da regressão logística e por que ela é adequada para classificação?

A log-loss, também chamada de cross-entropy loss, é a função de custo normalmente utilizada na regressão logística para medir o quão boas são as probabilidades produzidas pelo modelo. Como a regressão logística prevê uma probabilidade p de uma observação pertencer à classe 1, queremos não apenas acertar a classe, mas também **avaliar o quanto o modelo estava confiante nessa previsão**. Em outras palavras, a log-loss recompensa o modelo quando ele atribui alta probabilidade ao resultado que realmente ocorreu e penaliza fortemente quando ele atribui baixa probabilidade ao resultado observado, tornando-a adequada para treinar um modelo que precisa produzir probabilidades para classes.

Para uma observação, a log-loss é −[y log(p) + (1−y) log(1−p)], em que y é o valor real, 0 ou 1, e p é a probabilidade prevista para a classe 1. Se y = 1, a fórmula vira −log(p): se o modelo prevê 0,9, a penalização é pequena, mas se prevê 0,01, a penalização é enorme, porque o modelo atribuiu uma probabilidade quase zero a um evento que realmente aconteceu. Da mesma forma, quando y = 0, a penalização depende de −log(1−p), então prever 0,01 é uma boa previsão, enquanto prever 0,99 é fortemente penalizado. Isso é adequado para classificação porque a função avalia diretamente a qualidade das probabilidades previstas e penaliza de maneira muito forte previsões erradas e excessivamente confiantes. 

Além disso, a log-loss está diretamente relacionada à máxima verossimilhança da distribuição Bernoulli: minimizar a log-loss é matematicamente equivalente a maximizar a probabilidade de observar os dados que realmente ocorreram segundo as probabilidades estimadas pelo modelo. Por isso, ela não foi escolhida arbitrariamente; ela é naturalmente derivada do pressuposto probabilístico da regressão logística. 

---

### 8. Como funciona o threshold de classificação? O que muda se eu passar de um threshold de 0,5 para 0,8?

O threshold é o ponto de corte que usamos para transformar a probabilidade produzida pela regressão logística em uma classe. A regressão logística não diz diretamente “é classe 0” ou “é classe 1”; ela produz algo como “existe 70% de probabilidade de ser classe 1”. Se o threshold for 0,5, por exemplo, classificamos como classe 1 todas as observações com probabilidade maior ou igual a 50% e como classe 0 as demais. Se aumentarmos o threshold para 0,8, ficamos muito mais exigentes para classificar alguém como classe 1: uma previsão de 70%, que antes seria classe 1, agora passa a ser classe 0. 

Como consequência, normalmente teremos menos previsões positivas, o que tende a reduzir os falsos positivos e aumentar a especificidade, mas pode aumentar os falsos negativos e reduzir o recall ou sensibilidade. Imagine um modelo para detectar fraude: com threshold 0,5, uma transação com 60% de probabilidade de ser fraude seria bloqueada; com threshold 0,8, essa mesma transação seria considerada normal, porque o modelo não está suficientemente confiante.

A escolha do threshold deve depender do custo relativo dos erros: se perder um caso positivo for muito mais grave do que gerar um falso alarme, podemos escolher um threshold mais baixo para aumentar o recall; se falsos positivos forem muito custosos, podemos aumentar o threshold. 

---

### 9. Como lidar com classes desbalanceadas em uma regressão logística?

Quando temos classes desbalanceadas em uma regressão logística, significa que uma classe aparece muito mais vezes do que a outra, por exemplo, 99% das transações são legítimas e apenas 1% são fraude. O primeiro ponto é entender que simplesmente olhar para a acurácia pode ser enganoso: um modelo que classifica todas as transações como legítimas teria 99% de acurácia, mas seria completamente inútil para detectar fraudes. 

Por isso, eu começaria avaliando métricas mais adequadas ao objetivo, como precision, recall, F1-score, PR-AUC e, dependendo do caso, ROC-AUC. No treinamento da regressão logística, uma abordagem comum é utilizar class weights, atribuindo um peso maior aos erros cometidos na classe minoritária; dessa forma, o modelo passa a considerar mais importante acertar os exemplos de fraude, por exemplo. 

Outra possibilidade é fazer oversampling da classe minoritária ou undersampling da classe majoritária, embora essas técnicas devam ser aplicadas com cuidado e exclusivamente no conjunto de treinamento para evitar data leakage. 

Também podemos ajustar o threshold de classificação: em um problema de fraude, por exemplo, podemos reduzir o threshold de 0,5 para aumentar o recall e capturar mais fraudes, aceitando potencialmente mais falsos positivos. 

---

### 10. Quais são as principais premissas e limitações da regressão logística?

A primeira é que existe uma relação linear entre os preditores e o log-odds do evento: isso é importante porque a regressão logística assume que log(p/(1-p)) = β₀ + β₁X₁ + ... + βₚXₚ, então a relação precisa ser aproximadamente linear nessa escala, mesmo que a relação entre uma feature e a probabilidade não pareça linear. 

Também assumimos independência entre as observações, porque a estimação da verossimilhança pressupõe que as observações fornecem informações independentes; dados temporais, medidas repetidas do mesmo indivíduo ou observações agrupadas podem violar essa premissa. 

Outra questão importante é a ausência de multicolinearidade severa entre os preditores, porque quando duas ou mais variáveis carregam praticamente a mesma informação, os coeficientes podem ficar instáveis e difíceis de interpretar, embora o modelo ainda possa produzir boas previsões. 

Também precisamos tomar cuidado com separação perfeita ou quase perfeita, que acontece quando uma combinação das features consegue separar completamente as classes; nesse caso, os coeficientes podem crescer indefinidamente e a estimação dos parâmetros se torna problemática. 

Diferentemente da regressão linear, não exigimos normalidade dos resíduos nem homocedasticidade, porque o target é binário e segue uma distribuição Bernoulli, então esses conceitos não se aplicam da mesma maneira. 

Entre as limitações, a principal é que a regressão logística possui uma estrutura relativamente simples: se a relação entre as variáveis e o log-odds for muito complexa e não fizermos transformações ou criarmos interações, o modelo pode sofrer underfitting. Além disso, ela pode ser sensível a outliers e a features mal especificadas, e seus coeficientes podem ser difíceis de interpretar quando existem muitas interações ou transformações. 

---

### 11. Como Ridge e Lasso podem ser utilizados em regressão logística?

Podem ser utilizados na regressão logística como técnicas de regularização, adicionando uma penalização à função de custo para evitar que os coeficientes fiquem excessivamente grandes e para reduzir o risco de overfitting. 

Na regressão logística tradicional, buscamos os coeficientes que minimizam a log-loss; com Ridge, adicionamos uma penalização L2, proporcional à soma dos quadrados dos coeficientes, ficando conceitualmente Log-loss + λΣβ², enquanto no Lasso adicionamos uma penalização L1, proporcional à soma dos valores absolutos dos coeficientes, Log-loss + λΣ|β|. 

O parâmetro λ controla a intensidade da regularização: quanto maior ele for, maior será a pressão para reduzir os coeficientes. A diferença mais importante é o comportamento dos coeficientes: Ridge tende a diminuir todos os coeficientes em direção a zero, mas normalmente não os torna exatamente zero, enquanto Lasso pode levar alguns coeficientes exatamente a zero, funcionando também como uma forma de seleção de variáveis. Isso pode ser especialmente útil quando temos muitas features ou features correlacionadas, embora o Lasso possa ter um comportamento instável na escolha entre variáveis altamente correlacionadas. 

Na prática, o λ é tratado como hiperparâmetro, sendo escolhido normalmente por validação cruzada, enquanto os coeficientes são aprendidos durante o treinamento. Portanto, Ridge e Lasso não mudam a ideia fundamental da regressão logística — continuamos estimando probabilidades com a sigmoid e utilizando log-loss —, mas modificam a função objetivo para controlar a complexidade do modelo e melhorar sua capacidade de generalização.

---

### 12. A regressão logística é um modelo linear ou não linear? Explique.

A regressão logística é considerada um modelo linear, mas existe uma sutileza importante: ela é linear em relação aos parâmetros e aos log-odds, **e não diretamente em relação à probabilidade**. 

O modelo primeiro calcula uma combinação linear das features, z = β₀ + β₁X₁ + ... + βₚXₚ; portanto, se alterarmos uma feature em uma unidade, seu efeito sobre z é determinado pelo seu coeficiente, mantendo as demais constantes. 

Depois aplicamos a função sigmoid para transformar esse z em uma probabilidade entre 0 e 1. Como a **sigmoid é uma função não linear**, a relação entre uma feature e a probabilidade não é linear: por exemplo, aumentar uma variável pode alterar pouco a probabilidade quando ela já está próxima de 0 ou 1, e alterar mais quando está próxima de 0,5. 

Entretanto, se voltarmos para a escala de log-odds, temos log(p/(1-p)) = β₀ + β₁X₁ + ... + βₚXₚ, que é uma relação linear. É por isso que chamamos a regressão logística de modelo linear de classificação: **a fronteira de decisão entre as classes é linear no espaço das features**. 

Em um problema com duas variáveis, por exemplo, o modelo pode separar as classes por uma reta; com três variáveis, por um plano; e em dimensões maiores, por um hiperplano. Portanto, apesar de utilizar uma transformação não linear, a regressão logística continua sendo um modelo linear, porque sua estrutura fundamental é uma combinação linear das features e dos parâmetros, e sua fronteira de decisão também é linear.

---

### 13. Como o algoritmo lida com missings e outliers? 

Missings e outliers não são tratados automaticamente pelo algoritmo de uma forma que garanta um bom resultado, então precisamos decidir como lidar com eles antes ou durante o pipeline de modelagem. 

Para valores ausentes, uma abordagem comum é fazer imputação, por exemplo usando mediana para variáveis numéricas ou moda para categóricas, mas a escolha depende do mecanismo e do contexto do missing; em alguns casos, podemos adicionar uma variável indicadora informando que aquele valor estava ausente, porque a ausência em si pode carregar informação. Também podemos remover observações ou utilizar modelos que tratem missing de outra maneira, mas devemos evitar simplesmente substituir tudo por zero sem entender o significado desse zero. 

Para outliers, a situação é diferente: a regressão logística não assume normalidade dos preditores, mas observações extremas podem ter grande influência sobre os coeficientes, principalmente quando possuem alta alavancagem ou estão associadas à variável target de forma muito diferente do restante dos dados. Por isso, eu primeiro investigaria se o outlier representa um erro de qualidade dos dados ou uma observação legítima; se for erro, podemos corrigi-lo ou removê-lo, enquanto, se for legítimo, talvez seja melhor mantê-lo e considerar transformações, tratamento de valores extremos, regularização ou modelos mais robustos. 

---

### 14. É necessário aplicar pré processamento de dados como padronização e normalização?

Não é obrigatoriamente necessário, mas em muitos casos é altamente recomendado, principalmente quando usamos regressão logística com regularização ou quando queremos uma otimização mais estável. 

A regressão logística em si não exige que as variáveis estejam na mesma escala: se uma variável está entre 0 e 1 e outra entre 0 e 100.000, o modelo ainda pode, em princípio, aprender os coeficientes adequados. O problema aparece principalmente na otimização: como métodos como Gradiente Descendente usam o gradiente para atualizar os parâmetros, features em escalas muito diferentes podem gerar uma função de custo mal condicionada e tornar a convergência mais lenta ou instável. 

Além disso, quando usamos Ridge ou Lasso, o scaling se torna ainda mais importante, porque a penalização é aplicada aos coeficientes; sem padronização, uma variável pode ser penalizada de maneira efetivamente diferente de outra apenas por estar em uma escala diferente. Para variáveis categóricas transformadas em dummies, normalmente não há necessidade de padronização, enquanto para variáveis numéricas podemos utilizar, por exemplo, standardization, transformando cada variável para média 0 e desvio-padrão 1. 

---

### 15. O modelo tende a overfitting ou underfitting?

A regressão logística tende a apresentar maior viés e menor variância, principalmente quando comparada a modelos mais flexíveis, porque assume uma relação linear entre as variáveis e o log-odds da classe. Essa hipótese restringe a complexidade do modelo: por um lado, reduz a sensibilidade a pequenas variações nos dados e, consequentemente, o risco de overfitting; por outro, pode levar a underfitting quando a relação real entre as variáveis e o resultado é muito complexa ou não linear. Portanto, de forma geral, podemos associar a regressão logística a **viés relativamente alto e variância relativamente baixa**, mas isso não significa que ela nunca sofra overfitting — com muitas variáveis, features muito correlacionadas ou pouca regularização, por exemplo, isso pode acontecer. A regularização L1 ou L2 pode ser utilizada para controlar a complexidade e reduzir a variância.