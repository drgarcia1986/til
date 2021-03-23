# Pegando informações sobre janelas do X

O comando `xprop` pode ser utilizado para capturar informações sobre janelas
ativas na interface gráfica (_x_) do linux. Por exemplo, para capturar informações
sobre a janela que está com foco, pode ser utilizado o seguinte comando:

```bash
$ xprop -root _NET_ACTIVE_WINDOW
```

Já para capturar informações sobre uma janela especifica, o comando a seguir por ser
utilizado:

```bash
$ xprop -id $ID_DA_JANELA WM_NAME
```
O comando anterior retona o _nome_ da janela informada através do argumento `-id`.

## Referências
* [xprop(1)](https://linux.die.net/man/1/xprop)
