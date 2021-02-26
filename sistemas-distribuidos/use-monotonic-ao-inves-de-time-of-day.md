# Use monotonic ao invés de tempos baseados em epoch para medir a duração de um processamento

O snippet de código a seguir talvez seja a forma mais comum de medir o tempo de execução de um
determinado trecho de código:

```python
import time

start = time.time()
some_func()
end = time.time()
duration = end - start
```

Usando o **Python** como exemplo, a função [`time.time()`](https://docs.python.org/3/library/time.html#time.time)
retorna um _float_ que representa o tempo em segundos desde o [epoch](https://docs.python.org/3/library/time.html#epoch),
que pode ser classificado grosseiramente como o ponto onde o tempo começou.
Usando o Unix como exemplo (visto que esse dado varia de acordo com a plataforma) o _epoch_ é 01/01/1970, 00:00:00 (UTC).

## O Problema

Porém, se você tiver muito azar, esse tempo pode mudar durante a execução do trecho de código alvo da
medição provocando assim o efeito de [**Clock Skew**](https://en.wikipedia.org/wiki/Clock_skew)
(fenômeno onde relógios diferentes podem perder a sincronia).

Talvez o principal protocolo utilizado para sincronização de relógios é o [**NTP**](https://en.wikipedia.org/wiki/Network_Time_Protocol).
É muito comum a utilização do _NTP_ para garantir a sincronia de relógios entre múltiplos nós de um sistema distribuído,
assim como manter o relógio correto de diversos dispositivos (assim como do seu computador ou smartphone), diminuindo assim
os casos de _Clock Skew_.

Voltando ao exemplo anterior, podemos ter uma atualização do relógio durante o processamento que está sendo medido:

```python
import time

start = time.time()
some_func()  # <- Atualização de relógio decorrente do NTP
end = time.time()
duration = end - start  # possivelmente um valor negativo
```

## A Solução
Para prevenir que isso ocorra, existe a função [`time.monotonic()`](https://docs.python.org/3/library/time.html#time.monotonic)
que retorna um _float_ representando o tempo em segundos desde um tempo determinado pelo sistema operacional.
Esse tempo não é editável e não pode voltar atrás (geralmente esse é o tempo desde o momento do boot do sistema operacional).

```python
import time

start = time.monotonic()
some_func()
end = time.monotonic()
duration = end - start
```

Dessa forma não corremos o risco de uma intervenção (seja de qualquer origem) no tempo do sistema operacional durante a medição.

## Kernel
As chamadas do kernel para recuperar o _tempo corrente_ e o _tempo monotonic_ são as seguintes:
* `clock_gettime(CLOCK_REALTIME)`
* `clock_gettime(CLOCK_MONOTONIC)`

## Observações
O tempo _monotonic_ não pode ser usado para sincronimos entre sistemas distribuídos já que é um tempo baseado em um valor do sistema operacional local, nesses casos o tempo real através do uso do _NTP_ continua sendo uma melhor opção.

## Referências
* [Distributed Systems 3.2: Clock Synchronisation](https://youtu.be/mAyW-4LeXZo)
* [clock\_gettime](https://linux.die.net/man/3/clock_gettime)
* [Python Time API](https://docs.python.org/3/library/time.html)
