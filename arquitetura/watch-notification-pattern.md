# Watch Notification Pattern
O _watch notification pattern_ é um padrão de notificação e broadcasting onde um serviço se registra
a um serviço central de notificações (ou _broker_) e a cada evento ocorrida no contexto de desse
serviço central, esse se encarrega de notificar todos os serviços nele registrados.

Pode se considerar que o _watch notification pattern_ se assemelha bastante ao Design Pattern [Observer](https://refactoring.guru/pt-br/design-patterns/observer)
mas de forma distribuída.

Outra referência fica por conta do padrão [Fan Out](../concorrencia/fan-in-e-fan-out.md), visto que
o modo de notificação se dá em broadcasting para todos os serviços registrados.

## Exemplo de uso
Um exemplo simples de uso desse padrão seria um _serviço de configurações distribuídas_ onde um conjunto de
configurações (_feature toggles_ por exemplo) ficaria centralizado e gerenciado por esse serviço
e esse permitiria que outros serviços se registrassem para receber eventos de atualização dessas configurações.

_serviço se registra para receber eventos de atualização_
```
+------------+                 +--------+
|  service1  |-----------------+ config |
+------------+                 + server |
                               +--------+
```

_serviço de configurações notifica os serviços nele registrados_
```
                                 +------------+
                     +-----------+  service1  |
                     |           +------------+
                     |                         
                     |                         
+----------+         |           +------------+
|  config  +---------+-----------+  service2  |
|  server  +         |           +------------+
+----------+         |                         
                     |                         
                     |           +------------+
                     |-----------+  service3  |
                                 +------------+
```

## Referências
* [Lesson 35 - Watch Notification Pattern](https://www.developertoarchitect.com/lessons/lesson35.html)
