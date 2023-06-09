Algoritmo Needleman-Wunsch para Alinhamento de Sequências
======

Contexto
---------

O algoritmo de Needleman-Wunsch é muito usado na área de bioinformática para alinhar sequências, como proteínas e nucleotídeos. Ele essencialmente divide um grande problema em diversos pequenos problemas, usando as soluções dos pequenos problemas para encontrar uma solução ótima para o grande problema.

Esse algoritmo possui uma certa importância, uma vez que pode haver uma certa manipulação dos parâmetros, sendo muito beneficial para casos específicos. 

Alinhamento de sequências
---------
Antes de abordarmos de fato o algorítmo de Needleman-Wunsch e como ele funciona, precisamos primeiro entender o que são os alinhamentos de sequências, ou strings, que são o objetivo final desse algorítmo.

Os alinhamentos consistem em uma forma de se alterar duas sequências, adicionando {red}(gaps) [[**-**]] (espaços vazios que não representam nenhuma letra), para que elas apresentem o mesmo tamanho, caso sejam diferentes, e tenham o maior número possível de letras alinhadas no mesmo index.

Para exemplificar um caso de alinhamento, temos como entradas as sequências:

|index        | 0 | 1 | 2 | 3 | 4 | 5 |
|-------------|---|---|---|---|---|---|
| Sequência 1 | G | A | C | T |   |   |
| Sequência 2 | G | C | A | T | C | T |


Uma possível forma de se alinhar essas duas sequências, apenas inserindo gaps, seria:

|index        | 0 | 1 | 2 | 3 | 4 | 5 |
|-------------|---|---|---|---|---|---|
| Sequência 1 | G | **-** | A | **-** | C | T |
| Sequência 2 | G | C | A | T | C | T |

??? Atividade 1

Para treinar e fixar o conceito de alinhamento de sequências, vamos treinar esse processo.

Tendo como entradas as sequências:

|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | T | A |   |   |
| Sequência 2 | C | C | A | T | T | A |   |

Uma forma de alinhar as sequências, apenas inserindo gaps, seria:

|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | A |   |
| Sequência 2 | C | C | A | T | T | A |   |

Encontra outra forma de alinhas as mesmas sequências de entrada, porém de forma diferente da encontrada no exemplo.

::: Gabarito

Outra forma de alinhar as sequências, apenas inserindo gaps, seria:

|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | T | **-** | A |   |
| Sequência 2 | C | C | A | T | T | A |   |
:::
???

!!! Aviso
É importante destacar que esse gabarito é apenas uma das diversas formas de alinhar uma sequência, como exemplo podemos alinhá-las usando ainda mais gaps:


|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | **-** | A |
| Sequência 2 | **-** | C | C | A | T | T | A |

Tendo conhecimento desse grande número de possibilidades para se alinhar uma sequência, como vamos selecionar a melhor opção para cada caso?

O próximo tópido do handout irá abordar essa questão.
!!!

O sistema de pontuação
---------

Para selecionar o melhor alinhamento possível em cada caso, utiliza-se um sistema de pontuação, também chamado de função de score.

Esse sistema consiste em comparar cada correspondência de elementos no mesmo índice das duas sequências já alinhadas, classificar essa correspondência e atribuir uma pontuação a ela.

Os tipos de correspondência possíveis são:

* **Match**: quando as elementos do mesmo índice são iguais.

* **Mismatch**: quando as elementos do mesmo índice são diferentes.

* **Indel**: quando ao menos um dos elementos comparados é um gap.

Como exemplo, podemos classificar as correspondências de um alinhamento da seguinte forma:

|index        | 0 | 1 | 2 | 3 | 4 | 5 |
|-------------|---|---|---|---|---|---|
| Sequência 1 | G     |     **-** |     A |     **-** |        G |     T |
| Sequência 2 | G     | C     | A     | T     |     C    | T     |
| Tipo        | Match | Indel | Match | Indel | Mismatch | Match |

Com essa classificação, podemos atribuir diferentes pontuações para cada tipo de correspondência, dessa forma cada alinhamento terá um score final que irá depender da correspondência de cada elemento seu.

Por exemplo, caso definíssemos como sistema de pontuação:

* **Match**: +2.

* **Mismatch**: -2.

* **Indel**: -1

Teremos como pontuação para o exemplo acima:

|index        | 0 | 1 | 2 | 3 | 4 | 5 |
|-------------|---|---|---|---|---|---|
| Sequência 1 | G     |     **-** |     A |     **-** |        G |     T |
| Sequência 2 | G     | C     | A     | T     |     C    | T     |
| Tipo        | Match | Indel | Match | Indel | Mismatch | Match |
| Pontuação   | +2 | -1 | +2 | -1 | -2 | +2 |

Por fim, teremos como score total do alinhamento a soma de cada posição: 

$$ +2 -1 +2 -1 -2 +2 = 4 $$

Dessa forma, podemos supor que uma alteração no sistema de pontuação poderá alterar diretamente o score final de um alinhamento.

