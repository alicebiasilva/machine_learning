# Árvore de decisão

### 1. O que é uma árvore de decisão?

É um algoritmo de aprendizado supervisionado utilizado tanto em problemas de classificação quanto de regressão que realiza as previsões criando uma sequência hierárquica de divisão de dados, que visualmente se torna uma grafo acíclico, ou árvore, de regras lógicas, onde cada nó representa uma regra de decisão baseado em uma variável, e cada folha contém a previsão final. A resposta final é dada pela classe majoritária do nó ou pela média. 

---

### 2. Como o algoritmo decide qual variável deve ser usada no nó?

A escolha é feita avaliando todas as variáveis disponíveis e todos os possíveis pontos de corte associados a cada uma delas. Para cada combinação , o algoritmo calcula quão bem a regra proposta separa o target, ou seja, aumenta a homogeneidade dos grupos resultantes. 

Para variáveis numéricas, o algoritmo primeiro ordena os valores distintos da variável e gera candidatos de corte entre os valores consecutivos, normalmente usando os pontos médicos entre eles. Para cada ponto médio, os registros da base são divididos entre "maior ou igual que" e "menor que" em relação ao valor de corte, e o algoritmo calcula o quão homogêneos esses grupos se tornam em relação ao target.

Para variáveis categóricas, o algoritmo precisa encontrar a melhor forma de agrupar as categorias. Dependendo do algoritmo utilizado, ele pode testar diferentes combinações de categorias para criar os grupos e avaliar qual divisão produz a maior redução na impureza ou o maior ganho de informação. Por exemplo, se uma variável possui as categorias A, B, C e D, o algoritmo pode testar uma divisão como {A, B} vs. {C, D} e compará-la com outras combinações possíveis.

Depois de avaliar todas essas possibilidades, o algoritmo compara os resultados obtidos por todas as variáveis e escolhe aquela que produz a melhor divisão do conjunto de dados, de acordo com o critério utilizado pela árvore, como ganho de informação, redução da entropia ou redução da impureza de Gini.

Assim, a variável escolhida para o nó não é simplesmente aquela que possui maior correlação com o target, mas aquela que, considerando também o ponto de corte ou agrupamento das categorias, consegue produzir a separação mais eficiente entre os grupos naquele momento da construção da árvore. Esse processo é repetido de forma recursiva nos nós seguintes, até que algum critério de parada seja atingido.

---

### 3. Como é calculado quão bem a variável separa o target em classificação? 

Para entender como o algoritmo de uma árvore de decisão escolhe a melhor variável para um nó, primeiro precisamos entender como ele mede a qualidade de uma divisão. A ideia principal é verificar o quanto uma determinada regra consegue tornar os grupos resultantes mais homogêneos em relação ao target. Em outras palavras, antes da divisão podemos ter um conjunto bastante misturado, com diferentes classes do target, e uma boa divisão é aquela que consegue separar essas classes de maneira mais eficiente.

Em problemas de classificação, as duas medidas mais conhecidas para avaliar essa impureza são a impureza de Gini e a entropia. Ambas têm o mesmo objetivo geral, que é medir o grau de mistura das classes dentro de um nó, mas utilizam fórmulas diferentes.

No caso do Gini, e considerando, por exemplo, um problema binário em que o target pode assumir os valores 0 ou 1, a impureza de Gini é calculada como 1−(p0^2)−(p1^2), em que p0 e p1 representam as proporções de cada classe dentro daquele nó. Se tivermos um nó com 50% de cada classe, ele será bastante impuro. Já se tivermos um nó composto praticamente por uma única classe, a impureza será próxima de zero. Portanto, quanto menor a impureza, mais homogêneo é o grupo.

A entropia segue uma ideia semelhante, mas utiliza uma função logarítmica. A interpretação é parecida: quando temos apenas uma classe no nó, a entropia é zero, porque não existe incerteza sobre a classe de uma observação escolhida aleatoriamente. Quando as classes estão mais equilibradas, a incerteza aumenta e, consequentemente, a entropia também aumenta. Em um problema binário, por exemplo, um nó com 50% de cada classe possui a maior entropia possível.

