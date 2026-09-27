# Support Vector Machine 

### 1. O que é SVM?

Support Vector Machine (SVM), é um algoritmo de aprendizado supervisionado usado principalmente para classificação, embora também exista uma versão para regressão, chamada Support Vetor Regression (SVR).

O modelo se baseia na ideia encontrar um hiperplano que separe as classes de forma a maximizar a margem, ou seja, a distância entre o hiperplano e as observações mais próximas de cada classe. Essas observações mais próximas são chamadas de vetores de suporte e são justamente elas que determinam a posição da fronteira de decisão. Em problemas em que as classes não são linearmente separáveis, o SVM pode utilizar kernels para representar os dados em um espaço de maior dimensão onde uma separação possa ser encontrada, como se o modelo aplicasse transformações nos dados para visualizar o mesmo efeito de outra forma no plano, uma forma visualmente melhor separável.

Podemos fazer uma analogia entre uma rua e as calçadas laterais, é como se a rua fosse a fronteira e as calçadas os vetores de suporte, de modo que cada classe só pode ficar para trás do seu lado da calçada. O objetivo do SVM é justamente maximizar essa margem (largura da rua), pois o modelo parte do pressuposto que isso é eficaz em o bter uma separação mais robusta aos dados.

Matematicamente, o SVM procura uma reta, plano ou hiperplano que possa ser escrito como: w^T x + b = 0.
Essa é a fronteira de decisão.

* x = características da observação
* w = pesos atribuídos às características
* b = intercepto

Para uma classificação binária, o modelo calcula:
f(x) = w^T x + b 

Se f(x) for positivo, podemos classificar como uma classe; se for negativo, como a outra.

---

### 2. O que são os support vectors e por que eles são tão importantes para definir o hiperplano de decisão?

Os support vectors são as observações de treinamento que ficam mais próximas da fronteira de decisão, ou seja, aquelas que ficam na margem ou, no caso de uma soft margin, podem até estar dentro dela. Eles são importantes porque são justamente os pontos que determinam a posição e a orientação do hiperplano de decisão: o SVM busca a fronteira que maximiza a margem em relação a esses pontos. As observações que estão muito distantes da fronteira têm pouca ou nenhuma influência direta sobre sua definição, enquanto os support vectors são os pontos críticos que sustentam essa fronteira. Por isso, se alterarmos ou removermos alguns desses pontos, o hiperplano pode mudar significativamente, enquanto alterações em pontos muito distantes da margem tendem a ter pouco impacto.

---

### 3. Explique a diferença entre SVM de margem rígida (hard margin) e margem suave (soft margin). Em que situação você usaria cada um?

A diferença está em como o SVM trata erros de classificação e violações da margem. 

No **hard margin**, o modelo exige que todas as observações sejam corretamente separadas e estejam fora da margem, ou seja, **não permite erros no conjunto de treinamento**. Por isso, só é adequado quando os dados são realmente separáveis de forma linear e possuem pouco ou nenhum ruído.

Já no **soft margin**, o SVM permite que algumas observações ultrapassem a margem ou até sejam classificadas incorretamente, em prol de obter um poder de generalização melhor do problema, e **adicionando uma penalização por essas violações**. Esse equilíbrio é controlado pelo parâmetro (C): valores maiores penalizam mais os erros e tendem a produzir uma margem mais rígida, enquanto valores menores permitem mais violações em troca de uma margem maior.

Na prática, o soft margin é muito mais utilizado, porque dados reais geralmente possuem ruído, sobreposição entre classes e outliers; o hard margin é mais apropriado para situações em que temos uma separação claramente perfeita e confiável entre as classes.

---

### 4. Qual é o papel do hiperparâmetro C? O que tende a acontecer quando aumentamos ou diminuímos C?

O hiperparâmetro (C) controla a penalização dos erros aceitáveis. Define o equilíbrio entre maximizar a margem e penalizar erros ou violações da margem. 

Quando aumentamos (C), o modelo passa a penalizar mais fortemente os erros de classificação, buscando classificar corretamente mais observações do treinamento, mesmo que isso resulte em uma margem menor; portanto, o modelo fica mais rígido e pode apresentar maior risco de overfitting. 

Quando diminuímos (C), o modelo aceita mais violações da margem e dá mais importância a encontrar uma margem ampla, produzindo uma fronteira mais tolerante a ruídos e podendo reduzir a variância, embora um (C) excessivamente baixo possa levar a underfitting. 

Assim, de forma intuitiva, (C) alto significa "erre pouco, mesmo que a margem fique menor", enquanto (C) baixo significa "aceite alguns erros para obter uma margem maior".

