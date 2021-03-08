# Logical timers e ordem de eventos
Podemos classificar ações que ocorrem em um nó de um sistema distribuído como `eventos`.
Alguns possíveis exemplos para esses eventos:

* Envio de mensangens
* Recebimento de mensagens
* Processamentos locais
* Etc.

Dentro do contexto do mesmo nó é fácil definir a ordem desses eventos, visto que geralmente eles são sequenciais (assumindo
um ambiente single thread) e também podemos usar o tempo físico do nó (pode ser tanto baseado em [epoch ou em monotic](./use-monotonic-ao-inves-de-time-of-day.md)),
como indicador de ordem de execução, porém em sistemas distribuídos esses eventos ocorrem de forma concorrente e nem sempre
podemos contar com a sincronia entre os relógios de todos os nós desse sistema.

Existem algumas técnicas para mensurar esse ordem:

* **Physical clocks**: Técnicas baseadas em tempos reais (e.g. timestamp).
* **Logical clocks**: Técnicas baseadas em contadores de eventos.

Como dito anteriormente, _physical clocks_ não são bons para garantir a ordem de processamento de eventos devido ao fato de que as mensagem podem
chegar no nó de forma desorganizada (o que já invalida uma ordenação pelo tempo do momento da chegada) e também pelo
fato de que os relógios entre os nós podem estar dessincronizados.

Sendo assim, _logical clocks_ podem ser uma melhor opção.

## Lamport Clocks
Consiste em um contador a nível de nó do sistema distribuído. Esse contador é incrementado a cada novo evento e enviado
como parte do identificador da mensagem que está sendo transmitida entre os nós. Ao receber uma mensagem, esse nó
deve fazer uma comparação entre o seu contador e o contador recebido na mensagem, o maior valor incrementado a 1 passa a ser o valor
do contador do nó atual:

1. count = `1`
2. received\_message = `{"count": 3, "msg": "the message"}`
3. count = `max(count, received\_message["count"]) + 1`
4. count = `4`

```
 +---+                 +---+                +---+
 | A |                 | B |                | C |
 +---+                 +---+                +---+
   |                     |                    |  
   |                     |                    |  
 1 |                     |                    | 1 
   |                     |                    |  
   |                     |                    |  
 2 +------+              |                    |  
   |      |              |                    |  
   |      |   (2,m1)     |                    |  
   |      ---------------+ 3                  |  
   |                     |                    |  
 3 |                   4 +-------+            |  
   |                     |       |  (4, m2)   |  
   |                     |       -------------+ 5 
   |                     |                    |  
```

Ao receber uma mensagem o contador de eventos de **B** começa em _3_ por ser o primeiro evento ocorrido do nó e ele receber o
contador com o valor de _2_ vindo de **A** que é o remetente da mensagem, isso faz com que o contador de **B** para o próximo evento
seja atualizado para _4_. O mesmo ocorre com **C** onde mesmo não sendo o primeiro evento, o evento referente a mensagem `m2` conta com o contador _4_ 
fazendo com que o valor do contador de **C** passe para 5.

Os eventos _3_ e _1_ se repetem no cluster, porém podemos garantir que esses identificadores sejam únicos adicionando a
identificação do nó a mensagem, ficando algo como por exemplo: `{"count": 3, "node": "B", "msg": "m1"}`.

Ainda sim, _lamport clocks_ não são o suficiente para determinar se dois eventos são concorrentes, tornando ainda mais
difícil para determinar se um evento ocorreu antes de outro ou se ambos ocorreram de forma concorrente.

### Vector Clocks
Consiste em um vetor de contadores onde cada elemento desse vetor representa um o contador de um dos nós do sistema distribuído.
Assim como no _lambort clocks_, cada evento incrementa o contador de eventos do nó, porém no caso de _vector clocks_ somente
o elemento que representa o nó no vetor é incrementado a não ser no caso de mensagens recebidas, nestes caso todos os contadores
são atualizados baseados nos contadores que foram recebidos na mensagem.

1. counters = `[1, 0, 3, 0]`
2. node\_pos = `1`
2. received\_message = `{"counters": [3, 0, 5, 1], "msg": "the message"}`
3. counters = `[max(counters[i], msg_counters[i]) for i in received\_message["counters"]]`
4. counters[node\_pod] = `counters[node\_pos] + 1`
5. counters = `[3, 1, 5, 1]`

```
       +---+                 +---+                +---+
       | A |                 | B |                | C |
       +---+                 +---+                +---+
         |                     |                    |  
         |                     |                    |  
 [1,0,0] |                     |                    | [0,0,1]
         |                     |                    |  
         |                     |                    |  
 [2,0,0] +------+              |                    |  
         |      |              |                    |  
         |      |([2,0,0],m1)  |                    |  
         |      ---------------+ [2,1,0]            |  
         |                     |                    |  
 [3,0,0] |             [2,2,0] +-------+            |  
         |                     |       |([2,2,0],m2)|  
         |                     |       -------------+ [2,2,2]
         |                     |                    |  
```

Comparando esse exemplo com o anterior, somente os contadores referentes ao nó atual são atualizados
a cada evento, enquanto que outros contadores se mantém com os valores originais (salvo casos onde o nó atual
possui uma referência de contador mais atualizado para os outros nós).

Desta forma podemos determinar que `[2, 2, 0]` por exemplo representa o segundo evento de **A**, o segundo evento
de **B** e essa por fim essa mensagem ainda não gerou um evento em **C**.

## Referências
* [Distributed Systems 3.3: Causality and happens-before](https://youtu.be/OKHIdpOAxto)
* [Distributed Systems 4.1: Logical time](https://youtu.be/x-D8iFU1d-o)
