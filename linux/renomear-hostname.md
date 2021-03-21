# Renomar o hostname

Renomar o hostname em uma distribuição linux é fácil, basta editar o arquivo
`/etc/hostname` e o arquivo `/etc/hosts`. O primeiro arquivo contém apenas o
hostname desejado, e.g.:

```
my-hostname
```
Enquanto que o segundo possui um mapeamento entre _ip <-> hostname_, e.g.:

```
127.0.0.1 localhost
127.0.0.1 my-hostname
```

## Referências
* [Ubuntu Linux Change Hostname](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/)
