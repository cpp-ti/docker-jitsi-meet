# Iniciando o jitsi 

Para iniciar este container basta:

### Editar variáveis:

Copie o arquivo `.env.example` para `.env` e edite conforme as necessidades.

### Gerar as senhas seguras:

Roda o comando abaixo para gerar as senhas seguras:

`./gen-passwords.sh`

### Iniciar:

Inicie o jitsi com o comando abaixo se for utilizar ele no `traefik`:

`docker-compose -f docker-compose.traefik.yml up -d --build`

# Instalando módulos personalizáveis:

Para instalar os seus próprios módulos, faça um link da sua pasta de módulos com a pasta de módulos deste container, exemplo:

`sudo ln -s ~/projetos/conferencia/prosody ~/.jitsi-meet-cfg/prosody/prosody-plugins-custom`

Após isso, edite o arquivo `~/.jitsi-meet-cfg/prosody/config/conf.d/jitsi-meet.cfg.lua` e adicione o nome do seu módulo na lista de módulos ativados:

```
Component "muc.meet.jitsi" "muc"
    ...
    modules_enabled = {
        ...
        "nome_do_seu_modulo";
        ...
    }
    ...
```

Após isso recrie o container do `prosody`:
```shell script
docker-compose stop prosody
docker-compose -f docker-compose.traefik.yml up -d --build prosody
```


## Sobrescrever assets

Antes de subir o container, editar o arquivo `web/rootfs/defaults/meet.conf` e adicionar:

```
location = /libs/lib-jitsi-meet.min.js {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/lib-jitsi-meet.min.js;
}

location = /libs/lib-jitsi-meet.min.map {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/lib-jitsi-meet.min.map;
}

location = /lang/main-ptBR.json {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/main-ptBR.json;
}

location = /css/all.css {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/all.css;
}

location = /libs/app.bundle.min.js {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/app.bundle.min.js;
}

location = /libs/app.bundle.min.map {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/app.bundle.min.map;
}

location = /external_api.min.js {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/external_api.min.js;
}

location = /external_api.js {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/external_api.min.js;
}

location = /external_api.min.map {
  proxy_pass https://conferencia.cpp-ti.com.br/jitsi/external_api.min.map;
}
```

Se já subiu o container editar o arquivo `sudo nvim ~/.jitsi-meet-cfg/web/nginx/meet.conf`.

Reiniciar o container web: `docker-compose restart web`.

# Jitsi Meet on Docker

![](resources/jitsi-docker.png)

[Jitsi](https://jitsi.org/) is a set of Open Source projects that allows you to easily build and deploy secure videoconferencing solutions.

[Jitsi Meet](https://jitsi.org/jitsi-meet/) is a fully encrypted, 100% Open Source video conferencing solution that you can use all day, every day, for free — with no account needed.

This repository contains the necessary tools to run a Jitsi Meet stack on [Docker](https://www.docker.com) using [Docker Compose](https://docs.docker.com/compose/).

## Installation

The installation manual is available [here](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker).

## TODO

* Support container replicas (where applicable).
* TURN server.

