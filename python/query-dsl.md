# Como criar queries a partir de argumentos de um método

Sempre fiquei curioso em como os ORMs em python tranformavam comparações feitas em argumentos de métodos em consultas SQL, por exemplo `db.search(User.name == 'John')`.
Ao bater o olho nesse código a primeira coisa que vem a mente é que o argumento do método `search` ira receber um booleano e isso não ajuda muito na construção de uma query.

Mas afinal de contas, como `db.search(User.name == 'John')` se transformaria em `select * from user where name = 'John'` ?
Através de _Operator overloading_, que é possível ser feito no python através de [métodos com nomes especiais](https://docs.python.org/3/reference/datamodel.html?#special-method-names).

Esses métodos, são os métodos responsáveis por operações básicas feitas em um objeto como por exemplo soma, subtração, comparação, etc.
O exemplo mais comum é o método `__init__` que será executado assim que um objeto seja inicializado ou o método `__str__` que será executado assim que a representação em string
de um objeto for requisitada (por exemplo `print(obj)`).

Para demonstrar na prática como poderiamos tranformar `db.search(User.name == 'John')` em `select * from user where name = 'John'`, teriamos algo como:

```python
from dataclasses import dataclass


@dataclass
class Query:
    table: str
    field: str
    value: str

    def __str__(self):
        return (
            f'select * from {self.table} where {self.field} = "{self.value}"'
        )


@dataclass
class Field:
    table: str
    name: str

    def __eq__(self, other):
        return Query(self.table, self.name, other)


class TableMeta(type):
    def __getattr__(cls, attr):
        return Field(cls.__name__, attr)


class Table(metaclass=TableMeta):
    pass


class Database:
    def search(self, query):
        return str(query)


class User(Table):
    pass
```

Com essas classes é possível executar o seguinte código:

```python
>>> db = Database()
>>> print(db.search(User.name == 'John'))
select * from User where name = "John"
```

A parte mais relevante do código anterior fica por conta do método `__eq__` da classe `Field`, é através dele que `name == 'John'` ao invés de retornar `True` ou `False`
retorna um objeto do tipo `Query` que já está preparado para montar a query a partir dos dados que esse objeto recebeu no momento da sua criação.

Esse é um código bem simplista, porém, é uma boa introdução sobre o tema.

## Referências
* [TinyDB](https://tinydb.readthedocs.io/en/latest/)
* [Python Data Model](https://docs.python.org/3/reference/datamodel.html)
