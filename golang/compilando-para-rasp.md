# Compilando para Raspberry Pi

O compilador do Go é _cross-plataform_ e com alguns váriaveis de ambiente
é possível compilar para diferentes plataformas sem que seja necessário
estar executando o compilador na plataforma alvo.

No caso de um Raspberry Pi com o seguinte sistema:

```bash
$ uname -a
Linux rasp-1 5.10.17+ #1403 Mon Feb 22 11:26:13 GMT 2021 armv6l GNU/Linux
```

Sendo o SO `linux` e a arquitetura `armv6l`, a compilação do pode se feita
da seguinte maneira:

```bash
$ GOOS=linux GOARCH=arm GOARM=5 go build
```

## Referências
* [Cross Compiling Golang Applications For Use On A Raspberry Pi](https://www.thepolyglotdeveloper.com/2017/04/cross-compiling-golang-applications-raspberry-pi/)
* [GoARM](https://github.com/golang/go/wiki/GoArm)
