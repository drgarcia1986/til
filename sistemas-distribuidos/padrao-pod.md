# Padrão de arquitetura POD

> **P**oint **O**f **D**elivery.

_PoD_ é um padrão de arquitetura que representa um conjunto bem definido e reproduzível de recursos de **rede**, **storage** e **aplicação**
que representa a unidade básica de uma solução de software.
A idéia é que esse conjunto de recursos seja entregue de forma uniforme, assim como no processo de scale up/down,
atuando como uma abstração da unidade básica da aplicação.

Um exemplo de _PoD_ poderia ser algo como:

```
+-----------+    +----------+     +----------+ 
|  Nginx    |----|   App    |-----|  Logger  | 
+-----------+    +----------+     +----------+ 
      |                                       
      |                                        
      |                                        
      |   +----------+                         
      +---|  static  |                         
          +----------+                         
                           
```

* O _Nginx_ tem acesso ao _App_ através de rede, ou através de file system (para quando utilizar com Unix Sockets).
* O _Nginx_ tem acesso aos _statics_ através de file system proporcionando assim um cada de cache para arquivos estáticos.
* O _Logger_ tem acesso aos logs do _App_ através do file system ou rede utilizando UDP por exemplo.

Esse padrão é utilizado no Kubernentes com o mesmo nome de [POD](https://cloud.google.com/kubernetes-engine/docs/concepts/pod), 
porém no k8s esse padrão é utilizado para representar uma única réplica de uma aplicação, enquanto que o padrão
pode ser utilizado para representar a abstração de uma solução de software completa e complexa.

## Referências
* [Egnyte Architecture](http://highscalability.squarespace.com/blog/2019/11/25/egnyte-architecture-lessons-learned-in-building-and-scaling.html)
* [Better Data Center Standardization Through Pod Architecture Design](https://www.networkcomputing.com/data-centers/better-data-centers-standardization-through-pod-architecture-design)