Depois de definir uma dessas medidas, a árvore começa a testar diferentes divisões. Para uma variável numérica, por exemplo, ela pode testar regras como "idade menor que 30", "idade menor que 40", "idade menor que 50" e assim por diante. Para cada possível ponto de corte, os dados são separados em dois grupos. O algoritmo então calcula a impureza de cada um dos grupos resultantes.

Como os grupos podem ter tamanhos diferentes, a impureza da divisão é calculada por uma média ponderada das impurezas dos nós filhos. Por exemplo, se uma divisão gerar um grupo com 70% das observações e outro com 30%, a impureza do primeiro grupo terá um peso de 70% e a do segundo, de 30%. 

No caso da entropia, por exemplo, podemos representar a qualidade da divisão pelo ganho de informação. Primeiro calculamos a entropia do nó pai e depois subtraímos a entropia ponderada dos nós filhos. Quanto maior for esse ganho, melhor é a divisão, porque significa que a regra conseguiu reduzir bastante a incerteza sobre o target.

Com o Gini, a lógica é praticamente a mesma. Calculamos a impureza de Gini do nó pai e subtraímos a impureza ponderada dos nós filhos. Nesse caso, podemos chamar o resultado de redução da impureza. Novamente, quanto maior essa redução, melhor é a divisão.

Uma diferença importante entre Gini e entropia é a forma como elas medem a impureza. A entropia está diretamente relacionada ao conceito de informação e incerteza, enquanto o Gini é uma medida de impureza baseada nas probabilidades ao quadrado. Na prática, os dois critérios frequentemente produzem árvores semelhantes, embora possam escolher divisões diferentes em alguns casos. O Gini também tende a ser computacionalmente um pouco mais simples porque não envolve o cálculo de logaritmos.

---

### 4. Como é calculado quão bem a variável separa o target em regressão?

Em regressão, a lógica é muito parecida com a classificação, mas existe uma diferença fundamental: o target é contínuo, então não faz sentido falar em classes, Gini ou entropia. Em vez disso, a árvore procura divisões que façam com que os valores do target fiquem mais próximos entre si dentro de cada grupo.

Imagine que queremos prever o preço de uma casa usando a variável área. A árvore pode testar um ponto de corte como área < 100 m². Ela vai separar os dados em dois grupos e perguntar: os preços dentro de cada grupo ficaram mais homogêneos depois dessa divisão?

Para responder isso, uma das medidas mais comuns é o erro quadrático médio (MSE), ou equivalentemente a variância do target dentro do nó. O princípio é que, dentro de cada nó, a melhor previsão constante para o target é a média dos valores daquele nó.

---

### 5. As divisões de uma árvore precisam ser sempre binárias?

Não necessariamente. Isso depende do algoritmo de árvore de decisão e da implementação utilizada. Porém, em algoritmos bastante comuns, como o CART, as divisões são binárias, ou seja, cada nó interno é dividido em exatamente dois grupos. Por exemplo, se estivermos trabalhando com a variável idade, a árvore pode criar uma regra como "idade menor que 35" para um dos filhos e "idade maior ou igual a 35" para o outro.

No caso de variáveis categóricas, uma divisão binária também não significa necessariamente separar uma categoria contra todas as outras. Se tivermos uma variável região com as categorias Norte, Nordeste, Sul, Sudeste e Centro-Oeste, por exemplo, uma possível divisão seria colocar Norte, Nordeste e Centro-Oeste de um lado e Sul e Sudeste do outro. Então, mesmo tendo várias categorias, a árvore ainda pode produzir apenas dois filhos.

A razão para utilizar divisões binárias é que elas tornam o processo de construção da árvore mais simples e permitem que o algoritmo avalie sistematicamente diferentes pontos de corte ou diferentes agrupamentos de categorias. Depois que a primeira divisão é feita, cada um dos dois nós resultantes pode ser dividido novamente, criando uma estrutura hierárquica. Por isso, mesmo com divisões apenas binárias, a árvore consegue representar relações bastante complexas.

Por outro lado, existem algoritmos que permitem divisões com mais de dois filhos. Um exemplo é o CHAID, que pode criar múltiplos grupos em uma única divisão, principalmente quando trabalha com variáveis categóricas. Nesse caso, uma variável como região poderia gerar diretamente vários nós, em vez de fazer uma sequência de divisões binárias.