Porém, seria simplista afirmar que "C alto sempre causa overfitting", porque o efeito de C depende da estrutura dos dados, do kernel e de outros hiperparâmetros, como gamma no RBF. Um C alto pode ser adequado quando os dados são pouco ruidosos e a fronteira precisa ser mais precisa, enquanto o overfitting ocorre quando a combinação dos hiperparâmetros torna o modelo excessivamente adaptado às particularidades do conjunto de treinamento. Por isso, C deve ser escolhido empiricamente, normalmente por validação cruzada.

---

### 5. O que é o kernel trick e qual problema ele resolve?

O **kernel trick** é uma técnica que permite ao SVM encontrar **fronteiras de decisão não lineares** sem precisar calcular explicitamente novas variáveis ou construir de fato um espaço de dimensão maior. O problema que ele resolve é quando, no espaço original, não existe um hiperplano capaz de separar adequadamente as classes. 

Em vez de criar explicitamente novas representações dos dados, o kernel calcula diretamente uma função de similaridade entre pares de observações, como o produto interno entre suas representações em um espaço transformado. Assim, o SVM consegue trabalhar matematicamente como se estivesse nesse novo espaço, mas sem precisar realizar toda a transformação de forma explícita. 

Isso permite, por exemplo, que um SVM com kernel RBF construa uma **fronteira curva** no espaço original, capturando relações não lineares entre as classes. Portanto, o kernel trick resolve o problema da separação não linear de maneira computacionalmente mais eficiente do que criar explicitamente todas as novas dimensões necessárias.

---

### 6. Compare os kernels linear, polinomial e RBF. Em que tipo de problema você escolheria cada um?

Os três kernels permitem que o SVM encontre fronteiras de decisão com diferentes níveis de complexidade. 

O **kernel linear** é o mais simples e deve ser utilizado quando existe uma relação aproximadamente linear entre as classes, além de ser uma boa opção quando temos muitas features e queremos menor custo computacional e maior interpretabilidade. 

O **kernel polinomial** permite representar relações não lineares por meio de **combinações polinomiais** das variáveis, sendo útil quando acreditamos que a separação possui uma estrutura não linear relativamente controlada; sua complexidade pode ser ajustada principalmente pelo grau do polinômio. 

Já o **kernel RBF**, baseado em uma função de similaridade que considera a **distância** entre as observações, permite construir fronteiras bastante flexíveis e é uma opção comum quando a relação entre as classes é não linear e não conhecemos previamente sua estrutura. Porém, essa flexibilidade aumenta o custo computacional e exige maior atenção à escolha dos hiperparâmetros, principalmente C e gamma. 

Uma abordagem é avaliar um kernel linear como baseline e considerar polinomial ou RBF quando houver evidência de que uma fronteira linear não é suficiente.

---

### 7. Qual é o papel do hiperparâmetro gamma no kernel RBF? O que acontece com o modelo quando gamma é muito alto ou muito baixo?

O gamma controla o **quanto o kernel RBF considera a proximidade entre as observações**, determinando o alcance de influência de cada ponto sobre a fronteira de decisão. 

Quando gamma é **alto**, a influência de cada observação fica mais local, então o modelo cria uma fronteira mais flexível e consegue se adaptar a detalhes e pequenas variações dos dados de treinamento, aumentando o risco de **overfitting** e de alta variância. 

Quando gamma é **baixo**, cada observação exerce influência sobre uma região maior, produzindo uma fronteira mais suave e menos complexa, o que tende a reduzir a variância, mas, se for excessivamente baixo, pode deixar o modelo simples demais e causar **underfitting** e alto viés. 

Portanto, de forma intuitiva, **gamma alto = influência local e fronteira mais complexa; gamma baixo = influência mais ampla e fronteira mais suave**.

---

### 8. Por que a normalização ou padronização das variáveis é particularmente importante para SVM? 

A normalização ou padronização é particularmente importante no **SVM** porque o modelo utiliza **distâncias e produtos internos** para definir a fronteira de decisão, especialmente quando usamos kernels como o **RBF**. 

Se as variáveis estiverem em escalas muito diferentes, uma delas pode dominar o cálculo da distância e, consequentemente, influenciar muito mais a definição da fronteira. 

Ao padronizar as variáveis, colocamos ambas em uma escala comparável, permitindo que o SVM considere adequadamente a contribuição de cada uma.

---

### 9. Como você explicaria matematicamente a função objetivo de um SVM com margem suave? O que representam o termo de regularização e o hinge loss?

