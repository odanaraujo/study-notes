# Estrutura de dados com golang

---

Esse documento visa realizar anotações sobre estrutura de dados. Estarei usando golang para estudar e praticar as teorias. 

## Time complexity

Ao realizar soluções, temos considerações a fazer depois de criar nosso código.
- Qual a complexidade do tempo (time complexity)
- Qual a complexidade do espaço (space complexity)


Time complexity é uma forma de ter uma noção do tempo que o código levaria para terminar a execução. O input recebido nos ajuda a ter uma ideia de como nosso algoritmo vai performar.

*se for um número maior, tende a demorar mais*

## Space complexity

É a medida do espaço em memória (**RAM**) necessário para executar um programa ou algoritmo. Ajuda a avaliar a eficiência de um algoritmo em termos de uso de memória.

*Algoritmos que consomem menos memória são preferíveis*

De forma simples, é a possível quantia de memória que o um algoritmo utilizaria para finalizar uma execução.

**Exemplo para simplificar**

O space complexity é frequentemente expressa como uma função do tamanho da entrada. Por exemplo, **O(1)** significa que o espaço é constante, não importa o tamanho da entrada. **O(n)** significa que o espaço é aumenta linearmente com o tamanho da entrada. 

*Imagine uma lista de números. Se você copiar todos esses números em uma nova lista, estará usando espaço adicional, igual ao tamanho da lista original. Isso seria um space complexity de **O(n)***

## Notações assintóticas

Análise assintótica é uma técnica usada para avaliar o desempenho de algoritmos <u>à medida que o tamanho da entrada cresce para o infitito.</u> Ela nos ajuda a entender como o tempo de execução ou espaço necessário se comporta à medida que os problemas se tornam cada vez maiores.

Às vezes, otimizar um aspecto *(tempo ou espaço)* pode piorar o outro. É importante encontrar um equilíbrio dependendo dos requisitos do problema.

Notações assintóticas, então, são formas matemáticas de descrever o tempo de execução de um algoritmo.

**Ex:** Performance de um carro com 1L de gasolina
**Estrada com pouco trânsito:** 25km/L
**Cidade com muito trânsito:** 10km/L
**Cidade e estrada com trânsito médio:** 15km/L

Assim, notação assintótica vai nos ajudar a definir:

- O pior caso
- O médio caso
- O melhor caso

## Notação big O (O)

É uma forma de definir o limite superior (**upper bound**) de um algoritmo em execução. Isso exemplificaria qual o tempo máximo que uma aplicação levaria para executar.
**Ex:** Se um algoritmo executa em no máximo 100s em seu pior tempo, isso quer dizer que temos um **upper bound**, onde não teríamos cenários onde demoraria mais do que 100s.

## Regras da notação BIG O (O)

Imagine que temos um computador com um único processador e ele executa uma tarefa de cada vez. 

- Cada ação “assign” (var int x = 5) custa 1 unidade de tempo.
- Cada ação de return (return x) custa 1 unidade de tempo.
- Cada operação aritmética (x + y) custa 1 unidade de tempo.
C- ada operação lógica (x && y) custa 1 unidade de tempo.
- Outras pequenas operações também levam 1 unidade de tempo
- Para outras ordem menores, sempre desconsiderar:
**T = n² + 3n + 1 => O(n²)**
Na hora que terminar de calcular o algoritmo, sempre manter a unidade mais forte. Nesse caso, **n²**.

Desconsiderar constantes
**T = 3n² + 6n + 1 => O(n²)**

---

### Algoritmos constantes

```
func sum(x, y int) int {
	result := x + y
	return result
}
```

Linha     | Operações   | Unidade de tempo
--------- | ----------- | ------
2 | 1 + 1 + 1 + 1 | 4
3 | 1 + 1 | 2

<ol>
    <li> Acessa o x</li>
    <li> Acessa o y</li>
    <li> Realiza a soma dos valores</li>
    <li> Realiza a atribuição</li>
</ol>

*A linha 2 totaliza 4 unidades de tempo*
<ol>
    <li>Acessa o valor de result</li>
    <li>Realiza um return</li>
</ol>

*A linha 3 realiza 2 unidade de tempo*

T = 2 + 4  => 6
T ==~6(constante) - ou seja, independente do valor informado via parâmetro, ele não muda.

Assim, quando um algoritmo é constante, temos que o **time complexity é de O(1)**.

---

## Algoritmos lineares