---

### 6. Quais são os benefícios de uma árvore de decisão?

Uma das principais vantagens de uma árvore de decisão é a sua interpretabilidade. Diferentemente de modelos mais complexos, como redes neurais, uma árvore permite visualizar de forma relativamente simples quais regras estão sendo utilizadas para chegar a uma previsão. Por exemplo, podemos ter uma sequência de decisões como "idade menor que 35?", depois "renda maior que 5 mil?", e assim por diante. Isso facilita bastante a explicação do modelo para pessoas técnicas e também para áreas de negócio.

Outro benefício importante é que as árvores conseguem trabalhar tanto com variáveis numéricas quanto categóricas, dependendo da implementação, e não exigem necessariamente que os dados sejam escalados ou normalizados. Isso é diferente de alguns algoritmos que são sensíveis à escala das variáveis. Além disso, as árvores conseguem capturar relações não lineares e interações entre variáveis de maneira natural. Por exemplo, o efeito de uma variável pode depender de outra, e a árvore consegue representar isso por meio de diferentes caminhos e regras sem que seja necessário especificar manualmente essas interações.

Também considero uma vantagem o fato de as árvores exigirem relativamente pouco pré-processamento dos dados. Em geral, não precisamos transformar as variáveis para colocá-las na mesma escala, e o modelo consegue encontrar automaticamente os pontos de corte relevantes para as variáveis numéricas. Dependendo da implementação, também podemos trabalhar com dados categóricos de maneira bastante direta.

Outro ponto interessante é que árvores podem ser utilizadas tanto para classificação quanto para regressão. Em classificação, elas podem prever uma classe, como "fraude" ou "não fraude", enquanto em regressão podem prever um valor contínuo, como preço, renda ou demanda.

Além disso, árvores individuais servem como base para métodos de ensemble muito importantes, como Random Forest e métodos de boosting, como Gradient Boosting, XGBoost e LightGBM. Nesses casos, várias árvores são combinadas para obter modelos mais robustos e, geralmente, com melhor capacidade preditiva.

---

### 7. Quais as limitações desse algoritmo?

Uma das principais limitações de uma árvore de decisão é a tendência ao overfitting. Como a árvore vai criando novas divisões de forma recursiva, se não houver algum controle de complexidade ela pode crescer demais e começar a aprender características muito específicas dos dados de treinamento, inclusive ruído. Nesse caso, ela pode apresentar um desempenho muito bom no treino, mas generalizar mal para dados novos. Por isso, é importante utilizar mecanismos como profundidade máxima, número mínimo de observações por nó ou por folha, ganho mínimo para realizar uma divisão e, em alguns algoritmos, poda da árvore.

Outra limitação é que uma árvore individual pode apresentar alta variância. Isso significa que pequenas alterações nos dados de treinamento podem produzir uma estrutura de árvore bastante diferente. Uma determinada observação pode fazer com que uma divisão seja escolhida em um determinado ponto e, a partir daí, todas as divisões seguintes daquele ramo podem mudar. Essa instabilidade é uma das razões pelas quais métodos de ensemble, como Random Forest e Gradient Boosting, costumam apresentar desempenho mais robusto do que uma árvore isolada.

Também existe o problema de que árvores são modelos orientados a divisões sucessivas. Elas criam regiões do espaço de características por meio de regras do tipo "variável menor que determinado valor" ou "variável pertence a determinado grupo". Isso pode dificultar a representação de alguns padrões, principalmente relações lineares simples que poderiam ser representadas de maneira muito mais eficiente por um modelo linear. Uma árvore pode precisar de muitas divisões para aproximar determinados relacionamentos.

Outra limitação está relacionada à extrapolação. Em regressão, uma árvore normalmente prevê, em cada folha, um valor baseado nas observações de treinamento que chegaram àquela folha, frequentemente a média do target. Portanto, ela não costuma extrapolar bem para valores que estejam fora do comportamento observado nos dados de treinamento. Se, por exemplo, o maior valor de preço observado no treinamento for 500 mil, a árvore não vai naturalmente aprender uma tendência linear e prever 600 mil para uma nova observação; ela tende a retornar um valor associado a alguma das regiões que aprendeu.

