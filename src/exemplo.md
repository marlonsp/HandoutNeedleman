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

Alinhar as sequências seignifica que o alinhamento de sequências obtido ao aplicar o algoritmo de Needleman-Wunsch é o alinhamento ótimo entre as duas sequências, que maximiza o número de correspondências de caracteres ou símbolos em cada posição do alinhamento.

A outra entrada, o sistema de pontuação, é composto de 3 informações:

* match: Quando as letras do mesmo index são iguais.

* mismatch: Quando as letras do mesmo index são diferentes.

* indel: Quando o melhor alinhamento para um certo index é a inserção ou remoção de uma base na sequência, chamado de gap.


??? Atividade 1

Tomando como pontuação:
* match: +1;
* mismatch e indel: -1.

Calcule a pontuação total para as saídas do alinhamento:

* ACG-TAG
* GCGAT-G

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???


A partir daqui é do exemplo do hashi
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


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

