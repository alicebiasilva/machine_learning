# Redes Neurais 

### 1. O que é o modelo de Redes Neurais no contexto de Machine Learning?

Rede neural é uma classe de modelos de Machine Learning que pode ser utilizada tanto para classificação quanto para regressão. Ela é formada por camadas de neurônios artificiais que realizam transformações matemáticas sobre as entradas. Durante o treinamento, os pesos dessas conexões são ajustados por meio de backpropagation e de um algoritmo de otimização, minimizando uma função de perda. Dependendo do problema, a camada de saída e a função de perda são configuradas de forma diferente. Por exemplo, em classificação binária, podemos ter uma saída com sigmoid produzindo uma probabilidade; em regressão, uma saída contínua. Redes neurais também são muito utilizadas em problemas mais complexos, como visão computacional, processamento de linguagem natural e séries temporais.

Ela aprende os pesos e vieses das conexões entre os neurônios. Esses parâmetros são ajustados iterativamente durante o treinamento para que a saída produzida pela rede se aproxime do valor esperado.

---

### 2. Qual o racional do modelo?

Primeiro, os dados de entrada são fornecidos à camada de entrada da rede. Cada variável de entrada é conectada aos neurônios da primeira camada e recebe um peso. O neurônio faz uma combinação linear das entradas, adicionando um viés, e depois aplica uma função de ativação. Essa saída é passada para a próxima camada, e o processo se repete até chegar à camada de saída. A previsão produzida pela rede é então comparada ao valor real por meio de uma função de perda, que mede o erro do modelo. A partir desse erro, o backpropagation calcula como cada peso contribuiu para o erro, obtendo os gradientes. Em seguida, um algoritmo de otimização, como Gradient Descent ou Adam, atualiza os pesos na direção que reduz a função de perda. Esse processo é repetido por várias iterações, ou épocas, até que a rede aprenda parâmetros que produzam boas previsões.

1. Entrada dos dados: as variáveis X entram na rede

2. Forward propagation: cada neurônio calcula z = XW = b e aplica uma função de ativação a = f(z)

3. Previsão: a camada de saída produz o resultado

4. Cálculo do erro: a previsão é comparada ao valor verdadeiro utilizando uma função de perda, como binary cross-entropy para classificação binária ou MSE para regressão

5. Backpropagation: a rede calcula os gradientes da função de perda em relação aos pesos, propagando o erro da saída em direção às camadas anteriores

6. Atualização dos pesos: o otimizador altera os pesos usando a taxa de aprendizado

7. Repetição: esse processo é repetido para os exemplos do treinamento por várias épocas. Ao longo do treinamento, os pesos vão sendo ajustados para minimizar a função de perda.

---

### 3. O que são pesos e bias?

Pesos e bias são parâmetros aprendidos pela rede neural durante o treinamento. Os pesos determinam a influência de cada entrada sobre o neurônio, enquanto o bias é um termo adicional que permite deslocar a transformação e aumentar a flexibilidade do modelo. O neurônio calcula uma combinação ponderada das entradas, adiciona o bias e depois aplica uma função de ativação. Durante o treinamento, esses parâmetros são ajustados pelo otimizador com base nos gradientes calculados pelo backpropagation.

Matematicamente, um neurônio começa calculando:

$$ z = w_1x_1 + w_2x_2 + ... + w_nx_n + b $$

em que:

* x_i = valores das entradas;
* w_i = pesos, que determinam quanto cada entrada influencia o resultado;
* b = bias, um termo independente das entradas que permite deslocar o resultado;

Depois disso, normalmente aplicamos uma função de ativação: a=f(z).

### 4. O que são camadas e quais seus tipos?

As camadas são os blocos que compõem uma rede neural. Cada camada recebe uma entrada, aplica uma transformação matemática e produz uma saída que é passada para a próxima camada. Os principais tipos são a camada de entrada, as camadas ocultas e a camada de saída. A camada de entrada recebe as variáveis do problema; as camadas ocultas fazem as transformações e aprendem representações dos dados; e a camada de saída produz a previsão final.

Nas camadas ocultas, normalmente temos camadas densas, em que cada neurônio está conectado aos neurônios da camada anterior. Dependendo do tipo de dado e problema, também podemos utilizar arquiteturas específicas, como camadas convolucionais em redes CNN para imagens, camadas recorrentes como LSTM e GRU para sequências, e mecanismos de atenção usados em Transformers.

