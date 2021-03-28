# Criando um serviço do Systemd

Para criar um serviço do linux usando o [systemd](https://pt.wikipedia.org/wiki/Systemd)
basta criar um arquivo de configuração do serviço no diretório `/etc/systemd/system/`
com o nome seguido o seguinte padrão "_<nome do seu serviço>.service_", e.g:


```ini
[Unit]
Description=<descrição do serviço>

[Service]
User=<usuário que irá executar o serviço>
WorkingDirectory=<diretório do executável>
ExecStart=<comando de execução do serviço>
Restart=always

[Install]
WantedBy=multi-user.target
```

Após criar o arquivo de configuração do serviço, execute os seguintes comandos como root:

* `systemctl daemon-reload`: para recarregar a lista de serviços disponíveis.
* `systemctl start seu-servico.service`: para iniciar o serviço criado.
* `systemctl status seu-servico.service`: para obter o status de execução do serviço.
* `systemctl enable seu-servico.service`: para ativar o serviço em cada boot do SO.

## Referências
- [Creating a Linux service with systemd](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6)