O SVM busca encontrar uma fronteira de decisão que maximize a distância entre as classes. Essa distância é chamada de margem e, matematicamente, é dada por (2/|w|). Portanto, para maximizar a margem, precisamos minimizar (|w|), já que quanto menor for o denominador, maior será o resultado da divisão.

Em vez de minimizar diretamente (|w|), podemos minimizar uma função equivalente e mais conveniente matematicamente: (1/2|w|^2), que é convexa e possui um mínimo único global. Essa transformação não muda o objetivo de encontrar a maior margem, mas torna o problema mais conveniente para ser resolvido por técnicas de otimização. Esse termo é chamado de regularização, porque também ajuda a controlar a complexidade do modelo.

No caso do soft margin, porém, não queremos apenas uma margem grande. Também queremos penalizar os pontos que ficam dentro da margem ou que são classificados incorretamente. Para isso, adicionamos o hinge loss, que mede a violação da margem. Quando um ponto está corretamente classificado e fora da margem, sua perda é zero; quando viola a margem, a perda aumenta proporcionalmente à violação.

Assim, a função objetivo do SVM com soft margin combina os dois objetivos: o termo de regularização, que busca uma margem ampla, e o hinge loss, que penaliza as violações. O hiperparâmetro (C) controla o peso dado às violações. Conceitualmente, podemos escrever essa função como:

objetivo = regularização + (C) × hinge loss total

Dessa forma, o SVM procura um equilíbrio entre maximizar a margem e reduzir as violações da margem, em vez de exigir que todos os pontos sejam perfeitamente separados.

---

### 10. Como o SVM lidaria com uma base de dados com muitas colunas? e com muitas linhas?

O SVM costuma lidar melhor com alta dimensionalidade do que com um número muito grande de observações. 

Muitas features não são necessariamente um problema, principalmente quando temos poucas ou médias quantidades de linhas, mas é importante fazer escalonamento e eventualmente seleção de features. 

Já quando temos muitas linhas, o custo computacional pode crescer bastante, principalmente com kernels não lineares como o RBF, porque eles trabalham com relações entre as observações e pode gerar um número elevado de support vectors.

---

### 11. Como o SVM se comporta em um problema altamente desbalanceado? Que estratégias você adotaria para lidar com isso?

Em um problema altamente desbalanceado, o **SVM pode favorecer a classe majoritária**, porque minimizar os erros sem considerar o custo de cada classe pode levar o modelo a classificar muitos exemplos como pertencentes à classe dominante. 

Para lidar com isso, eu começaria utilizando **pesos de classe**, aumentando o custo dos erros cometidos na classe minoritária; em implementações como o `SVC`, isso pode ser feito com `class_weight='balanced'`. 

Também poderia utilizar técnicas de **undersampling da classe majoritária ou oversampling da minoritária**, tomando cuidado para aplicar essas técnicas apenas no conjunto de treinamento e evitar vazamento de dados. 

Além disso, eu não avaliaria o modelo apenas pela acurácia, pois ela pode ser enganosa nesse cenário; utilizaria métricas como **precision, recall, F1-score e matriz de confusão**. 

---

### 12. Como você faria tuning de C e gamma em um SVM com RBF? Que métrica e estratégia de validação utilizaria se o problema fosse de classificação desbalanceada?

Eu faria o tuning de **\(C\)** e **\(\gamma\)** utilizando uma busca em grade, como **Grid Search**, ou uma busca aleatória, testando valores em **escala logarítmica**, por exemplo, \(C \in \{0.01, 0.1, 1, 10, 100\}\) e \(\gamma \in \{0.001, 0.01, 0.1, 1\}\), porque esses hiperparâmetros podem variar bastante de escala. 

Em um problema desbalanceado, eu não utilizaria acurácia como métrica principal; escolheria uma métrica de acordo com o objetivo do negócio, como **F1-score** quando quero equilibrar precision e recall, **recall** quando é especialmente importante identificar a classe minoritária, ou **PR-AUC** quando quero avaliar o desempenho sobre diferentes limiares em um cenário com forte desbalanceamento. 

Para validação, utilizaria **Stratified K-Fold Cross-Validation**, que mantém aproximadamente a mesma proporção entre as classes em cada fold. Além disso, qualquer oversampling ou undersampling deve ser realizado **dentro de cada fold de treinamento**, por exemplo utilizando um pipeline, para evitar vazamento de dados. Dessa forma, consigo selecionar a combinação de \(C\) e \(\gamma\) que apresenta melhor desempenho de generalização na métrica escolhida.

---

### 13. SVM é sensível a outliers?