A camada de saída depende da tarefa. Em uma classificação binária, por exemplo, é comum utilizar um neurônio com função sigmoid para produzir uma probabilidade. Em classificação multiclasse, podemos utilizar vários neurônios com softmax. Já em regressão, normalmente utilizamos uma saída linear para produzir um valor contínuo.

---

### 5. Como escolher o numero ideal de camadas?

Não existe um número ideal de camadas que funcione para todos os problemas. A quantidade de camadas é um hiperparâmetro que depende da complexidade do problema, da quantidade e do tipo de dados e da arquitetura utilizada. Em geral, começo com uma arquitetura mais simples e aumento a profundidade conforme necessário, avaliando a performance em validação. Se o modelo tiver poucas camadas, pode não ter capacidade suficiente para aprender padrões complexos, levando a underfitting. Por outro lado, aumentar excessivamente a profundidade pode aumentar a complexidade, o custo computacional e o risco de overfitting. Portanto, o número de camadas pode ser definido experimentalmente por validação, comparando diferentes arquiteturas e utilizando técnicas de regularização quando necessário.

Mais camadas não significa necessariamente um modelo melhor. A ideia é ter capacidade suficiente para representar a relação existente nos dados, mas sem adicionar complexidade desnecessária.

---

### 6. O que é Forward e Backpropagation?

Forward propagation é o processo em que os dados percorrem a rede neural da camada de entrada até a camada de saída para gerar uma previsão. Em cada camada, os neurônios fazem uma combinação ponderada das entradas, adicionam o bias e aplicam uma função de ativação. Depois que a previsão é gerada, calculamos o erro por meio da função de perda.

Backpropagation é o processo utilizado para calcular como cada peso da rede contribuiu para esse erro. O algoritmo parte da saída e propaga o erro no sentido contrário, calculando os gradientes da função de perda em relação aos pesos usando a regra da cadeia. Esses gradientes são então utilizados pelo otimizador, como Gradient Descent ou Adam, para atualizar os pesos e reduzir o erro nas próximas iterações.

---

### 7. O que é early stopping?

Early Stopping é uma técnica utilizada durante o treinamento para interromper o treinamento antes de atingir o número máximo de épocas quando o desempenho no conjunto de validação deixa de melhorar. A ideia é monitorar uma métrica, geralmente a função de perda de validação, e parar após um determinado número de épocas sem melhoria, chamado de patience. Isso ajuda a evitar overfitting e também reduz o custo computacional do treinamento.

---

### 8. O que são épocas e como escolher a quantidade ideal?

Época é uma passagem completa pelo conjunto de treinamento durante o processo de aprendizado da rede neural. Em cada época, os dados normalmente são divididos em batches, a rede faz o forward pass, calcula a função de perda, realiza o backpropagation e atualiza os pesos. A quantidade de épocas é um hiperparâmetro e não existe um número ideal fixo. Eu escolheria acompanhando o desempenho no conjunto de validação e utilizaria técnicas como Early Stopping. Enquanto o erro de validação continua diminuindo, o treinamento pode continuar. Quando ele deixa de melhorar por um determinado número de épocas, podemos interromper o treinamento para evitar overfitting e custo computacional desnecessário.

---

### 8. Qual a função de perda do modelo, o que ele tenta otimizar e como?

A função de perda mede o quanto as previsões da rede estão distantes dos valores reais. O objetivo do treinamento é minimizar essa função de perda, ajustando os pesos e os bias da rede.

A função de perda depende do problema. Por exemplo:

* Regressão: MSE, MAE, Huber Loss.
* Classificação binária: Binary Cross-Entropy.
* Classificação multiclasse: Categorical Cross-Entropy.

O processo funciona assim:

1. Forward propagation: a rede recebe as entradas e produz uma previsão.

2. Cálculo da perda: comparamos a previsão \(\hat{y}\) com o valor real \(y\).

3. Backpropagation: calculamos os gradientes da perda em relação aos pesos e bias, usando a regra da cadeia.

4. Otimizador: utiliza esses gradientes para atualizar os parâmetros, normalmente seguindo a direção que reduz a perda:

$$ \theta_{novo} = \theta_{atual} - \eta \frac{\partial L}{\partial \theta} $$

onde \(\theta\) representa os pesos e bias, \(L\) é a função de perda e \(\eta\) é o learning rate.

Esse processo é repetido ao longo dos batches e épocas até que a perda seja minimizada ou até que algum critério de parada seja atingido.

---

