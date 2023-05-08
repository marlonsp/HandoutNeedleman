Algoritmo Needleman-Wunsch para Alinhamento de Sequências
======

Contexto
---------

O algoritmo de Needleman-Wunsch é muito usado na área de bioinformática para alinhar sequências, como proteínas e nucleotídeos. Ele essencialmente divide um grande problema em diversos pequenos problemas, usando as soluções dos pequenos problemas para encontrar uma solução ótima para o grande problema.

Esse algoritmo possui uma certa importância, uma vez que pode haver uma certa manipulação dos parâmetros, sendo muito beneficial para casos específicos. 

![](exemplo.jpeg)

Sobre o algoritmo
---------

* Entradas: duas sequências e um sistema de pontuação;

* Saídas: duas subsequências alinhadas;

As sequências são strings, e elas podem ou não ser de tamanhos diferentes. Você pode estar se perguntando: O que seria esse alinhamento das sequências?

Alinhar as sequências significa que o alinhamento de sequências obtido ao aplicar o algoritmo de Needleman-Wunsch é o alinhamento ótimo entre as duas sequências, que maximiza o número de correspondências de caracteres ou símbolos em cada posição do alinhamento.

A outra entrada, o sistema de pontuação, é composto de 3 informações:

* match: Quando as letras do mesmo index são iguais.

* mismatch: Quando as letras do mesmo index são diferentes.

* indel: Quando o melhor alinhamento para um certo index é a inserção ou remoção de uma base na sequência, chamado de gap.

A matriz de pontuação
---------
A matriz é preenchida usando as seguintes regras:

1. A primeira linha e coluna da matriz são preenchidas com valores que correspondem à penalidade por inserções ou exclusões de letras ou aminoácidos.
2. Cada célula na matriz é preenchida com a pontuação máxima das três células adjacentes mais a pontuação da comparação de letras ou aminoácidos para essa célula.
4. O caminho de volta através da matriz é usado para determinar a melhor correspondência entre as duas sequências.

Veja um exemplo do preencimento da matriz de pontuação para o alinhamento das sequências GATT e GCAT, com match = +1, mismatch = -1 e indel = -1:

:img 

Ao fim do preenchimento da matriz e após encontrar a solução ótima do alinhamento, podemos calcular a pontuação final do alinhamento realizando a somatória dos matches, mismatches e indels:

| G | C | A | T | - |
| G | - | A | T | T |
| - | - | - | - | - |
|+1 |-1 |+1 |+1 |-1 |
Obtendo ao fim da somatória uma pontuação de [[+1]].


??? Atividade 1

Tomando como pontuação:
* match: +1;
* mismatch e indel: -1.

Calcule a pontuação total para as saídas do alinhamento:

* ACG-TAG
* GCGATAG

::: Gabarito
Para os alinhamentos, temos o seguinte caso:
| A | C | G | - | T | A | G |
| G | C | G | A | T | A | G |
| - | - | - | - | - | - | - |
|-1 |+1 |+1 |-1 |+1 |+1 |+1 |
Obtendo ao fim da somatória uma pontuação de [[+3]].
:::

???


A partir daqui é do template, não faz parte do nosso handout
---------


Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

??? Atividade 2

Preencha a primeira posição.

::: Gabarito
Este é um exemplo de gabarito.
:::

???

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!
