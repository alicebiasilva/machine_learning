# Funções 


## Características e propriedades das funções


Uma função pode ser classificada de acordo com o seu **comportamento**. Essas classificações não dependem necessariamente da fórmula da função, mas de propriedades relacionadas à forma como ela associa elementos, à simetria do seu gráfico, ao seu crescimento, à sua continuidade e a outras características matemáticas.

Essas propriedades são importantes porque permitem analisar uma função sem precisar olhar apenas para sua fórmula. Em Ciência de Dados, Estatística e Machine Learning, conceitos como crescimento, continuidade, diferenciabilidade, convexidade e funções limitadas aparecem frequentemente na análise de modelos matemáticos e algoritmos de otimização.

---

## 1. Funções injetoras, sobrejetoras e bijetoras

Considere uma função:

$$
f:A\rightarrow B
$$

em que `A` é o domínio e `B` é o contradomínio.

### Função injetora

Uma função é **injetora** quando elementos diferentes do domínio nunca produzem o mesmo resultado.

Ou seja:

$$
x_1 \neq x_2
\Rightarrow
f(x_1) \neq f(x_2)
$$

Uma forma equivalente de escrever isso é:

$$
f(x_1)=f(x_2)
\Rightarrow
x_1=x_2
$$

Intuitivamente, uma função injetora não "junta" dois elementos diferentes do domínio no mesmo resultado.

Por exemplo:

$$
f(x)=2x
$$

é uma função injetora quando consideramos:

$$
f:\mathbb{R}\rightarrow\mathbb{R}
$$

Valores diferentes de `x` sempre produzem valores diferentes de `f(x)`.

### Função sobrejetora

Uma função é **sobrejetora** quando todo elemento do contradomínio é atingido por pelo menos um elemento do domínio.

Ou seja, para todo:

$$
y\in B
$$

existe pelo menos um:

$$
x\in A
$$

tal que:

$$
f(x)=y
$$

A diferença fundamental é:

- **Injetora:** não existem duas entradas diferentes produzindo a mesma saída.
- **Sobrejetora:** nenhuma saída do contradomínio fica sem ser atingida.

### Função bijetora

Uma função é **bijetora** quando é simultaneamente **injetora e sobrejetora**.

Nesse caso, existe uma correspondência de "um para um" entre os elementos do domínio e do contradomínio.

As funções bijetoras possuem uma **função inversa**:

$$
f^{-1}
$$

A função inversa desfaz a transformação realizada pela função original.

### Referência visual

