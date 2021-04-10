# Exit Codes a partir de 128
Processos que encerram sua execução com o seu exit code como `0`, representam uma execução com sucesso.
Qualquer valor diferente disso representa uma falha de execução, ou um comportamento inesperado.

Para os casos onde o processo recebeu um sinal externo (como por exemplo `SIGTERM`, `SIGXCPU`, etc),
existe uma regra especial para representar esse sinal através do exit code do processo.
Essa regra se baseia no número `128`, por exemplo, um processo encerrado com o exit code `137`,
representa um processo que foi encerrado através de um `SIGKILL`.
A regra é simples: código especial **128** + código do sinal enviado (no caso do _SIGKILL_ o código é **9**).

`128 + 9 (SIGKILL) = 137`

## Referências
* [Exit Status](http://www.gnu.org/software/bash/manual/html_node/Exit-Status.html)