Também é importante tomar cuidado com variáveis categóricas de alta cardinalidade. Variáveis com muitas categorias podem gerar divisões muito específicas e aumentar o risco de overfitting, dependendo de como são tratadas pelo algoritmo. Além disso, algumas implementações exigem que as variáveis categóricas sejam transformadas antes de serem utilizadas, por exemplo, por meio de one-hot encoding.

Outra questão é que uma árvore pode apresentar viés em relação a determinadas variáveis ou divisões, dependendo do critério utilizado e da forma como os candidatos são avaliados. Em algumas situações, variáveis com muitas possibilidades de divisão podem ter maior probabilidade de encontrar uma divisão aparentemente boa simplesmente por acaso. Isso precisa ser considerado principalmente quando estamos comparando diferentes tipos de variáveis.

Por fim, embora uma árvore individual seja relativamente fácil de interpretar, essa vantagem diminui conforme sua complexidade aumenta. Uma árvore muito profunda pode ter centenas de nós e se tornar tão grande que, apesar de ser tecnicamente interpretável, deixa de ser realmente compreensível para uma pessoa. Isso é ainda mais evidente quando passamos de uma árvore individual para ensembles com centenas ou milhares de árvores.

---

### 8. Como a árvore lida com valores ausentes?

A forma como uma árvore de decisão lida com valores ausentes depende bastante do algoritmo e da implementação utilizada. A árvore, por si só, não tem uma única estratégia universal para missing values. Algumas implementações conseguem tratar valores ausentes diretamente durante o treinamento e a predição, enquanto outras exigem que os valores sejam tratados previamente, por exemplo, por meio de imputação.

Uma estratégia bastante comum é fazer a imputação antes de treinar a árvore. Nesse caso, para uma variável numérica podemos substituir os valores ausentes pela média ou, de maneira geralmente mais robusta, pela mediana. Para uma variável categórica, podemos utilizar a categoria mais frequente ou criar uma categoria específica, como "desconhecido". Depois disso, a árvore trabalha normalmente com os dados já preenchidos. É importante que, em um processo de machine learning, essa imputação seja aprendida apenas com os dados de treinamento para evitar data leakage.

Alguns algoritmos de árvore possuem mecanismos próprios para lidar com valores ausentes. Uma abordagem é utilizar os chamados surrogate splits, ou divisões substitutas. A ideia é que, se uma observação não possui o valor da variável utilizada na divisão principal, a árvore procura outra variável que consiga reproduzir de maneira semelhante aquela divisão e utiliza essa variável como alternativa. Isso permite que a observação continue percorrendo a árvore mesmo com o valor ausente.

Outra possibilidade, presente em algumas implementações modernas, é aprender explicitamente para qual lado uma observação com valor ausente deve seguir. Por exemplo, suponha que a regra seja "renda < 5.000". Para as observações em que a renda está disponível, conseguimos decidir normalmente para qual filho elas devem ir. Durante o treinamento, o algoritmo pode aprender qual direção é mais adequada para os casos em que a renda estiver ausente, com base na redução de perda obtida. Assim, o missing passa a fazer parte do próprio processo de construção da árvore.

---

### 9. O que é a importância de uma variável?

A importância de uma variável em uma árvore de decisão é uma medida que tenta quantificar o quanto aquela variável contribuiu para as decisões tomadas pela árvore. Em outras palavras, ela nos ajuda a entender quais variáveis foram mais relevantes para reduzir o erro ou a impureza durante a construção do modelo.

Em uma árvore de classificação, por exemplo, sempre que uma variável é utilizada para fazer uma divisão, essa divisão produz alguma redução de impureza, que pode ser calculada usando Gini ou entropia. Em regressão, podemos utilizar critérios como redução da variância ou do erro quadrático médio. A contribuição daquela variável está relacionada justamente ao quanto essas divisões melhoraram o modelo.

Imagine, por exemplo, que uma árvore utilize a variável "renda" em três nós diferentes. Em cada um desses nós, a divisão utilizando renda produz uma determinada redução de impureza. A importância da variável pode ser calculada somando essas contribuições, normalmente ponderadas pelo número de observações que chegam a cada nó. Depois, essa contribuição pode ser normalizada para que a soma das importâncias de todas as variáveis seja igual a 1, ou 100%.