[Diagrama de funções injetoras, sobrejetoras e bijetoras — Wikimedia Commons](https://commons.wikimedia.org/wiki/File%3AInjective%2C_Surjective%2C_Bijective.svg)

---

## 2. Funções pares e ímpares

As funções também podem ser classificadas de acordo com a **simetria de seus gráficos**.

### Função par

Uma função é **par** quando:

$$
f(-x)=f(x)
$$

para todos os valores de `x` pertencentes ao domínio.

Geometricamente, isso significa que o gráfico da função é simétrico em relação ao **eixo y**.

Um exemplo clássico é:

$$
f(x)=x^2
$$

Pois:

$$
f(-x)=(-x)^2=x^2=f(x)
$$

Portanto, `f(x)=x²` é uma função par.

### Função ímpar

Uma função é **ímpar** quando:

$$
f(-x)=-f(x)
$$

Nesse caso, o gráfico possui simetria em relação à **origem** do sistema de coordenadas.

Um exemplo é:

$$
f(x)=x^3
$$

pois:

$$
f(-x)=(-x)^3=-x^3=-f(x)
$$

Portanto, `f(x)=x³` é uma função ímpar.

Uma função pode ainda não ser nem par nem ímpar.

### Referência

[Matemática Essencial — Funções — UEL](https://www.uel.br/projetos/matessencial/basico/medio/funcoes.html)

---

## 3. Funções crescentes e decrescentes

Outra maneira de classificar uma função é observar como seus valores se comportam quando `x` aumenta.

### Função crescente

Uma função é **crescente** em determinado intervalo quando, à medida que `x` aumenta, os valores de `f(x)` não diminuem.

Podemos representar isso por:

$$
x_1<x_2
\Rightarrow
f(x_1)\leq f(x_2)
$$

Se os valores sempre aumentarem, temos uma função **estritamente crescente**:

$$
x_1<x_2
\Rightarrow
f(x_1)<f(x_2)
$$

### Função decrescente

Uma função é **decrescente** quando, à medida que `x` aumenta, os valores de `f(x)` não aumentam:

$$
x_1<x_2
\Rightarrow
f(x_1)\geq f(x_2)
$$

Se os valores sempre diminuírem, temos uma função **estritamente decrescente**:

$$
x_1<x_2
\Rightarrow
f(x_1)>f(x_2)
$$

Uma função também pode ser crescente em uma região e decrescente em outra.

Por exemplo:

$$
f(x)=x^2
$$

é decrescente para:

$$
x<0
$$

e crescente para:

$$
x>0
$$

Essas propriedades são especialmente importantes quando começamos a estudar **derivadas**, pois a derivada pode ajudar a determinar onde uma função está crescendo ou diminuindo.

### Referência

[Preparatory Mathematics — Properties of Functions](https://courses.fit.cvut.cz/BIE-PKM/%40master/textbook/sec-funkce.html)

---

## 4. Funções contínuas e descontínuas

Uma função é **contínua** quando, de maneira intuitiva, seu gráfico não apresenta "quebras", "saltos" ou interrupções no ponto analisado.

Uma forma simples de imaginar é pensar que podemos desenhar o gráfico da função sem tirar o lápis do papel.

Por exemplo:

$$
f(x)=x^2
$$

é contínua para todos os números reais.

Já:

$$
f(x)=\frac{1}{x}
$$

não é contínua em:

$$
x=0
$$

pois a função não está definida nesse ponto.

Formalmente, uma função `f` é contínua em um ponto `a` quando:

$$
\lim_{x\rightarrow a}f(x)=f(a)
$$

Isso significa que o limite da função quando `x` se aproxima de `a` deve existir e ser igual ao valor da função naquele ponto.

Uma função pode ser contínua em alguns pontos e descontínua em outros.

### Tipos de descontinuidade

Alguns tipos comuns são:

- Descontinuidade removível
- Descontinuidade de salto
- Descontinuidade infinita
- Descontinuidade oscilatória

A continuidade é fundamental para o estudo de:

- Limites
- Derivadas
- Integrais
- Análise matemática

---

## 5. Funções diferenciáveis

Uma função é **diferenciável** em um ponto quando podemos calcular sua derivada nesse ponto.

A derivada mede a **taxa de variação instantânea** da função.

Geometricamente, ela está relacionada à inclinação da reta tangente ao gráfico.

A derivada pode ser representada por:

$$
f'(x)
$$

Por exemplo:

$$
f(x)=x^2
$$

possui derivada:

$$
f'(x)=2x
$$

Isso permite saber a inclinação da função em diferentes pontos.

A diferenciabilidade está intimamente relacionada à continuidade.

Se uma função é diferenciável em um ponto, então ela é necessariamente contínua nesse ponto.

Porém, o contrário não é necessariamente verdadeiro.

Uma função pode ser contínua e não ser diferenciável.

Um exemplo clássico é:

$$
f(x)=|x|
$$

Essa função é contínua em:

$$
x=0
$$

mas não é diferenciável nesse ponto porque possui uma "quina".

A diferenciabilidade é especialmente importante para compreender:

- Derivadas
- Derivadas parciais
- Gradientes
- Otimização
- Gradient Descent
- Machine Learning

---

## 6. Funções convexas e côncavas

A **convexidade** é uma propriedade especialmente importante em otimização e Machine Learning.

De maneira intuitiva, uma função convexa possui um formato semelhante a uma "tigela".

Um exemplo simples é:

$$
f(x)=x^2
$$

Uma definição matemática de convexidade é:

$$
f(\lambda x+(1-\lambda)y)
\leq
\lambda f(x)+(1-\lambda)f(y)
$$

para:

$$
0\leq\lambda\leq1
$$

Uma maneira visual de entender essa propriedade é imaginar dois pontos quaisquer sobre o gráfico.

O segmento de reta que conecta esses pontos fica **acima ou sobre o gráfico** da função.

### Função côncava

Uma função é **côncava** quando apresenta o comportamento oposto.

Um exemplo é:

$$
f(x)=-x^2
$$

Nesse caso, o segmento de reta entre dois pontos do gráfico tende a ficar **abaixo ou sobre o gráfico**.

A convexidade é importante porque problemas de otimização envolvendo funções convexas possuem propriedades que podem facilitar a busca pelo mínimo global.

Isso faz com que conceitos como:

- Convexidade
- Derivadas
- Gradientes
- Funções de custo
- Otimização

sejam importantes para compreender como algoritmos de Machine Learning encontram parâmetros que minimizam funções de custo.

---

## 7. Funções periódicas

Uma função é **periódica** quando seu comportamento se repete em intervalos regulares.

Uma função `f` é periódica quando existe um número positivo `T` tal que:

$$
f(x+T)=f(x)
$$

para todos os valores de `x` para os quais a expressão esteja definida.

O número `T` é chamado de **período** da função.

Um exemplo clássico é:

$$
f(x)=\sin(x)
$$

Para essa função:

$$
\sin(x+2\pi)=\sin(x)
$$

Portanto, seu período fundamental é:

$$
T=2\pi
$$

Funções periódicas aparecem naturalmente em fenômenos que se repetem, como:

- Ondas
- Oscilações
- Ciclos
- Sinais
- Fenômenos físicos

Além do seno e do cosseno, diversas outras funções podem apresentar comportamento periódico.

---

## 8. Funções limitadas e ilimitadas

Uma função é **limitada superiormente** quando existe um número `M` tal que:

$$
f(x)\leq M
$$

para todos os valores de `x` do domínio.

Ela é **limitada inferiormente** quando existe um número `m` tal que:

$$
f(x)\geq m
$$

para todos os valores de `x`.

Uma função é chamada simplesmente de **limitada** quando é limitada tanto superior quanto inferiormente.

Nesse caso, existe algum número positivo `K` tal que:

$$
|f(x)|\leq K
$$

para todo `x` do domínio.

Por exemplo:

$$
f(x)=\sin(x)
$$

é limitada porque:

$$
-1\leq\sin(x)\leq1
$$

Já:

$$
f(x)=x^2
$$

não é limitada superiormente nos números reais, pois podemos escolher valores de `x` cada vez maiores e obter valores de `f(x)` cada vez maiores.

Entretanto, `x²` é limitada inferiormente por `0`:

$$
x^2\geq0
$$

O conceito de limitação é importante no estudo de:

- Limites
- Sequências
- Séries
- Probabilidade
- Estatística
- Modelos matemáticos

---

## 9. Funções reais

Uma **função real** é uma função que trabalha com números reais.

Um exemplo é:

$$
f:\mathbb{R}\rightarrow\mathbb{R}
$$

Isso significa que a função recebe números reais e produz números reais.

Por exemplo:

$$
f(x)=x^2
$$

é uma função de `R` em `R`.

Funções reais são extremamente importantes na matemática aplicada porque muitas grandezas do mundo real podem ser representadas por números reais.

Em Ciência de Dados, é muito comum trabalhar com funções como:

$$
f(x)=wx+b
$$

ou funções com diversas variáveis:

$$
f(x_1,x_2,\ldots,x_n)
$$

Essas funções podem representar modelos que recebem características dos dados e produzem previsões.

---

## 10. Funções complexas

Os **números complexos** ampliam o conjunto dos números reais.

Um número complexo pode ser escrito como:

$$
z=a+bi
$$

onde `a` e `b` são números reais e:

$$
i^2=-1
$$

Uma função complexa pode receber e produzir números complexos:

$$
f:\mathbb{C}\rightarrow\mathbb{C}
$$

Um exemplo simples é:

$$
f(z)=z^2
$$

As funções complexas são estudadas principalmente na área chamada **Análise Complexa**.

Elas aparecem em áreas como:

- Física
- Engenharia
- Processamento de sinais
- Equações diferenciais
- Matemática aplicada

Para Ciência de Dados, funções complexas normalmente não são uma prioridade inicial. Porém, conhecer a diferença entre funções reais e complexas ajuda a compreender que funções podem operar sobre diferentes conjuntos numéricos.

---

# Resumo das propriedades

| Propriedade | Ideia principal |
|---|---|
| **Injetora** | Entradas diferentes produzem saídas diferentes |
| **Sobrejetora** | Todo elemento do contradomínio é atingido |
| **Bijetora** | É simultaneamente injetora e sobrejetora |
| **Par** | Simetria em relação ao eixo `y` |
| **Ímpar** | Simetria em relação à origem |
| **Crescente** | Os valores aumentam conforme `x` aumenta |
| **Decrescente** | Os valores diminuem conforme `x` aumenta |
| **Contínua** | Não apresenta ruptura no ponto analisado |
| **Diferenciável** | Possui derivada no ponto analisado |
| **Convexa** | Possui comportamento de "tigela" |
| **Côncava** | Possui comportamento oposto ao de uma função convexa |
| **Periódica** | Seu comportamento se repete após determinado período |
| **Limitada** | Seus valores permanecem dentro de determinados limites |
| **Real** | Trabalha com números reais |
| **Complexa** | Trabalha com números complexos |

---

 **Importante**: as propriedades podem coexistir

Essas propriedades **não são mutuamente exclusivas**.

Uma mesma função pode possuir várias características simultaneamente.

Por exemplo:

$$
f(x)=x
$$

é:

- Real
- Ímpar
- Contínua
- Diferenciável
- Estritamente crescente
- Bijetora, considerando:

$$
f:\mathbb{R}\rightarrow\mathbb{R}
$$

Já:

$$
f(x)=x^2
$$

é:

- Real
- Par
- Contínua
- Diferenciável
- Convexa
- Limitada inferiormente

Porém, ela **não é injetora** em `R`, pois:

$$
f(-1)=f(1)=1
$$

Isso demonstra por que é importante analisar diferentes propriedades de uma mesma função.

---

# Funções Algébricas

As **funções algébricas** são funções que podem ser construídas utilizando operações algébricas, como soma, subtração, multiplicação, divisão, potenciação e radiciação.

Elas formam uma parte fundamental do estudo de funções e servem como base para assuntos posteriores, como limites, derivadas, integrais, álgebra linear, estatística e otimização.

As principais funções algébricas estudadas neste capítulo são:

- Função constante
- Função identidade
- Função linear
- Função afim
- Função quadrática
- Função cúbica
- Função polinomial
- Função racional
- Função radical
- Função potência

Uma das melhores maneiras de compreender uma função é estudar tanto sua **expressão matemática** quanto seu **gráfico**. O gráfico permite visualizar propriedades como crescimento, decrescimento, raízes, pontos de máximo e mínimo, concavidade, simetria e comportamento no infinito.

> **Ferramenta recomendada:** [Desmos — Calculadora Gráfica](https://www.desmos.com/calculator)

O Desmos permite inserir expressões matemáticas, construir gráficos e alterar parâmetros das funções de forma interativa.

<br>

## 1. Função constante

A **função constante** é uma função cujo valor de saída permanece sempre igual, independentemente do valor de entrada.

Sua forma geral é:

$$
f(x)=c
$$

onde `c` é uma constante.

Por exemplo:

$$
f(x)=5
$$

Isso significa que qualquer valor de `x` produzirá o resultado `5`:

$$
f(-10)=5
$$

$$
f(0)=5
$$

$$
f(2)=5
$$

$$
f(100)=5
$$

Portanto, a função não depende do valor de `x` para determinar seu resultado.

## Gráfico

O gráfico de uma função constante é uma **reta horizontal**.

Por exemplo:

$$
f(x)=3
$$

gera uma reta paralela ao eixo `x`, passando pelo ponto `(0,3)`.

<br>

## 2. Função identidade

A **função identidade** é uma função que associa cada elemento do domínio a ele mesmo.

Sua forma geral é:

$$
f(x)=x
$$

Ou seja, o valor de saída é sempre igual ao valor de entrada.

Por exemplo:

$$
f(2)=2
$$

$$
f(-5)=-5
$$

$$
f(0)=0
$$

$$
f(10)=10
$$

## Gráfico

O gráfico da função identidade é uma **reta que passa pela origem** $(0,0)$ e possui inclinação igual a `1`. Ou seja, corta os quadrantes 1 e 3 criando uma reta diagonal.

- Função linear
- Função afim
- Função quadrática
- Função cúbica
- Função polinomial 
- Função racional 
- Função radical
- Função potência

### Exponenciais e logarítmicas

- Exponencial
- Exponencial natural
- Logarítmica
- Natural

### Trigonométricas

- Seno
- Cosseno
- Tangente
- Cotangente
- Secante
- Cossecante

E as suas inversas:

- Arco seno
- Arco cosseno
- Arco tangente

### Hiperbólicas

- Seno hiperbólico 
- Cosseno hiperbólico 
- Tangente hiperbólica
- Cotangente hiperbólica
- Secante hiperbólica

Também possuem versões inversas.

### Função modular 

### Destaques para Machine Learning 

- Função Sigmoide
- ReLU
- Leaky ReLU
- Tanh
- Softmax
- Função Hinge