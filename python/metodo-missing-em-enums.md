# Método \_missing\_ em Enums

A class [`Enum`](https://docs.python.org/3/library/enum.html) do Python possue um método
especial para os casos onde o membro solicitado não é reconhecido, e.g.:

```python
>>> from enum import Enum

>>> class ToyEnum(Enum):
...     ONE = 'one'
...     TWO = 'two'
...     UNKNOW = 'unknow'

...     @classmethod
...     def _missing_(cls, value):
...         return cls.UNKNOW

>>> ToyEnum('one')
<ToyEnum.ONE: 'one'>

>>> ToyEnum('xpto')
<ToyEnum.UNKNOW: 'unknow'>
```

### Observação
Note que o método é `_missing_` com underline simples (ou seja, não é um método `__dunder__`)

## Referências
* [Enum - supported-sunder-names](https://docs.python.org/3/library/enum.html#supported-sunder-names)