### 9. As transformações aplicadas nas camadas são sempre lineares?

Não. A transformação feita pelo neurônio antes da ativação é uma transformação linear — mais precisamente, uma transformação afim, porque envolve os pesos e o bias. Porém, depois dessa transformação normalmente aplicamos uma função de ativação não linear, como ReLU, sigmoid ou tanh. Essa não linearidade é fundamental para que a rede consiga aprender relações complexas. Se todas as camadas fossem apenas transformações lineares, mesmo empilhando várias camadas, a rede poderia ser reduzida a uma única transformação linear e teria capacidade limitada.

---

### 10. O que é função de ativação e quais os tipos?

A função de ativação é uma função aplicada à saída de um neurônio depois da transformação afim, introduzindo não linearidade na rede. Ela é importante porque, sem funções de ativação não lineares, mesmo uma rede com várias camadas poderia ser reduzida a uma única transformação linear e teria dificuldade para aprender relações complexas. A escolha da função depende da arquitetura e principalmente da camada em que ela é utilizada.

As principais são:

**ReLU f(x) = max(0,x)**

É muito utilizada nas camadas ocultas de redes profundas. Valores negativos são transformados em zero e valores positivos são mantidos.

Uma vantagem é ser computacionalmente simples e ajudar a reduzir problemas de gradiente em comparação com sigmoid e tanh em redes profundas. Porém, pode ocorrer o problema de “neurônios mortos”, quando um neurônio passa a produzir zero para praticamente todas as entradas.

**Sigmoid f(x) = 1 / (1+e^-x)**

Produz valores entre 0 e 1, por isso é muito utilizada na saída de classificadores binários, quando queremos interpretar a saída como uma probabilidade.

Um problema é que pode sofrer de vanishing gradient quando os valores de entrada são muito grandes ou muito pequenos.

**Tanh f(x) = (e^x - e^-x) / (e^x + e^-x)**

Produz valores entre -1 e 1. É centrada em zero e também pode sofrer de vanishing gradient. Foi bastante utilizada em redes neurais e ainda aparece, por exemplo, em algumas arquiteturas recorrentes.

**softmax**

É utilizada principalmente na camada de saída de classificação multiclasse.

Ela transforma os valores produzidos pela última camada em uma distribuição de probabilidades, de modo que as probabilidades das classes somem 1.

Nas camadas ocultas, ReLU é uma escolha bastante comum. Para classificação binária, sigmoid é frequentemente utilizada na saída; para classificação multiclasse, softmax; e para regressão, normalmente não utilizamos uma ativação não linear na saída, usando uma saída linear. A escolha pode mudar de acordo com a arquitetura e o problema.

---

### 11. A saída do modelo é sempre binária?

Não. A saída de uma rede neural não é necessariamente binária. Ela depende do tipo de problema e, principalmente, da função de ativação utilizada na camada de saída.

* Classificação binária: normalmente usamos 1 neurônio com sigmoid, produzindo uma probabilidade entre 0 e 1. Ex.: probabilidade de um cliente cancelar ou não.
* Classificação multiclasse: usamos um neurônio por classe com softmax. Ex.: classificar uma imagem como gato, cachorro ou pássaro → 3 neurônios na saída.
* Regressão: normalmente usamos 1 neurônio com ativação linear, produzindo um valor contínuo. Ex.: prever preço de uma casa.
* Regressão multivariada: podemos ter vários neurônios na saída, um para cada variável que queremos prever.

--- 

### 12. Quais as premissas do modelo?

As principais condições e considerações são:

* Independência das observações: idealmente, as observações de treino devem ser independentes quando o problema pressupõe isso. Em dados temporais, por exemplo, essa premissa não é necessariamente válida e a arquitetura e a estratégia de validação precisam considerar a dependência temporal.
* Dados representativos: o conjunto de treinamento deve representar razoavelmente os dados que o modelo encontrará em produção. Se houver uma mudança significativa na distribuição, a performance pode cair.
* Variáveis em escala adequada: redes neurais são sensíveis à escala das variáveis, principalmente porque utilizam otimização baseada em gradiente. Por isso, normalmente é recomendável normalizar ou padronizar as features numéricas.
* Relação aprendível: não é necessário assumir que a relação entre as variáveis é linear. Com funções de ativação não lineares e uma arquitetura adequada, a rede consegue aprender relações bastante complexas.
* Dados suficientemente informativos: a rede não consegue aprender uma relação que não esteja presente ou não seja identificável nos dados. Se as features não tiverem informação sobre o target, aumentar a complexidade da rede não resolve o problema.