Por exemplo, para casos de comparação em que deverão ser priorizados alinhamentos com o menor número de gaps, podemos atribuir uma pontuação mais negativa para os casos de Indel, como -2 ou -3.

Já para casos em que o número de gaps não seja tão relevante, mas queremos ao máximo evitar Mismatch, podemos atribuir uma pontuação mais negativa para os casos de Mismatch.

Assim conseguimos manipular e classificar um alinhamento como melhor ou pior que outro através de seu score total.

Para compreender melhor a influência do sistema de pontuação na escolha de um alinhamento, realize o seguinte exercício:

??? Atividade 2

Tendo como sistema de pontuação:

* **Match**: +3.

* **Mismatch**: -3.

* **Indel**: -1

Complete as lacunas e encontre o score final de cada um dos dois alinhamentos das mesmas sequências de entradas:

Alinhamento 1:

|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | A |   |
| Sequência 2 | C | C | A | T | T | A |   |
| Tipo        |  |  |  |  |  |  |   |
| Pontuação   |  |  |  |  |  |  |   |

Alinhamento 2:
|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | **-** | A |
| Sequência 2 | **-** | C | C | A | T | T | A |
| Tipo        |  |  |  |  |  |  |   |
| Pontuação   |  |  |  |  |  |  |   |

::: Gabarito

Alinhamento 1:
|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | A |   |
| Sequência 2 | C | C | A | T | T | A |   |
| Tipo        | Mismatch | Match | Mismatch | Indel | Match | Match |   |
| Pontuação   | -3 | +3 | -3 | -1 | +3 | +3 |   |


Score total: $$ -3 +3 -3 -1 +3 +3 = 2 $$


Alinhamento 2:
|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | **-** | A |
| Sequência 2 | **-** | C | C | A | T | T | A |
| Tipo        | Indel | Match | Mismatch | Indel | Match | Indel | Match |
| Pontuação   | -1 | +3 | -3 | -1 | +3 | -1 | +3 |


Score total: $$ -1 +3 -3 -1 +3 -1 +3 = 3 $$
:::
???

??? Atividade 3

Tendo como sistema de pontuação:

* **Match**: +3.

* **Mismatch**: -1.

* **Indel**: -3

Complete as lacunas e encontre o score final de cada um dos dois alinhamentos das mesmas sequências de entradas:

Alinhamento 1:

|index        | 0 | 1 | 2 | 3 | 4 | 5 |
|-------------|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | A |
| Sequência 2 | C | C | A | T | T | A |
| Tipo        |   |   |   |   |   |   |
| Pontuação   |   |   |   |   |   |   |

Alinhamento 2:
|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | **-** | A |
| Sequência 2 | **-** | C | C | A | T | T | A |
| Tipo        |  |  |  |  |  |  |   |
| Pontuação   |  |  |  |  |  |  |   |

::: Gabarito

Alinhamento 1:
|index        | 0 | 1 | 2 | 3 | 4 | 5 |
|-------------|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | A |
| Sequência 2 | C | C | A | T | T | A |
| Tipo        | Mismatch | Match | Mismatch | Indel | Match | Match |
| Pontuação   | -1 | +3 | -1 | -3 | +3 | +3 |


Score total: $$ -1 +3 -1 -3 +3 +3 = 4 $$


Alinhamento 2:
|index        | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------------|---|---|---|---|---|---|---|
| Sequência 1 | A | C | G | **-** | T | **-** | A |
| Sequência 2 | **-** | C | C | A | T | T | A |
| Tipo        | Indel | Match | Mismatch | Indel | Match | Indel | Match |
| Pontuação   | -3 | +3 | -1 | -3 | +3 | -3 | +3 |


Score total: $$ -3 +3 -1 -3 +3 -3 +3 = -1 $$
:::
???

A matriz de pontuação
---------

Para preecher a matriz de pontuação de forma analítica, é necessário comparar todas as células até o índice escolhido tanto da coluna quanto da linha. Vamos agora ver um exemplo para o alinhamento das sequências **GA** e **GCTA**, com **match = +1**, **mismatch = -1** e **indel = -1**:  

![](valor4.png)

Para saber o valor dessa célula que se localiza na posição (4,2) é necessário analisar parcialmente as duas sequências até sua segunda letra. Portanto a comparação seria entre GCT e G (vale lembrar que como a matriz tem uma casa vazia na primeira célula para ambas sequências, a posição 2 de linha ou coluna da matriz terá apenas uma letra da sequência correspondente):

![](valor5.png)

| G | C | T |
| G | - | - |
| - | - | - |
| 1 |-1 |-1 |

Dessa forma o valor encontrado da célula é -1.

Porém, ao aplicar o algorítmo para sequências muito grandes, a resolução analítica teria um grande gasto computacional, fazendo-se necessário uma resolução numérica para facilitar.

Essa resolução se baseia no fato de que ao você, por exemplo, descer uma casa desse problema para a posição (4,3), seria análogo a apenas adicionar uma letra ao exemplo anterior, portanto é possível você analisar apenas as 3 casas vizinhas da célula, as da esquerda, cima e diagonal-superior-esquerda. Dessa forma, a resolução analítica preenche a matriz de maneira recursiva, começando na casa (0,0) e indo até a de maior valor.