Então, se uma variável tiver importância de 0,40, podemos interpretar que ela foi responsável por aproximadamente 40% da redução total de impureza atribuída às variáveis utilizadas pela árvore, de acordo com esse critério específico.

Um ponto importante é que importância de variável não significa causalidade. Se uma variável tiver alta importância, isso não significa que ela cause o comportamento do target. Significa apenas que ela foi bastante útil para o modelo realizar suas divisões e reduzir o erro durante o treinamento.

Também é importante ter cuidado ao interpretar essa medida. A importância baseada na redução de impureza, muitas vezes chamada de feature importance ou Mean Decrease in Impurity, pode apresentar alguns vieses. Por exemplo, variáveis numéricas ou categóricas com muitas possibilidades de divisão podem ter mais oportunidades de encontrar divisões que parecem boas. Além disso, quando duas variáveis carregam informações muito semelhantes, a árvore pode escolher uma delas para fazer as principais divisões e acabar atribuindo uma importância menor à outra, mesmo que ela também seja bastante informativa.

Por isso, em uma análise mais cuidadosa, eu não utilizaria apenas essa importância para concluir quais variáveis são realmente relevantes. Poderia complementar a análise com métodos como permutation importance ou SHAP, que ajudam a avaliar a contribuição das variáveis de outras perspectivas.

---

### 10. O que é análise permutation importance e SHAP?

A Permutation Importance procura medir o quanto o desempenho do modelo piora quando embaralhamos aleatoriamente os valores de uma determinada variável. A ideia é simples: se eu embaralho os valores de uma variável, eu destruo a relação entre aquela variável e o target, mas mantenho aproximadamente a mesma distribuição dos valores. Depois disso, avalio novamente o desempenho do modelo. Se o erro aumentar bastante, significa que o modelo dependia daquela variável para fazer boas previsões, então podemos considerá-la importante. Se o desempenho praticamente não mudar, significa que o modelo consegue fazer suas previsões sem depender muito daquela variável.

Uma vantagem da Permutation Importance é que ela é relativamente simples e pode ser aplicada a diferentes tipos de modelos, porque não depende de conhecermos a estrutura interna do algoritmo. Porém, ela possui algumas limitações, principalmente quando existem variáveis muito correlacionadas. Se duas variáveis carregam praticamente a mesma informação, ao embaralhar uma delas o modelo pode continuar utilizando a outra. Nesse caso, a importância individual da primeira pode parecer baixa, mesmo que aquele conjunto de variáveis seja muito importante para o modelo.

SHAP, ou SHapley Additive exPlanations, é uma técnica utilizada para explicar como cada variável contribuiu para uma determinada previsão do modelo. A ideia vem dos valores de Shapley, da teoria dos jogos, em que imaginamos que cada variável é como um "jogador" que contribui para o resultado final. Para uma determinada observação, o SHAP começa com um valor base, que representa a previsão média do modelo, e calcula quanto cada variável contribuiu para afastar essa previsão do valor base até chegar à previsão final. Por exemplo, imagine um modelo que prevê o risco de inadimplência de um cliente e que a previsão média seja de 20%. Para um determinado cliente, o modelo pode chegar a uma previsão de 65%. O SHAP pode mostrar que a renda baixa contribuiu com +15 pontos percentuais, o histórico de atrasos com +25 pontos percentuais, enquanto o longo tempo de relacionamento com o banco contribuiu com -5 pontos percentuais. A soma dessas contribuições leva à previsão final de 65%. Dessa forma, o SHAP não apenas informa que uma variável é importante, mas mostra para aquela observação específica se ela empurrou a previsão para cima ou para baixo e quanto contribuiu para isso. Além das explicações individuais, podemos agregar os valores SHAP de várias observações para entender quais variáveis são mais importantes para o modelo como um todo. É importante lembrar, porém, que SHAP explica o comportamento do modelo, e não estabelece uma relação de causalidade entre as variáveis e o target.

---

### 11. Como evitar o overfitting?