---

### 13. Como o modelo lida com outliers?

Redes neurais podem ser sensíveis a outliers, principalmente porque o treinamento é baseado na minimização de uma função de perda e no cálculo de gradientes. Um valor extremo pode gerar um erro elevado e influenciar significativamente a atualização dos pesos. O impacto depende também da função de perda: por exemplo, o MSE é mais sensível a valores extremos, enquanto MAE e Huber Loss são mais robustos. Por isso, os outliers devem ser investigados para entender se são erros ou observações legítimas e, dependendo do caso, podem ser tratados por remoção, transformação ou pelo uso de uma função de perda mais robusta.

---

### 14. Como o modelo lida com missings?

Redes neurais não lidam diretamente com valores ausentes, porque as operações matemáticas da rede precisam de entradas numéricas válidas. Portanto, os missings normalmente são tratados antes do treinamento, por exemplo, através de imputação por média ou mediana para variáveis numéricas e moda ou uma categoria específica para variáveis categóricas. Também podemos criar uma variável indicadora para informar ao modelo que aquele valor originalmente estava ausente. É importante que os parâmetros utilizados na imputação sejam aprendidos apenas no conjunto de treinamento, evitando data leakage.

---

### 15. O modelo necessida de pré processamento de dados?

Sim. Redes neurais geralmente necessitam de pré-processamento. É comum tratar valores ausentes, transformar variáveis categóricas em representações numéricas e principalmente colocar as variáveis numéricas em escalas adequadas, por meio de normalização ou padronização, para facilitar a otimização baseada em gradientes. Dependendo do problema, também podemos tratar outliers e realizar transformações específicas dos dados. O pré-processamento exato depende do tipo de dado e da arquitetura utilizada.

---

### 16. Existem regularizadores?

Sim. Redes neurais possuem diversas técnicas de regularização, utilizadas principalmente para reduzir overfitting e melhorar a capacidade de generalização do modelo.

As principais são:

* L1 (Lasso): adiciona uma penalização proporcional ao valor absoluto dos pesos. Pode levar alguns pesos a zero, promovendo uma espécie de seleção de variáveis.
* L2 (Ridge/Weight Decay): penaliza o quadrado dos pesos, incentivando pesos menores e reduzindo a complexidade do modelo.
* Dropout: durante o treinamento, desativa aleatoriamente uma proporção dos neurônios em cada iteração. Isso impede que a rede dependa excessivamente de determinados neurônios.
* Early Stopping: interrompe o treinamento quando o desempenho no conjunto de validação deixa de melhorar, evitando que a rede continue se ajustando excessivamente aos dados de treinamento.
* Data augmentation: aumenta artificialmente a diversidade dos dados de treinamento, principalmente em problemas de imagem, texto ou áudio.
* Batch normalization: embora seja principalmente uma técnica de normalização, também pode proporcionar um efeito regularizador em determinadas situações.

---

### 17. Qual a relação entre viés e variância nesse modelo?

Redes neurais também apresentam o trade-off entre viés e variância. Uma rede com capacidade insuficiente pode ter alto viés e apresentar underfitting, enquanto uma rede muito complexa pode apresentar alta variância e overfitting. Para reduzir o viés, podemos aumentar a capacidade da rede ou diminuir a regularização. Para reduzir a variância, podemos utilizar técnicas como Dropout, L1 ou L2, Early Stopping e Data Augmentation. O objetivo é encontrar uma capacidade de modelo que consiga aprender os padrões dos dados sem se ajustar excessivamente ao conjunto de treinamento.

---

### 18. Quais as vantagens e desvantagens do modelo?

As principais vantagens das redes neurais são a capacidade de aprender relações complexas e não lineares, trabalhar com diferentes tipos de dados e aumentar sua capacidade de representação conforme a arquitetura é ampliada. Além disso, elas conseguem aprender automaticamente representações relevantes das features, reduzindo a necessidade de engenharia manual de atributos em alguns problemas. Como desvantagens, podemos citar o alto custo computacional em arquiteturas complexas, a necessidade de uma quantidade adequada de dados, a grande quantidade de hiperparâmetros para ajustar, o risco de overfitting e a menor interpretabilidade em comparação com modelos mais simples.

---

### 18. O que é Vanishing e exploding gradients?