Sim. O SVM é sensível a outliers, principalmente porque alguns pontos podem influenciar bastante a posição do hiperplano e, consequentemente, a margem.

No hard margin, essa sensibilidade é ainda maior, porque o modelo exige que todos os pontos sejam corretamente classificados e respeitem a margem. Um único outlier pode fazer o hiperplano se deslocar bastante para conseguir separá-lo, reduzindo a margem ou até tornando o problema inviável.

No soft margin, o parâmetro C ajuda a controlar esse efeito. Quando C é muito alto, o SVM penaliza fortemente os pontos que violam a margem, então tende a tentar se ajustar aos outliers, ficando mais sensível a eles. Quando C é menor, o modelo aceita mais violações em troca de uma margem mais ampla, tornando-se menos influenciado por pontos individuais.

---

### 14. Qual o custo computacional de treinamento e inferência do SVM?

O custo computacional do SVM depende principalmente da quantidade de observações, do número de variáveis e, principalmente, do tipo de kernel utilizado. 

No treinamento, SVMs com kernels não lineares, como RBF ou polinomial, podem ser bastante custosos quando temos muitas linhas, porque o algoritmo precisa trabalhar com relações entre pares de observações, fazendo com que o custo e a memória cresçam de forma significativa com o número de amostras. Por isso, o SVM costuma ser mais confortável em bases com poucas ou médias quantidades de observações, mesmo podendo lidar bem com muitas variáveis. Já com kernel linear, o treinamento pode ser muito mais escalável, especialmente em problemas de alta dimensionalidade. 

Na inferência, o custo depende principalmente da quantidade de support vectors: para classificar uma nova observação, o modelo precisa calcular sua relação com os vetores de suporte. Assim, quanto mais support vectors o modelo tiver, maior será o custo para fazer previsões. 

---

### 15. Como lidar com problemas de classificação não binários?

Em problemas de classificação não binários, ou seja, quando temos três ou mais classes, existem algumas estratégias para transformar o problema em classificações binárias. No contexto de SVM, as principais são One-vs-Rest (OvR) e One-vs-One (OvO).

No One-vs-Rest, treinamos um classificador para cada classe. Por exemplo, se temos as classes A, B e C, treinamos três modelos: A contra B e C, B contra A e C, e C contra A e B. Na previsão, cada modelo produz um score e escolhemos a classe associada ao maior score.

No One-vs-One, treinamos um classificador para cada par de classes. Com A, B e C, teríamos A contra B, A contra C e B contra C. Na previsão, cada classificador "vota" em uma das duas classes, e a classe que receber mais votos é escolhida.

A diferença principal é que One-vs-Rest utiliza menos modelos, mas cada modelo precisa distinguir uma classe de todas as outras, enquanto One-vs-One utiliza mais modelos, porém cada modelo resolve um problema menor, envolvendo apenas duas classes. Em SVM, essa escolha é importante principalmente por causa do custo computacional, especialmente quando temos muitas classes.

---

### 16. Quais são as principais suposições do SVM?

O SVM não exige uma distribuição estatística específica dos dados; sua principal suposição é geométrica, de que existe uma fronteira de decisão capaz de separar ou distinguir razoavelmente as classes, buscando uma fronteira com boa margem. Na prática, é importante também considerar a escala das variáveis, a presença de outliers e a adequação do kernel escolhido.

---

### 17. Como o algoritmo lida com missings? 

O SVM não lida diretamente com valores ausentes (missings). Em geral, as implementações tradicionais, como SVC do scikit-learn, esperam que todas as variáveis utilizadas no treinamento e na previsão estejam preenchidas. Portanto, é necessário fazer um tratamento dos missings antes de treinar o modelo.

---

### 18. Quais as principais vantagens e desvantagens do algoritmo?

As principais vantagens do SVM são que ele funciona bem em problemas de alta dimensionalidade, consegue encontrar fronteiras de decisão não lineares por meio dos kernels e busca uma fronteira que maximize a margem, o que pode favorecer uma boa generalização. Além disso, ele não exige uma distribuição estatística específica dos dados e costuma funcionar bem mesmo quando temos relativamente poucas observações em relação à quantidade de variáveis. 

Por outro lado, uma das principais desvantagens é o custo computacional quando temos muitas observações, principalmente utilizando kernels não lineares. O SVM também é sensível à escala das variáveis, aos outliers e à escolha dos hiperparâmetros, como C e gamma. Além disso, o resultado pode ser menos interpretável do que modelos mais simples, porque a fronteira de decisão, especialmente com kernels, pode ser difícil de explicar. Ele também não trata missings diretamente, exigindo uma etapa de pré-processamento.