Para evitar overfitting em árvores de decisão, a principal estratégia é controlar a complexidade da árvore. Uma árvore muito profunda consegue criar regras cada vez mais específicas para os dados de treinamento e, com isso, pode acabar aprendendo não apenas os padrões reais, mas também o ruído. Nesse caso, ela apresenta um desempenho muito bom no treinamento, mas perde capacidade de generalização em dados novos.

Uma das formas mais comuns de controlar isso é limitar a profundidade máxima da árvore, por meio do parâmetro max_depth. Quanto menor a profundidade, menor a complexidade do modelo e, consequentemente, menor a capacidade de criar regras extremamente específicas. Outra estratégia é definir um número mínimo de observações necessário para realizar uma divisão, como no min_samples_split. Dessa forma, a árvore não continua criando novos nós quando já existem poucas observações naquele grupo.

Também podemos definir um número mínimo de observações em uma folha, utilizando algo como min_samples_leaf. Isso impede que a árvore termine com folhas contendo pouquíssimas observações. Por exemplo, se permitirmos que uma folha tenha apenas uma observação, o modelo pode simplesmente memorizar características daquele indivíduo, o que aumenta bastante o risco de overfitting.

Outra possibilidade é exigir uma redução mínima de impureza ou de erro para que uma divisão seja realizada. Se uma determinada divisão melhora muito pouco o modelo, não vale a pena criar um novo nó. Assim, evitamos que a árvore continue crescendo por causa de pequenas melhorias que podem ser resultado apenas de ruído nos dados.

Também podemos utilizar poda da árvore, principalmente em algoritmos que suportam cost-complexity pruning. Nesse caso, podemos primeiro construir uma árvore maior e posteriormente remover partes que não contribuem suficientemente para o desempenho do modelo. A ideia é encontrar um equilíbrio entre a capacidade preditiva e a complexidade da árvore.

Além de controlar diretamente a árvore, eu utilizaria validação cruzada para escolher esses hiperparâmetros. Por exemplo, podemos testar diferentes valores de max_depth, min_samples_leaf e outros parâmetros e verificar quais apresentam melhor desempenho em dados que não foram utilizados no treinamento. Isso é importante porque simplesmente escolher uma árvore pequena não garante que teremos o melhor modelo; queremos encontrar uma complexidade que generalize bem.

Outra estratégia, principalmente quando buscamos maior performance, é utilizar ensembles de árvores, como Random Forest ou Gradient Boosting. No Random Forest, por exemplo, combinamos várias árvores treinadas com diferentes amostras e subconjuntos de variáveis, o que tende a reduzir a variância de uma árvore individual. Já métodos de boosting constroem árvores sequencialmente, tentando corrigir os erros das anteriores, e também possuem mecanismos de regularização para controlar a complexidade.

---

### 12. O modelo adota alguma premissa em relação aos dados?

De forma geral, uma das grandes vantagens das árvores de decisão é que elas fazem poucas premissas sobre a distribuição dos dados quando comparadas a modelos como regressão linear ou regressão logística. A árvore não assume, por exemplo, que as variáveis tenham distribuição normal, que exista uma relação linear entre as variáveis independentes e o target ou que as variáveis tenham escalas semelhantes. Ela simplesmente procura regras de divisão que melhorem a separação das classes, no caso de classificação, ou reduzam a variabilidade do target, no caso de regressão.

Também não existe uma premissa forte de independência entre as variáveis. Podemos ter variáveis correlacionadas e a árvore ainda consegue construir o modelo. No entanto, isso não significa que a correlação seja irrelevante: quando existem variáveis muito semelhantes, a árvore pode escolher uma delas para realizar uma divisão e acabar utilizando pouco ou nada a outra, o que pode afetar a interpretação da importância das variáveis.

Em regressão, especificamente, uma árvore também não assume que o relacionamento entre as variáveis e o target seja linear. Ela consegue representar relações não lineares por meio de uma sequência de divisões. Por exemplo, o efeito de uma variável sobre o target pode ser diferente em diferentes faixas de valores, e a árvore consegue capturar isso naturalmente.

Outra característica importante é que árvores não exigem que as variáveis estejam na mesma escala. Se tenho uma variável que varia de 0 a 1 e outra que varia de 0 a 1 milhão, isso não é um problema para a árvore, porque ela toma decisões com base em pontos de corte. Por esse motivo, normalmente não precisamos fazer normalização ou padronização das variáveis antes de treinar uma árvore.

