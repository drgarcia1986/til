# Usando Semáforo em códigos async

Em alguns casos é necessário o controle de quantas operações serão feitas de
forma concorrente/paralela. Nessas situações, podemos fazer uso de [semáforos](https://pt.wikipedia.org/wiki/Sem%C3%A1foro_(computa%C3%A7%C3%A3o)).
Semáforos determinam a quantidade de threads e/ou processos que podem ter acesso
a um recurso compartilhado.

Em python, códigos que utilizam o [asyncio](https://docs.python.org/3/library/asyncio.html),
podem utilizar semáforos através da classe [`Semaphore`](https://docs.python.org/3/library/asyncio-sync.html#asyncio.Semaphore).
Por exemplo:

```python
async with asyncio.Semaphore(10):
    await some_func()
```

No código acima estamos determinando que a função `some_func()` só poderá ser invocada
no máximo 10 vezes de forma concorrente. Outra forma de escrever esse exemplo seria:

```python
sem = asyncio.Semaphore(10)

await sem.acquire()
try:
    await some_func()
finally:
    sem.release()
```

### Exemplo mais elaborado
O exemplo a seguir pode ser executado para visualizar o controle que o objeto `semaphore` detem
na execução do código, onde apenas duas tasks serão executadas concorrentemente:

```python
import asyncio
from random import randint


async def pool_wait(*tasks, pool_size=None):
    if (
        not isinstance(pool_size, int) or
        pool_size <= 0
    ):
        raise ValueError('pool_size must be a positive integer')

    semaphore = asyncio.Semaphore(pool_size)

    async def exec_task(task):
        async with semaphore:
            await task

    await asyncio.gather(*(exec_task(t) for t in tasks))


async def say_and_sleep(msg, time=1):
    print(f'{msg} -> sleeping for {time}s')
    await asyncio.sleep(time)


async def main():
    tasks = [
        say_and_sleep(f'Task: {i}', randint(1, 3))
        for i in range(10)
    ]
    await pool_wait(*tasks, pool_size=2)


if __name__ == '__main__':
    asyncio.run(main())
```
Note que a função `pool_wait` pode ser utilizada de forma genérica como um _helper_ para
tasks que devem ser executadas com um controle em relação ao número de operações
concorrentes.

## Referências
* [asyncio.Semaphore](https://docs.python.org/3/library/asyncio-sync.html#asyncio.Semaphore)
