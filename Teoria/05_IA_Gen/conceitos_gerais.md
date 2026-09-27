# IA Generativa

### 1. O que é IA generativa e qual a diferença para Machine Learning tradicional?

IA generativa é uma área da inteligência artificial que utiliza modelos de Machine Learning, principalmente Deep Learning, para aprender padrões e distribuições presentes nos dados e gerar novos conteúdos, como texto, imagem, áudio ou código. A principal diferença para o Machine Learning tradicional está no objetivo: no ML tradicional, geralmente treinamos o modelo para prever um valor, classificar uma observação ou tomar uma decisão, enquanto na IA generativa buscamos aprender a estrutura dos dados para produzir novas amostras semelhantes às observadas durante o treinamento. Portanto, IA generativa não é algo separado de Machine Learning, mas uma aplicação de técnicas de ML voltada à geração de novos conteúdos.

---

### 2. O que é um modelo de linguagem e como ele aprende a prever o próximo token?

Um modelo de linguagem é um modelo de Machine Learning treinado para aprender padrões e relações estatísticas em sequências de texto. No caso de modelos autoregressivos, como os LLMs, o treinamento consiste em fornecer uma sequência de tokens e fazer o modelo prever qual é o próximo token, considerando os tokens anteriores. Por exemplo, dado ‘O céu está’, o modelo calcula uma distribuição de probabilidade sobre os possíveis próximos tokens, como ‘azul’. Durante o treinamento, essa previsão é comparada com o token correto usando uma função de perda, normalmente a cross-entropy, e os pesos do modelo são ajustados por backpropagation e otimização por gradiente. Repetindo esse processo em grandes volumes de texto, o modelo aprende padrões de linguagem, contexto, relações entre palavras e estruturas mais complexas, permitindo posteriormente gerar texto token por token.

---

### 3. O que é um Transformer e por que o mecanismo de Attention é tão importante?

O Transformer é uma arquitetura de redes neurais criada para lidar principalmente com dados sequenciais e que tem como principal componente o mecanismo de Attention. O Attention permite que o modelo, ao processar um token, avalie quais outros tokens da sequência são mais relevantes para aquele contexto, atribuindo diferentes pesos de importância a eles. Isso é importante porque permite capturar dependências entre elementos mesmo quando estão muito distantes na sequência, sem precisar processá-los necessariamente de forma sequencial como acontecia em arquiteturas recorrentes, como RNNs e LSTMs. No Transformer, isso é feito principalmente por meio do mecanismo de Self-Attention, em que cada token pode se relacionar com os demais tokens da própria sequência. Essa arquitetura também permite maior paralelização durante o treinamento e escala muito bem para grandes volumes de dados, sendo a base de modelos como BERT e os LLMs modernos.

---

### 4. Qual a diferença entre encoder, decoder e encoder-decoder?

Encoder e decoder são componentes do Transformer que têm funções diferentes. O encoder recebe a sequência de entrada e transforma os tokens em representações contextualizadas, permitindo que cada token incorpore informações dos demais tokens da sequência. Ele é muito utilizado em tarefas de compreensão, como classificação e representação de texto; o BERT, por exemplo, é baseado em encoder. O decoder, por outro lado, é projetado para gerar uma sequência de saída, normalmente de forma autoregressiva, usando o que já foi gerado para prever o próximo token. Modelos como GPT utilizam essa arquitetura de decoder. Já uma arquitetura encoder-decoder combina os dois: o encoder processa e representa a entrada, e o decoder utiliza essa representação para gerar uma saída. Isso é comum em tarefas de transformação de uma sequência em outra, como tradução automática, sumarização e conversão de texto. Em resumo: o encoder é focado em representar a entrada, o decoder em gerar a saída, e o encoder-decoder conecta os dois para transformar uma sequência em outra..

---

### 5. O que são tokens, embeddings e positional encoding?

Tokens, embeddings e positional encoding são três conceitos fundamentais para entender como um Transformer processa texto. Tokens são as unidades em que o texto é dividido antes de entrar no modelo. Um token pode ser uma palavra inteira, parte de uma palavra, um caractere ou até um símbolo, dependendo do tokenizer. Depois, cada token é convertido em um embedding, que é um vetor numérico de dimensão fixa que representa características semânticas e linguísticas aprendidas pelo modelo. Tokens com relações semânticas semelhantes tendem a apresentar representações vetoriais semelhantes. O problema é que o embedding, por si só, não informa a posição do token na sequência. Por isso existe o positional encoding, que adiciona ao modelo informações sobre a posição de cada token, permitindo diferenciar, por exemplo, ‘o cachorro mordeu o homem’ de ‘o homem mordeu o cachorro’. Em resumo, o token representa a unidade de texto, o embedding transforma essa unidade em uma representação numérica e o positional encoding adiciona a informação de ordem e posição na sequência.

---

