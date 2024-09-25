# sistemas-recomendaca
## Propósito do Projeto
O projeto tem como objetivo desenvolver um sistema de recomendação de filmes que utiliza algoritmos de agrupamento para sugerir filmes com base no histórico de usuários. A ideia central é proporcionar uma experiência personalizada, ajudando os usuários a descobrir novos filmes que se alinhem aos seus gostos e preferências.

##Algoritmo de Agrupamento Utilizado
Para as recomendações, utilizamos o algoritmo K-means, que é eficaz na segmentação de dados em grupos (clusters). Este algoritmo permite identificar padrões no comportamento dos usuários, agrupando aqueles que possuem preferências similares. Com isso, o sistema pode sugerir filmes que outros usuários no mesmo grupo gostaram, aumentando a relevância das recomendaçõesgit


### Sistema de Recomendação

O **Sistema de Recomendação** tem como objetivo ajudar os usuários a descobrir novos filmes com base em suas preferências pessoais. O sistema utiliza um conjunto de dados que inclui informações sobre usuários e filmes, permitindo que ele faça recomendações personalizadas.

### Algoritmo de Agrupamento Utilizado

Para gerar essas recomendações, o projeto emprega um **algoritmo de agrupamento**, especificamente o **K-means**. Esse algoritmo funciona da seguinte maneira:

 **Coleta de Dados**: Primeiramente, o sistema coleta dados sobre usuários e seus filmes preferidos.
 **Pré-processamento**: Os dados são normalizados, o que garante que as comparações sejam justas, independentemente da escala dos dados.
**Agrupamento**: O K-means é aplicado para agrupar usuários com interesses semelhantes. Ele divide os usuários em "k" grupos com base em suas preferências.
**Recomendação**: Com os grupos formados, o sistema pode sugerir filmes que usuários similares já gostaram, aumentando a probabilidade de agradar ao novo usuário.

Esse método proporciona uma abordagem eficiente para oferecer recomendações personalizadas, ajudando os usuários a encontrar filmes que se alinhem aos seus gostos.

Claro! Vamos aprofundar ainda mais a explicação do código, detalhando cada componente e suas interações, além de contextualizar a aplicação do algoritmo de agrupamento KMeans no cenário de recomendação de filmes.

---

### Introdução

O código apresentado utiliza o algoritmo KMeans para agrupar usuários com base nos filmes que assistiram, com o objetivo de fornecer recomendações personalizadas. Essa técnica é uma aplicação comum em sistemas de recomendação, onde a similaridade entre usuários é usada para sugerir conteúdos que eles ainda não experimentaram.

### Importação de Bibliotecas

O primeiro passo no código é a importação das bibliotecas necessárias:

```python
import numpy as np
from sklearn.cluster import KMeans
```

Aqui, **NumPy** é uma biblioteca poderosa para manipulação de arrays e operações matemáticas em Python. Ela é especialmente útil em tarefas científicas e de análise de dados, permitindo a manipulação eficiente de grandes conjuntos de dados. O **KMeans**, por sua vez, é um algoritmo de aprendizado não supervisionado da biblioteca **Scikit-learn**. Ele é projetado para dividir um conjunto de dados em grupos (ou clusters) com base em características comuns.

### Criação do Conjunto de Dados

A próxima etapa do código envolve a criação do conjunto de dados que será usado para o agrupamento:

```python
filmes_assistidos = np.array([
    [1, 0, 0, 1],
    [1, 1, 0, 0],
    [0, 1, 0, 0],
    [0, 0, 1, 1],
    [1, 0, 1, 0],
    [1, 0, 1, 0],
    [1, 0, 1, 0],
    [1, 0, 1, 0],
    [1, 0, 1, 0],
])
```

Neste trecho, criamos um array NumPy onde cada linha representa um usuário e cada coluna representa um filme. Os valores '1' e '0' indicam se o usuário assistiu ao filme correspondente. Por exemplo, o primeiro usuário assistiu ao filme 1 e ao filme 4, mas não assistiu aos filmes 2 e 3.

Esse conjunto de dados é fundamental para o treinamento do modelo, pois fornece informações sobre as preferências de cada usuário. A escolha dos filmes e a forma como os dados são estruturados influenciam diretamente a eficácia do algoritmo de agrupamento.

### Definição do Número de Clusters

O próximo passo é definir quantos grupos o modelo deve identificar:

```python
num_clusters = 2
```

Aqui, estamos especificando que queremos dividir os usuários em dois grupos. A escolha do número de clusters pode ser influenciada por vários fatores, incluindo a natureza dos dados e o que se espera alcançar com a segmentação. É comum testar diferentes números de clusters e usar métodos como o "Elbow Method" para determinar o número ideal.

### Inicialização do Modelo KMeans

Uma vez definido o número de clusters, inicializamos o modelo KMeans:

```python
kmeans = KMeans(n_clusters=num_clusters, n_init=10)
```

Na inicialização, `n_clusters` especifica quantos grupos queremos e `n_init` define quantas vezes o algoritmo será executado com diferentes centroides iniciais. O KMeans pode ser sensível à escolha inicial dos centroides, e múltiplas inicializações ajudam a garantir que o algoritmo encontre uma solução globalmente ótima.

### Treinamento do Modelo

Após a inicialização, o próximo passo é treinar o modelo com os dados de filmes assistidos:

```python
kmeans.fit(filmes_assistidos)
```

Durante o treinamento, o KMeans calcula a posição dos centroides para cada cluster e atribui os usuários aos grupos com base na proximidade desses centroides. O algoritmo repete esse processo até que as posições dos centroides não mudem significativamente ou até que um número máximo de iterações seja alcançado.

