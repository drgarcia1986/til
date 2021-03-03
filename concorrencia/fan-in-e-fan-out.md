# Fan-In e Fan-Out

_Fan-In_ e _Fan-Out_ são padrões de concorrência baseados em mensagens e [multiplexação e demultiplexação](https://pt.wikibooks.org/wiki/Redes_de_computadores/Multiplexa%C3%A7%C3%A3o_e_demultiplexa%C3%A7%C3%A3o).

No padrão _Fan-In_ diversas mensagens de diferentes remetentes são direcionadas a um único receptor:

```
+------------+                               
|   Sender   +---------+                     
+------------+         |                     
                       |                     
                       |                     
+------------+         |       +------------+
|   Sender   |---------+-------+  Receiver  |
+------------+         |       +------------+
                       |                     
                       |                     
+------------+         |                     
|   Sender   +---------+                     
+------------+                               
```


Enquanto que no padrão _Fan-Out_ um único remetente se encarrega de enviar a mensagem para diversos receptores.


```
                                 +------------+
                     +-----------+  Receiver  |
                     |           +------------+
                     |                         
                     |                         
+----------+         |           +------------+
|  Sender  +---------+-----------+  Receiver  |
+----------+         |           +------------+
                     |                         
                     |                         
                     |           +------------+
                     |-----------+  Receiver  |
                                 +------------+
```

Um exemplo muito popular de uso do padrão _Fan-Out_ é o padrão [PubSub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern),
enquanto que _WebBrowsers_ podem ser considerados um exemplo popular de uso do _Fan-In_.


## Referências
* [Go Concurrency Patterns: Pipelines and cancellation](https://blog.golang.org/pipelines)