A matriz é preenchida de maneira numérica usando as seguintes regras:

1. As células na primeira linha e coluna da matriz são preenchidas com valores que correspondem à penalidade por inserções ou exclusões de letras ou aminoácidos, pois há apenas o uso da célula da esquerda somado ao indel, no caso da primeira linha, e o uso da célula de cima somado ao indel, no caso da primeira coluna.
2. Cada célula na matriz é preenchida ao comparar a pontuação final das três células adjacentes (de cima, da esquerda e da diagonal superior esquerda), cada uma somada ao indel, match ou mismatch, e escolher a maior pontuação para essa célula. Não se assuste, será explicado em mais detalhes.  

**Preencher primeira linha e coluna da matriz:**

A primeira posição é preenchida com 0, pois ainda não foi feita nenhuma comparação e não foram acrescentados ou retirados pontos.

![](img0.png)


??? Atividade 4

Seguindo a primeira regra, complete a primeira linha e a primeira coluna, lembrando que o **indel = -1**.

::: Gabarito

![](imgbase.png)

:::

???

Agora que já temos a estrutura da matriz, temos que completá-la!

Relembrando a segunda regra, para resolver uma única célula (em azul), devemos usar as 3 vizinhas (em vermelho).

![](celula1.png)

São feitos 3 cálculos. Dois deles, das células da esquerda e da direita, utilizam o indel (-1). A terceira conta é baseada no match (+1) ou mismatch (-1), então a variável usada pode variar.

$$ totalEsquerda =  esquerda + indel $$
$$ totalCima =  cima + indel $$
$$ totalDiagonal =  diagonal + (mis)match $$

Para a primeira célula, levando em conta que há um match, as três contas serão:

$$ totalEsquerda =  - 1 - 1 = - 2 $$
$$ totalCima =  - 1 - 1 = - 2 $$
$$ totalDiagonal =  0 + 1 = 1 $$

Com as 3 contas feitas, compara-se os 3 resultados e a maior solução, totalDiagonal = 1, é colocada na célula.

![](celula2.png)

Agora fazendo na segunda célula.

![](celula3.png)

Para a segunda célula, levando em conta que há um mismatch, as três contas serão:

$$ totalEsquerda =  1 - 1 = 0 $$
$$ totalCima =  - 2 - 1 = - 3 $$
$$ totalDiagonal =  -1 - 1 = - 2 $$

Fazendo o mesmo passo de antes, coloca-se na célula o totalEsquerda = 0 na célula.

![](celula4.png)

??? Atividade 5

Agora é a sua vez de fazer! Complete a matriz, se lembrando dos matches e mismatches.

::: Gabarito

![](completa.png)

:::

???


Finalizando a matriz
---------

Ao fim do preenchimento da matriz e após encontrar a solução ótima do alinhamento, podemos calcular a pontuação final do alinhamento realizando a somatória dos matches, mismatches e indels:

| G | C | A | T | - |
| G | - | A | T | T |
| - | - | - | - | - |
|+1 |-1 |+1 |+1 |-1 |
Obtendo ao fim da somatória uma pontuação de [[+1]].

Traceback da Matriz
---------


Já sabendo todas as etapas de construção da matriz, a parte final do algorítmo envolve fazer o traceback das células para determinar o alinhamento de ambas as sequências.

![](traceback.png)

Começando na célula mais a direita, e mais abaixo, se escolhe o caminho que tenha o valor mais baixo. 

Cada passo durante o traceback, uma atitude é tomada em relação às sequências. Ao se escolher a célula acima a que você está no momento, é criado um gap na sequência à esquerda da matriz, analogamente, ao se escolher a célula da esquerda, um gap será criado na sequência no topo da matriz. Escolher a célula da diagonal, que seria o caso mais comum, é definido como um match ou um mismatch.

Ao término dessa etapa, é realizado a soma de cada um dos passos tomados em que:

$$ match =  1 $$
$$ mismatch =  -1 $$
$$ gap =  -1 $$

Relembrando que essa é apenas uma das possíveis atribuições desses valores, podendo mudar de acordo com seu objetivo.

Exemplo de finalização do algoritmo
---------

Utilizando essa matriz como exemplo para a última etapa do processo, o caso ótimo seria usando os passos:

![](exemplo.jpeg)

$$ Diagonal, diagonal, diagonal, diagonal, cima, diagonal, esquerda, diagonal $$

Resultando em:

$$ - 1 + 1 - 1 - 1 + 1 + 1 - 1 + 1 = 0$$

E por fim, o alinhamento final seria:

|index        | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------------|---|---|---|---|---|---|---|---|
| Sequência 1 | G | C | A | T | - | G | C | G |
| Sequência 2 | G | - | A | T | T | A | C | A |
| Tipo        | Match | Indel | Match | Match | Indel | Mismatch |Match | Mismatch |
| Pontuação   | +1 | -1 | +1 | +1 | -1 | -1 | +1 | -1 |