### Classificação dos Usuários

Após o treinamento, usamos o modelo para prever a qual grupo cada usuário pertence:

```python
grupos_indice = kmeans.predict(filmes_assistidos)
```

O método `predict` fornece uma lista onde cada índice representa um usuário e o valor correspondente é o grupo ao qual o usuário foi atribuído. Essa informação é crucial, pois permite que personalizemos as recomendações com base nas preferências de usuários semelhantes.

### Exibição dos Grupos dos Usuários

A seguir, o código imprime qual grupo cada usuário pertence:

```python
print("usuario pertence ao seguinte grupo:")
for i, grupo_indice in enumerate(grupos_indice):
    print(f"Usuario {i+1}: Grupo {grupo_indice+1}")
```

Neste trecho, utilizamos um loop para iterar sobre os grupos e exibir de maneira clara a classificação de cada usuário. Essa visualização é útil para verificar se o agrupamento foi realizado de forma coerente.

### Exibição dos Filmes Assistidos

Além de classificar os usuários, o código também imprime quais filmes foram assistidos por cada usuário:

```python
print("\nFilmes assistidos:")
for i in range(len(filmes_assistidos)):
   assistidos = np.where(filmes_assistidos[i] == 1)[0]
   print(f"Usuario {i+1}: {', '.join(map(str, filmes_assistidos[i]))}")
```

Aqui, utilizamos `np.where` para localizar os índices dos filmes assistidos. A saída mostra de maneira clara quais filmes foram assistidos por cada usuário, o que pode ser útil para análises futuras.

### Função de Recomendação de Filmes

Agora, entramos na parte mais interessante do código: a função que recomenda filmes.

```python
def recomendar_filme(filmes, filmes_assistidos, grupo_indice):
```

Essa função recebe como parâmetros um vetor de filmes (o que o usuário assistiu), a matriz de filmes assistidos e os índices dos grupos. O objetivo da função é sugerir novos filmes para um usuário com base nas preferências de outros usuários que pertencem ao mesmo grupo.

### Identificação do Grupo do Usuário

Dentro da função, determinamos a qual grupo o usuário pertence:

```python
usuario_id = len(filmes_assistidos)
grupo_usuario = kmeans.predict([filmes])[0]
```

Aqui, fazemos uma previsão com o vetor de filmes do usuário para identificar seu grupo. Isso permite que a função personalize as recomendações de acordo com as preferências dos usuários semelhantes.

### Identificando Usuários do Mesmo Grupo

Com o grupo do usuário identificado, o próximo passo é encontrar outros usuários que pertencem ao mesmo grupo:

```python
usuarios_no_mesmo_grupo = [i for i in range(len(grupos_indice)) if grupos_indice[i] == grupo_usuario]
```

Criamos uma lista contendo os índices dos usuários que estão no mesmo grupo. Essa informação é crucial para as recomendações, pois será a base para sugerir filmes que o usuário ainda não assistiu.

### Coletando Filmes Recomendados

Agora, coletamos todos os filmes assistidos pelos usuários do mesmo grupo:

```python
filmes_recomendados = set()
for usuario in usuarios_no_mesmo_grupo:
   filmes_assistidos_usuario = np.where(filmes_assistidos[usuario] == 1)[0]
   filmes_recomendados.update(filmes_assistidos_usuario)
```

Utilizamos um conjunto para armazenar os filmes recomendados, o que ajuda a evitar duplicatas. Para cada usuário no grupo, verificamos quais filmes eles assistiram e adicionamos esses filmes ao conjunto.

### Remoção de Filmes Já Assistidos

Depois de coletar os filmes, removemos aqueles que o usuário já assistiu:

```python
filmes_recomendados = filmes_recomendados - set(filmes_assistidos[usuario])
```

Essa etapa é crucial, pois o objetivo é fornecer recomendações de filmes que o usuário ainda não viu. Ao remover os filmes já assistidos, garantimos que as sugestões sejam realmente úteis.

### Ajustando os Índices dos Filmes

A seguir, ajustamos os índices dos filmes recomendados:

```python
filmes_recomendados = [filmes[i] for i in filmes_recomendados]
```

Essa conversão garante que as recomendações sejam apresentadas corretamente, retornando à forma original.

### Retorno da Função

Por fim, a função retorna a lista de filmes recomendados:

```python
return sorted(filmes_recomendados)
```

A lista é ordenada para facilitar a visualização e o usuário pode facilmente identificar quais filmes são sugeridos.

### Exemplo de Uso da Função

Para ilustrar o uso da função, temos o seguinte trecho:

```python
filmes_assistidos_usuario = [1, 1, 0, 0]
filmes_recomendados = recomendar_filme(filmes_assistidos_usuario, filmes_assistidos, grupos_indice)
```

Aqui, criamos um vetor que representa os filmes assistidos por um usuário específico. Chamamos a função `recomendar_filme`, passando o vetor de filmes assistidos, a matriz de filmes assistidos e os índices dos grupos. As recomendações geradas são armazenadas em `filmes_recomendados`.

### Exibição das Recomendações

Finalmente, imprimimos as recomendações:

```python
print(f"\nFilmes recomendados: {filmes_recomendados}")
```

Essa saída fornece ao usuário uma lista clara de quais filmes ele pode assistir a seguir, com base nas preferências de outros usuários do mesmo grupo.

### Conclusão

Esse código é um exemplo prático de como técnicas de aprendizado de máquina, como o KMeans, podem ser aplicadas.