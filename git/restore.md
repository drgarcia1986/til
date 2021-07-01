# Restaurando o conteudo de um diretório se baseando em uma branch
A partir da versão 2.23 do git é possível utilizar o comando `git restore` para restaurar
o conteudo de um diretório tendo como base um branch:

```bash
$ git restore --source=<BRANCH COM O CONTEUDO A SER RESTAURADO> --worktree -- <DIRETORIO>
```

e.g.

```bash
$ git restore --source=main --worktree -- modified_directory
```

## Referências
- [How to git reset --hard a subdirectory?](https://stackoverflow.com/a/15404733)