Por outro lado, dizer que a árvore não faz premissas fortes não significa que não existam cuidados necessários. Ainda precisamos considerar questões como qualidade dos dados, valores ausentes, valores extremos, cardinalidade de variáveis categóricas e principalmente o risco de overfitting. Além disso, dependendo da implementação, pode haver requisitos específicos sobre como variáveis categóricas e valores ausentes devem ser tratados.

---

### 13. Qual o custo computacional do modelo?

O custo computacional de uma árvore de decisão é relativamente baixo, principalmente na predição. Durante o treinamento, o algoritmo precisa testar diferentes variáveis e possíveis pontos de corte para encontrar a divisão que melhor separa o target, então o custo aumenta conforme aumentam o número de observações, variáveis e possíveis divisões.

De forma simplificada, o treinamento pode ficar na ordem de O(p⋅nlog⁡n) em implementações eficientes, onde n é o número de observações e p o número de variáveis, embora a complexidade exata dependa da implementação e dos dados.
Na predição, o custo é menor, porque basta percorrer um caminho da raiz até uma folha. Portanto, ele depende principalmente da profundidade da árvore. Árvores muito profundas aumentam tanto o custo computacional quanto o risco de overfitting.

---

### 14. Quais modelos existem?

Existem diferentes algoritmos de árvores de decisão, e eles se diferenciam principalmente pela forma como escolhem as divisões, pelo tipo de target que conseguem tratar e pela forma como lidam com variáveis categóricas e valores ausentes. Um dos mais conhecidos é o CART, que significa Classification and Regression Trees. Ele pode ser utilizado tanto para classificação quanto para regressão e normalmente trabalha com divisões binárias, escolhendo a divisão que mais reduz a impureza ou o erro.

Outro algoritmo bastante conhecido é o ID3, que foi desenvolvido para classificação e utiliza principalmente o conceito de ganho de informação baseado em entropia para escolher as divisões. Uma evolução do ID3 é o C4.5, que também utiliza entropia, mas introduz melhorias como o gain ratio e mecanismos para lidar melhor com variáveis categóricas e valores ausentes. O C5.0 é uma evolução posterior dessa família, buscando melhorias de eficiência e desempenho.

Também temos o CHAID, que utiliza testes estatísticos, especialmente o qui-quadrado em problemas de classificação, para determinar as divisões. Diferentemente do CART, ele pode criar divisões com mais de dois filhos, sendo particularmente interessante quando trabalhamos com variáveis categóricas.

---

### 15. Por que as árvores de regressão tem estrutura de "escadinha"?

Árvores de regressão têm essa aparência de "escadinha" porque elas fazem previsões constantes dentro de cada região criada pelas divisões da árvore. Diferentemente de uma regressão linear, que aprende uma função contínua, a árvore divide o espaço das variáveis em diferentes regiões e, para cada região, normalmente utiliza a média dos valores do target das observações que caíram naquela folha como previsão.

Por exemplo, imagine que queremos prever o preço de um imóvel em função da área. A árvore pode criar uma primeira divisão em 100 m² e depois outras divisões em 150 m² e 200 m². Assim, podemos ter uma previsão média de 300 mil para imóveis abaixo de 100 m², 400 mil entre 100 e 150 m², 500 mil entre 150 e 200 m² e 600 mil acima de 200 m². Quando colocamos isso em um gráfico, o valor previsto permanece constante dentro de cada intervalo e muda abruptamente quando atravessamos um ponto de corte. É justamente isso que produz o formato de escada.

Então, a árvore não está aprendendo uma função contínua como y=ax+b. Ela está construindo uma função piecewise constant, ou seja, uma função constante por partes. Quanto mais profunda for a árvore e quanto mais divisões ela fizer, mais regiões ela consegue criar e, consequentemente, mais "degraus" aparecem.

Essa característica também explica uma das limitações das árvores de regressão: elas não são muito boas em extrapolação. Se a relação real entre uma variável e o target for aproximadamente linear, por exemplo, uma árvore vai representar essa relação por uma sequência de degraus em vez de uma reta. Além disso, fora do intervalo observado no treinamento, ela não consegue simplesmente continuar a tendência; normalmente vai atribuir a previsão correspondente a alguma folha existente.

