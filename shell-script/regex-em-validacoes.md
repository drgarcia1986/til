# Regex em validações

É possível realizar validações e capturas de grupos com regex no shell script
bastando utilizar o operador `=~` e.g.:

```bash
#!/bin/bash

PATTERN="(\w+)\s+(\w+)"
if [[ "Diego Garcia" =~ $PATTERN ]]; then
    echo "Nome: ${BASH_REMATCH[1]} - Sobrenome ${BASH_REMATCH[2]}"
fi
```

O código anterior apresenta o seguinte resultado:

```
Nome: Diego - Sobrenome: Garcia
```

Caso o texto testado de match com a regex informada pós operador `=~`
a operação será processada como _"true"_ e os grupos de captura
estarão disponíveis na variável `$BASH_REMATCH`.
Essa variável pode ser acessada como um array, sendo a posição `0` o
conteúdo completo do texto que deu match e as posições seguintes
são referentes aos grupos de captura.

## Referências
* [Bash Regular Expressions](https://www.linuxjournal.com/content/bash-regular-expressions)
