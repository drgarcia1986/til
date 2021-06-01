# Criando e recuperando um dump do Postgres
Complementando o TIL ["Rodar um pod com client para Postgres"](./rodar-um-pod-com-client-para-postgres.md), a partir do pod gerado é possível criar
um dump de um banco postgres através do comando `pg_dump`:

```
$ pg_dump -h <HOST> -U <USER> -f <DESTINO DO DUMP> <DATABASE>
```

Feito isso, gere um _tar.gz_ do backup com o comando:
```
$ tar cvzf <DESTINO> <ARQUIVO DO DUMP>
```

Para recuperar esse arquivo, basta utilizar o comando `kubectl cp`:
```
$ kubectl cp <NOME DO POD>:<PATH DO ARQUIVO> <DESTINO> -n <NAMESPACE>
```

E por fim, para fazer o restore:
```
$ psql -h <HOST> -U <USER> -d <DATABASE> -f <PATH DO DUMP>
```

É necessário fazer o drop/create database antes.

## Referências
- [pg_dump](https://www.postgresql.org/docs/12/app-pgdump.html)
- [kubectl cp](https://medium.com/@nnilesh7756/copy-directories-and-files-to-and-from-kubernetes-container-pod-19612fa74660)