---

### 16. Como funciona o processo de treinamento de uma árvore, passo a passo? 

O treinamento de uma árvore começa com todas as observações no nó raiz. O algoritmo avalia todas as variáveis e possíveis regras de divisão e, para cada uma, calcula o quanto ela melhora a separação do target. Em classificação, pode usar Gini ou entropia; em regressão, MSE ou redução da variância.

Depois, ele escolhe a divisão que proporciona o maior ganho e separa os dados em novos nós. Em seguida, repete o mesmo processo dentro de cada nó filho, procurando novamente a melhor variável e o melhor ponto de corte. Esse processo acontece recursivamente, fazendo a árvore crescer.

A árvore para de crescer quando algum critério é atingido, como max_depth, número mínimo de observações por folha ou ganho mínimo necessário para realizar uma nova divisão. Quando chegamos a uma folha, o modelo define a previsão: em classificação, normalmente a classe predominante; em regressão, geralmente a média do target daquela folha.

---

### 17. Qual a diferença entre pré-poda e pós-poda? 

A diferença é basicamente quando controlamos a complexidade da árvore. Na pré-poda, limitamos o crescimento da árvore durante o treinamento. Por exemplo, podemos definir uma profundidade máxima (max_depth), um número mínimo de observações para realizar uma divisão (min_samples_split) ou um número mínimo de observações por folha (min_samples_leaf). Assim, a árvore é impedida de crescer quando atinge esses critérios.

Na pós-poda, primeiro permitimos que a árvore cresça mais e, depois, removemos partes da árvore que não contribuem o suficiente para a capacidade de generalização do modelo. Um exemplo é a poda por complexidade de custo (cost-complexity pruning), em que buscamos um equilíbrio entre o erro da árvore e sua complexidade.

A principal diferença é que pré-poda impede o crescimento excessivo desde o início, enquanto pós-poda constrói uma árvore maior e depois simplifica sua estrutura. Em ambos os casos, o objetivo é reduzir o overfitting e melhorar a generalização.

---

### 18. Árvores são sensíveis a outliers?

Árvores de decisão são, em geral, menos sensíveis a outliers do que modelos como regressão linear, porque trabalham com regras de divisão e não dependem diretamente da distância entre as observações. Um valor extremo em uma variável, por exemplo, não altera a escala das outras observações nem necessariamente impede que a árvore encontre bons pontos de corte.

Porém, isso não significa que árvores sejam imunes a outliers. Em regressão, um outlier no target pode ter bastante influência, principalmente quando o critério utilizado é baseado em erro quadrático, como o MSE, porque o erro é elevado ao quadrado. Além disso, se a árvore tiver muita liberdade para crescer, ela pode criar divisões muito específicas para isolar algumas observações extremas, aumentando o risco de overfitting.

---

### 19. Árvores são sensíveis à escala das variáveis? Por quê?

Não. Em geral, árvores de decisão não são sensíveis à escala das variáveis, porque suas decisões são baseadas em pontos de corte e não em distância ou magnitude absoluta. Por exemplo, se uma variável assume valores entre 0 e 100 e eu transformá-la para uma escala entre 0 e 1, a árvore pode simplesmente ajustar o ponto de corte de acordo com a nova escala, mantendo essencialmente a mesma divisão dos dados.

Por isso, normalmente não é necessário aplicar técnicas como normalização ou padronização antes de treinar uma árvore. Isso é diferente de modelos como KNN, K-Means ou regressão com regularização, nos quais a escala das variáveis pode afetar diretamente o resultado.

---

### 20. O que acontece com variáveis altamente correlacionadas?

Quando temos variáveis altamente correlacionadas em uma árvore de decisão, o principal problema não é necessariamente a capacidade preditiva, mas a interpretação da importância das variáveis. Como duas variáveis carregam informações muito semelhantes, a árvore pode escolher uma delas para fazer uma determinada divisão e praticamente não utilizar a outra. Isso faz com que uma variável apareça como muito importante e a outra como pouco importante, mesmo que ambas contenham informações relevantes para prever o target.