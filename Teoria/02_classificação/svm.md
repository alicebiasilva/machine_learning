# Support Vector Machine 

### 1. O que é SVM?

SVM, ou Support Vector Machine, é um algoritmo de aprendizado supervisionado usado principalmente para classificação, embora também exista uma versão para regressão, chamada SVR. 

A ideia central do SVM é encontrar um hiperplano que separe as classes da melhor maneira possível, buscando uma fronteira de decisão com boa capacidade de generalização, controlando o trade-off entre maximizar a margem e penalizar erros de classificação.

intuitivamente, encontrar a separação que maximiza a margem, ou seja, a distância entre o hiperplano e os pontos mais próximos de cada classe. Esses pontos mais próximos são chamados de support vectors e são justamente eles que determinam a posição do hiperplano de decisão.


---

Qual é a intuição geométrica do Support Vector Machine e o que exatamente ele está tentando otimizar?

O que são os support vectors e por que eles são tão importantes para definir o hiperplano de decisão?

O que significa maximizar a margem em um SVM? Qual é a relação entre margem e generalização do modelo?

Explique a diferença entre SVM de margem rígida (hard margin) e margem suave (soft margin). Em que situação você usaria cada um?

Qual é o papel do hiperparâmetro C? O que tende a acontecer quando aumentamos ou diminuímos C?

O que é o kernel trick e qual problema ele resolve? Explique sem simplesmente dizer que ele "transforma os dados para outra dimensão".

Compare os kernels linear, polinomial e RBF. Em que tipo de problema você escolheria cada um?

Qual é o papel do hiperparâmetro gamma no kernel RBF? O que acontece com o modelo quando gamma é muito alto ou muito baixo?

Por que a normalização ou padronização das variáveis é particularmente importante para SVM? Dê um exemplo em que não normalizar poderia prejudicar bastante o modelo.

Como você explicaria matematicamente a função objetivo de um SVM com margem suave? O que representam o termo de regularização e o hinge loss?

Qual é a relação entre C e overfitting/underfitting? E por que seria simplista dizer que "C alto sempre causa overfitting"?

Imagine um dataset com 10 milhões de observações e 500 features. Você escolheria SVM? Se não, quais seriam suas preocupações e alternativas?

Como o SVM se comporta em um problema altamente desbalanceado? Que estratégias você adotaria para lidar com isso?

Como você faria tuning de C e gamma em um SVM com RBF? Que métrica e estratégia de validação utilizaria se o problema fosse de classificação desbalanceada?

Você treinou dois modelos: Logistic Regression e SVM-RBF. O SVM apresentou AUC ligeiramente maior, mas é muito mais lento, menos interpretável e difícil de colocar em produção. Como você decidiria qual modelo levar para produção?