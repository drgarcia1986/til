# Tombstone
Em bancos de dados distribuídos que usam o padrão de _consistência eventual_, `tombstone` é o nome utilizado ao se referir a um registro que foi supostamente removido.
Em cenários onde a deleção do registro não foi totalmente propagada para todos os nós do cluster, esse registro se torna um _tombstone_, ou seja, um objeto temporário indicando que esse registro deve ser removido de todos os nós do cluster.
Esse objeto garante a consistência dos dados evitando que esse registro seja retornado em listagens e não permitindo a edição do mesmo (mesmo quem em algum nó do cluster o registro ainda esteja visível)
A regra de remoção total do _tombstone_ vai de acordo com o sistema distribuído, pode ser por exemplo após o dado estar sincronizado em todos os nós do cluster, ou mesmo após um tempo determinado.

## Referências
- [Distributed Systems 5.1: Replication](https://youtu.be/mBUCF1WGI_I)
