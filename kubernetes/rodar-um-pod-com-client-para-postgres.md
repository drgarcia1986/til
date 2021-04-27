# Rodar um pod com client para Postgres
É comum as situações onde precisamos acessar um banco de dados através de um cluster de
kubernetes, seja para testar conectividade, seja para executar queries.

Para isso, a Bitnami disponibilizou uma imagem com o ferramental necessário para isso e
graças ao comando `kubectl run` podemos criar um pod temporário com acesso shell a um
container com essas ferramentas:

```
$ kubectl run -i --tty postgres-client --image=bitnami/postgresql --restart=Never --env="PGPASSWORD=<PASSWORD>" -- bash
```

Ao acessar o shell do container, basta executar o seguinte comando para se conectar ao banco:

```
$ psql --host <HOST> -U <USER> -d <DATABASE>
```

## Referências
- [Bitnami Docker Postgres](https://github.com/bitnami/bitnami-docker-postgresql)
