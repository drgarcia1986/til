# Thundering Herd Problem
O _Thundering Herd Problem_ (em português podemos chamar de _efeito de estouro de manada_) consiste no
efeito onde diversos request competem simultaneamente pelo mesmo recurso causando um nível de pressão
elevado ao serviço que entrega esse recurso.

Alguns exemplos de cenários onde isso pode acontecer:

* Spike agressivos de requests em uma API.
* Expiração massiva de cache (causando spikes no serviço alvo).
* Fechamento de circuito de circuit breakers.
* Etc.

Algumas estratégias podem ser empregadas para minimizar esse efeito.

## Request Coalescing
Consiste em um padrão que emprega uma espécie de _"sala de espera"_, onde um único
request é autorizado a efetivamente acessar o recurso no serviço alvo, enquanto que os outros requests
esperam que esse primeiro seja concluído e o recurso esteja em cache.

## Jitter
Consiste em uma estratégia que emprega intervalos randômicos.

#### Em caches
Podemos empregar estratégias de _jitter_ em regra de expiração de cache, fazendo com que
diferentes chaves de cache expirem em momentos diferentes, minimizando a necessidade de acesso
massivo aos recursos no serviço alvo.

#### Em exponential backoff
Outra forma de _jitter_ é empregar intervalos randômicos em uma estratégia de exponential backoff,
onde as retentativas de request ocorrem em intervalos randômicos, evitando assim a sincronia entre
essas chamadas.

## Referências
* [How Facebook Live Streams To 800,000 Simultaneous Viewers](http://highscalability.com/blog/2016/6/27/how-facebook-live-streams-to-800000-simultaneous-viewers.html)
* [The Thundering Herd Problem](https://www.eximiaco.tech/en/2019/05/28/the-thundering-herd-problem/)
* [Nginx: Proxy Cache Lock](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_lock)
* [Exponential Backoff and Jitter](https://aws.amazon.com/pt/blogs/architecture/exponential-backoff-and-jitter/)