```
func calculate(n int) int {
	sum := 0
	for i := 1; 1 <= n; i++ {
		sum += i
	}

	return sum
}
```

No exemplo do código acima, temos a seguinte leitura:
<ol>
<li>linha 2 não acessa a variável e nem o valor, apenas atribui o valor, gerando apenas uma operação </li>
<li>Na linha 3, teremos que dividir em algumas partes
<ol>

<li>No primeiro momento, i recebe valor de 1 </li>
<li>Em seguida, é acessado o valor de i e de n e realizado uma operação lógica
<li>Nessa operação lógica, sempre teremos (n + 1), pois, ele faz uma verificação se o valor é menor ou igual. Caso positivo, aí ele sai do for.</li>
<li>Na última parte, acessa o valor de i, que é uma operação. Pegamos o valor de i e soma mais 1, gerando uma nova operação. Pega esse valor e atribui a i, gerando uma última operação. </li>
<li>Na linha 4, acessamos o sum, a soma do sum + i, atribuição que custa mais um e o acesso ao i.</li>
<li>Na linha 6, temos o return e o sum, totalizando 2 unidades de tempo.</ol></li>
</ol>


**Usando um exemplo, inserindo 3 no parâmetro, teríamos a seguinte tabela:**

Linha     | Operações            | Unidade de tempo
--------- | -----------          | ------
2         | 1                    | 1
3         | 1 + 3(n + 1) + 3n    | 6n + 4
4         | n(1 + 1 + 1 + 1)     | 4n
6         | 1 + 1 | 2            |

**Qual valor eu aplico ao big O?**

Quando temos partes do código que aumentam a complexidade conforme o valor recebido aumenta, então, temos que calcular sempre considerando o tamanho da entrada, geralmente utilizando N.

1 + 6n + 4 + 4n + 2 ⇒ 10n + 7

**Time complexity: O(N)**
Assumimos sempre a maior complexidade/valor para a fórmula, já que os valores constantes, como vimos, são desconsiderados.

![time complexity O(N)](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220812122843/Logarithmic-time-complexity-blog-1.jpg)

---

## Estruturas Polinomiais

Mais formalmente, um algoritmo é considerado polinomial se o tempo de execução    T(n) for limitado por um polinômio de n, onde "n" representa o tamanho da entrada.
Por exemplo, se T(n) for da forma T(n) = O(n²) ou T(n) = O(n³), então o algoritmo é considerado polinomial.

```
func Calculate(n int) {
	for i := 1; i <= n; i++ {
		for j := 1; j <= n; j++ {
			print(fmt.Sprintf("i = %d, j = %d\n", i, j))
		}
		println("finalizando \n")
	}
	print("fora do loop")
}
```

Considerando o código acima, temos:

<ol>
    <li>Na linha 1:
        <ol>
            <li>realiza a atribuição ao valor de 1</li>
            <li>vai fazer interação n + 1</li>
            <li>Por último, temos i++, onde ele acessa o valor de i, faz o valor +1 e atribui o novo valor a i. </li>
        </ol>
    </li>
    <li>Na linha 2:
        <ol>
            <li> A mesma coisa da linha 1, porém, como é um for dentro de outro for, ele vai executar total de vezes que o for de fora for executado.</li>
        </ol>
    </li>
    <li> Na linha 3:
        <ol>
            <li> o print será executado n vezes o primeiro for e n vezes o segundo for, ficando n² * tudo que está dentro do for. </li>
            <li>Nesse caso, ele acessa o valor de i, valor de j e realiza o print. Cada ação gerando uma unidade de tempo.</li>
        </ol>
    </li>
    <li>Na linha 5:
        <ol>
            <li>executa o print n vezes o for de dentro.</li>
        </ol>
    </li>
    <li>Última linha:
        <ol>
            <li>Executa apenas uma vez, já que se encontra fora do for.</li>
        </ol>
    </li>
</ol>

Linha     | Operações            | Unidade de tempo
--------- | -----------          | ------
1         | 1 + 3(n + 1) + 3n    | 6n + 2
2         | n(1 + 3(n + 1) + 3n) | 6n² + 2n
3         | n²(1 + 1 + 1)        | 3n²
7         | 1                    | 1

**Qual valor eu aplico ao BIG O?**

T = 6n + 4 + 6n² + 4n + 3n² + n + 1
T = 9n² + 11n + 5
T = n²

![estrutura polinomiais](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20220812122843/Logarithmic-time-complexity-blog-1.jpg)

---

## Introdução à arrays unidimensionais