### 6. O que é pré-treinamento e o que acontece no fine-tuning?

Pré-treinamento é a etapa em que o modelo aprende padrões gerais a partir de uma grande quantidade de dados, normalmente sem depender de uma tarefa específica. Em um modelo de linguagem, por exemplo, ele pode ser treinado para prever o próximo token, ajustando seus pesos por meio da função de perda e backpropagation. O resultado é um modelo que aprendeu representações gerais de linguagem, mas ainda não necessariamente está otimizado para uma aplicação específica. No fine-tuning, partimos desse modelo pré-treinado e continuamos o treinamento utilizando um conjunto de dados mais específico, geralmente relacionado à tarefa ou ao comportamento que queremos obter. Os pesos do modelo são ajustados para adaptar o conhecimento geral aprendido durante o pré-treinamento ao novo objetivo. Por exemplo, podemos fazer fine-tuning de um modelo de linguagem com dados de atendimento ao cliente para adaptá-lo a esse domínio. Portanto, o pré-treinamento aprende conhecimento e padrões gerais, enquanto o fine-tuning adapta esse modelo para uma tarefa, domínio ou comportamento específico.

---

### 7. Qual a diferença entre fine-tuning, prompting e in-context learning?

A principal diferença está em como adaptamos o modelo para realizar uma tarefa. No fine-tuning, modificamos os pesos do modelo por meio de um novo processo de treinamento, utilizando dados específicos da tarefa ou do domínio. Já no prompting, não alteramos os pesos do modelo: fornecemos instruções no prompt para orientar o comportamento do modelo durante a inferência. O in-context learning é uma forma de adaptação em que fornecemos exemplos diretamente no contexto do prompt, mostrando ao modelo como determinada tarefa deve ser realizada, sem atualizar seus pesos. Por exemplo, podemos fornecer alguns pares de entrada e resposta e pedir que o modelo faça uma nova previsão seguindo aquele padrão. Portanto, no fine-tuning há atualização dos parâmetros do modelo; no prompting há apenas instruções; e no in-context learning fornecemos exemplos no contexto para que o modelo identifique o padrão da tarefa durante aquela interação.

---

### 8. O que são temperatura, top-k e top-p e como afetam a geração?

Temperatura, top-k e top-p são parâmetros usados durante a geração para controlar quais tokens podem ser escolhidos pelo modelo e o grau de aleatoriedade da resposta. A temperatura modifica a distribuição de probabilidades dos próximos tokens: temperaturas menores tornam a distribuição mais concentrada, fazendo o modelo favorecer os tokens mais prováveis e gerar respostas mais determinísticas; temperaturas maiores tornam a distribuição mais uniforme, aumentando a diversidade e a aleatoriedade. O top-k limita a escolha aos \(K\) tokens com maior probabilidade. Por exemplo, com \(k=10\), o modelo só pode escolher entre os dez tokens mais prováveis. Já o top-p, também chamado de nucleus sampling, seleciona o menor conjunto de tokens cuja probabilidade acumulada seja pelo menos \(p\). Assim, diferentemente do top-k, a quantidade de candidatos pode variar de acordo com a distribuição de probabilidades. Portanto, temperatura controla a aleatoriedade da distribuição, enquanto top-k e top-p restringem o conjunto de candidatos que podem ser amostrados. Esses parâmetros atuam na etapa de geração, não alterando os pesos do modelo.

---

### 9. O que são hallucinations e por que elas acontecem?

Hallucinations são situações em que um modelo de IA generativa produz uma informação que parece plausível e é apresentada com confiança, mas que é incorreta, inventada ou não pode ser sustentada pelos dados disponíveis. Elas acontecem porque um modelo de linguagem é treinado principalmente para aprender padrões e estimar a probabilidade do próximo token, e não para verificar se cada afirmação gerada corresponde necessariamente a um fato. Assim, quando o modelo não possui informação suficiente, o contexto é ambíguo ou a distribuição de probabilidades favorece uma sequência plausível, ele pode gerar uma resposta coerente, mas factualmente incorreta. A temperatura e outros parâmetros de amostragem também podem aumentar a variabilidade da geração, mas não são a causa fundamental das hallucinations. Para reduzir esse problema, podemos utilizar técnicas como RAG, que fornece ao modelo informações externas recuperadas de fontes confiáveis, grounding, validação das respostas, uso de ferramentas externas e prompts que orientem o modelo a declarar quando não possui informação suficiente.

---

### 10. O que é RAG e qual problema ele resolve?
### 11. Qual a diferença entre RAG e fine-tuning?
### 12. Como avaliar um sistema baseado em LLM?
### 13. O que são embeddings e como são utilizados em busca semântica?
### 14. O que é uma vector database e por que ela é utilizada em aplicações de GenAI?
### 15. O que são agentes de IA e como eles diferem de um chatbot tradicional?