Vanishing e exploding gradients são problemas que podem ocorrer durante o backpropagation em redes neurais profundas. No vanishing gradient, os gradientes ficam muito pequenos ao serem propagados pelas camadas, fazendo com que as primeiras camadas recebam atualizações muito pequenas e tenham dificuldade para aprender. No exploding gradient, os gradientes ficam muito grandes, fazendo com que as atualizações dos pesos sejam excessivas e tornando o treinamento instável. Para lidar com esses problemas podemos utilizar inicialização adequada dos pesos, funções de ativação apropriadas, técnicas de normalização e, no caso de exploding gradients, gradient clipping. Arquiteturas como conexões residuais e LSTM também ajudam a melhorar o fluxo dos gradientes.

---

### 20. Quais as arquiteturas de redes neurais?

As principais arquiteturas de redes neurais são diferenciadas principalmente pela forma como os neurônios são conectados e pelo tipo de dado que elas conseguem representar. A MLP (Multi-Layer Perceptron) é a arquitetura mais tradicional, formada por camadas densamente conectadas, sendo bastante utilizada em problemas de classificação e regressão, especialmente com dados tabulares. As CNNs (Convolutional Neural Networks) utilizam operações de convolução para identificar padrões locais e são muito utilizadas em imagens e outros dados com estrutura espacial. As RNNs (Recurrent Neural Networks) foram desenvolvidas para trabalhar com dados sequenciais, utilizando informações de etapas anteriores da sequência, sendo aplicáveis a séries temporais e textos. Como variantes das RNNs, temos as LSTMs (Long Short-Term Memory) e as GRUs (Gated Recurrent Units), que utilizam mecanismos de gates para controlar o fluxo de informações e lidar melhor com dependências de longo prazo. Também existem os Autoencoders, formados por um encoder e um decoder, utilizados para aprender representações comprimidas dos dados, redução de dimensionalidade e detecção de anomalias, e as GANs (Generative Adversarial Networks), compostas por um gerador e um discriminador que competem durante o treinamento para produzir novos dados semelhantes aos reais. Por fim, temos os Transformers, que utilizam mecanismos de attention, especialmente self-attention, para capturar relações entre diferentes partes de uma sequência e são amplamente utilizados atualmente em processamento de linguagem natural, visão computacional e modelos multimodais.

---

### 21. O que são redes neurais profundas?

Redes neurais profundas (Deep Neural Networks) são redes neurais que possuem múltiplas camadas ocultas entre a camada de entrada e a camada de saída. A ideia de “profunda” está justamente relacionada à profundidade da rede, ou seja, à quantidade de camadas que realizam transformações sucessivas nos dados.

A principal vantagem dessa profundidade é que a rede consegue aprender representações hierárquicas. Por exemplo, em uma rede profunda para reconhecimento de imagens, as primeiras camadas podem aprender padrões simples, como bordas, camadas intermediárias podem combinar esses padrões para identificar formas e as camadas mais profundas podem representar estruturas mais complexas, como partes de objetos.

Isso permite que redes profundas aprendam relações muito complexas sem que todas as características precisem ser definidas manualmente. Por outro lado, quanto maior a rede, maior tende a ser a quantidade de parâmetros, o custo computacional e o risco de overfitting, exigindo técnicas como regularização e uma quantidade adequada de dados.

* Rede neural rasa (shallow): geralmente possui uma ou poucas camadas ocultas.
* Rede neural profunda (deep): possui múltiplas camadas ocultas, sendo a profundidade o principal critério.

---

### 22. No deep learning valem os mesmos conceitos? ou existem outras arquiteturas?

Os conceitos fundamentais de redes neurais continuam válidos no Deep Learning, como neurônios, pesos, funções de ativação, forward propagation, backpropagation, função de perda e otimização. A principal diferença é que o Deep Learning utiliza redes com maior profundidade e arquiteturas especializadas para diferentes tipos de dados. Por exemplo, MLPs para dados mais estruturados, CNNs para dados espaciais como imagens, RNNs, LSTMs e GRUs para sequências e Transformers para problemas que utilizam mecanismos de atenção. Portanto, os fundamentos permanecem, mas surgem arquiteturas e componentes específicos para aumentar a capacidade da rede em diferentes problemas.

Por que precisamos de funções de ativação não lineares?
O que é uma função de perda?
O que o modelo está tentando otimizar?
O que caracteriza uma rede neural profunda?
Como evitar overfitting em redes neurais?
O que são dropout, batch normalization e early stopping?