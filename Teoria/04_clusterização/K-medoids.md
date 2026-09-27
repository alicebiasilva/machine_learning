# K-medoids 

### 1. O que é K-medoids e qual é o seu objetivo?

K-medoids é um algoritmo de clusterização não supervisionada que divide os dados em K grupos, mas, diferentemente do K-means, representa cada cluster por um medoid, que é uma observação real da base. O objetivo é escolher os K medoides e atribuir cada observação ao medoid mais próximo, minimizando a soma das distâncias dentro dos clusters. Como não utiliza a média para representar os grupos, ele tende a ser mais robusto a outliers e permite utilizar diferentes métricas de distância.

### 2. Como funciona o algoritmo K-medoids passo a passo?


Como o algoritmo escolhe os medoids iniciais?
Como uma observação é atribuída a um determinado cluster?
Qual função objetivo o K-medoids procura minimizar?
Por que o K-medoids tende a ser mais robusto a outliers que o K-means?
Como a escolha da métrica de distância influencia o K-medoids?
O K-medoids exige que utilizemos distância euclidiana?
Como escolher o número de clusters K?
Podemos utilizar silhouette score para avaliar o K-medoids? Como?
Quais são as principais diferenças entre K-means e K-medoids?
Em quais situações você escolheria K-medoids em vez de K-means?
Quais são as principais limitações computacionais do K-medoids?
Como o algoritmo se comporta quando temos um número muito grande de observações?
Como o K-medoids pode ser utilizado com dados em que a média não possui uma interpretação adequada?
Quais são as principais vantagens e desvantagens do K-medoids?
Imagine que você precisa segmentar clientes, mas existem muitos outliers e a distância entre clientes não é necessariamente euclidiana. Como avaliaria se K-medoids seria uma alternativa adequada ao K-means?