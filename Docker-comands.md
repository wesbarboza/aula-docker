Docker!
# Helps
docker --help

## Instalar o docker

curl -fsSL https://get.docker.com/ | bash
sudo usermod -aG docker cloud_user


docker container --help

# Listar containers
docker container ls

docker ps  (listar containers em execução)

docker ps -a (listar todos containers)

# Start container
docker container start idcontainer

# baixando imagem> 

docker pull nginx  >> sempre ira baixar a latest

docker pull nginx:tag, por exemplo a versão



# listar imagem que está no server

docker image ls

# deletar imagem
docker image rm id da imagem

# inspecionar imagem
docker image inspect ID ou nome

# criar imagem baseada em uma ja baixada
docker image tag nginx:1.21.3 data-nginx






 docker run --name web -dt nginx


 # acessando porta do container

 docker run -d -e NGINX_ENTRYPOINT_QUITE_LOGS=1 nginx:1.19.4-alpine

 docker run -d -p 8080:80 nginx:1.19.4-alpine 

 docker ps (listar)

 docker rm (id) -f //remover


 # criar volumes (persistindo dados no container)

 docker run --name linux_alpine --rm -i -t alpine:3.12.1 sh (entra direto no sh do container )

ctrl D (sai do container)



docker run -v "/var/www" --name (dentro do container o volume)


docker exec -it namecontainer sh (entra no container)

cd /var/www - criar arquivo

docker container stop id
docker container rm id


docker run --name "servidor_wev" -d -p 8080:80 -e NGINX_ENTRYPOINT_QUITE_LOGS=1 -v "/home/local:/usr/share/nginx/html" nginx:1.19.4-alpine

 # inspecionando informações

 docker inspect id



  // monts

  ## Seção 7 - Dockerfile

  Dockerfile >>> Build >>> Imagem >>> Run >>>> Container


  Dockerfile

  ou se tiver mais que um, especificar

  postgres.dockerfile
  servidor_web.dockerfile   

inicio do arquivo:
```
FROM nginx:1.19.4-alpine 
LABEL maintainer="Weslley Barboza<wes.aparecido@gmail.com>"
COPY . /usr/share/nginx/html
EXPOSE 80
```


docker build -f Dockerfile -t usernamedockerhub/servidor_web:v1 .

//por padrão o build procura o arquivo Dockerfile, nao precisaria especificar com o -f
-t tag nomedeusuario do dockerhub / nomedaimagem :tag=v1

docker image ls
REPOSITORY                TAG             IMAGE ID       CREATED         SIZE
wesbarboza/servidor_web   v1              a22b2f49180a   2 minutes ago   21.8MB
httpd                     2.4             1132a4fc88fa   5 days ago      143MB
nginx                     1.19.4-alpine   e5dcd7aa4b5e   11 months ago   21.8MB


- executar
```
docker run --name "servidor_web" -d -p 8080:80 wesbarboza/servidor_web:v1
```

# Publicando imagem no dockerhub

docker login --username=wesbarboza

docker image ls
REPOSITORY                TAG             IMAGE ID       CREATED          SIZE
wesbarboza/servidor_web   v1              a22b2f49180a   11 minutes ago   21.8MB

docker push wesbarboza/servidor_web:v1


docker pull wesbarboza/servidor_web:v1

FROM nginx:1.19.4-alpine 
LABEL maintainer="Weslley Barboza<wes.aparecido@gmail.com>"
COPY . /usr/share/nginx/html
WORKDIR /var/www
RUN comandos apt update && apt etc....


ssh cloud_user@35.86.213.136
PNPX0401amqf

## Seção 8 - Redes no Docker

Configuração é a bridge >>> container <> Host

docker run --rm  alpine:3.12.1 sh -c "ifconfig" //crie um container faça algo e seja removido

```
Unable to find image 'alpine:3.12.1' locally
3.12.1: Pulling from library/alpine
188c0c94c7c5: Pull complete
Digest: sha256:c0e9560cda118f9ec63ddefb4a173a2b2a0347082d7dff7dc14272e7841a5b5a
Status: Downloaded newer image for alpine:3.12.1
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:200 (200.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

cloud_user@12922475571c:~$ docker run --net host  --rm  alpine:3.12.1 sh -c "ifconfig"
docker0   Link encap:Ethernet  HWaddr 02:42:C9:EE:ED:AF
          inet addr:172.17.0.1  Bcast:172.17.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:c9ff:feee:edaf/64 Scope:Link
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:3 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:306 (306.0 B)

ens5      Link encap:Ethernet  HWaddr 0E:6F:18:10:FA:4D
          inet addr:172.31.28.114  Bcast:172.31.31.255  Mask:255.255.240.0
          inet6 addr: 2600:1f14:ec4:9803:74ea:73f4:3444:a386/128 Scope:Global
          inet6 addr: fe80::c6f:18ff:fe10:fa4d/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:4099 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2279 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3428322 (3.2 MiB)  TX bytes:379497 (370.6 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:639 errors:0 dropped:0 overruns:0 frame:0
          TX packets:639 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:57478 (56.1 KiB)  TX bytes:57478 (56.1 KiB)



docker network ls

# Inspecionando o driver bridge
docker network inspect bridge

```
[
    {
        "Name": "bridge",
        "Id": "22d565f7dceeed7a66e7bfe3cfbaa595d66d287275369538ff211ebed1ad5e15",
        "Created": "2021-10-27T02:23:43.584220522Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

# Criando dois containers  e verificando rede
docker run --name "container_1_bridge" -d alpine:3.12.1 sleep 5000 //sleep 5000 
docker run --name "container_2_bridge" -d alpine:3.12.1 sleep 5000 

docker exec -it container_1_bridge ifconfig

eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:26 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:3170 (3.0 KiB)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)


cloud_user@12922475571c:~$ docker exec -it container_2_bridge sh
/ # ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.120 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.094 ms


docker network ls
docker inspect bridge

# Criar outra bridge para uma nova faixa de ips
docker network create --driver bridge outra_faixa


criando um outro container utilizando esta nova faixa de ips criados

docker run --name "container_3_bridge_nova_faixa" -d --net outra_faixa alpine:3.12.1 sleep 5000

# Verificando rede do container criado na outra faixa
cloud_user@12922475571c:~$ docker exec -it "container_3_bridge_nova_faixa" ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:44 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:6240 (6.0 KiB)  TX bytes:0 (0.0 B)



# Criando comunicação entre as duas redes

docker network connect bridge container_3_bridge_nova_faixa

# testando 

docker exec -it container_3_bridge_nova_faixa ifconfig 

// cria a bridge entre a rede docker e a nova brige
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:c9:ee:ed:af brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0


# Para desconectar
docker network disconnect bridge container_3_bridge_nova_faixa


## Acessando banco de dados

subir a pasta fusion



POSTGRES.DOCKERFILE


fusion.dockerfile

FROM python:3.9.0-alpine
LABEL maintainer "Geek University <contato@geekuniversity.com.br>"
COPY . /var/www
WORKDIR /var/www
RUN apk update && apk add zlib-dev jpeg-dev gcc musl-dev python3-dev postgresql-dev && pip install -r requirements.txt && python manage.py migrate
ENTRYPOINT python manage.py runserver 0.0.0.0:8000
EXPOSE 8000




FROM postgres:13.1-alpine
LABEL mainteiner "Wes"
ENV POSTGRES_USER=fusion_user
ENV POSTGRES_PASSWORD=fusion_pass
ENV POSTGRES_DB=fusion_app
EXPOSE 5432


# Buildar a imagem do pgfusion
docker build -f pgfusion.dockerfile -t guniversity/pgfusion:v1 .

# Subir container
docker run --name "pgfusion" -d guniversity/pgfusion:v1


# Buildar a imagem do fusion
docker build -f fusion.dockerfile -t guniversity/fusion:v1 .

# Subir container
docker run --name fusion -d -p 8080:8000 guniversity/fusion:v1


# Criar usuario no banco

docker exec -it fusion sh

puthon manage.py createsuperuser



## Seção 9 - Docker Compose

baixar exemplo fusion-incio

criar docker-compose.yml

```yaml
version: "3.8"

services:
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: wesbarboza/nginx_fusion:v1
    container_name: nginx
    ports:
      - "8080:80"
    networks:
      - nwfusion
    depends_on:
      - fusion1
      - fusion2
      - fusion3
  pgfusion:
    build:
      dockerfile: ./docker/pgfusion.dockerfile
      context: .
    image: wesbarboza/pgfusion:v1
    container_name: pgfusion
    ports:
      - "5432"
    networks:
      - nwfusion
    volumes:
      - pgdata:/var/lib/postgresql/data

  fusion1:
    build:
      dockerfile: ./docker/fusion.dockerfile    
      context: .
    image: wesbarboza/fusion:v1
    container_name: fusion1
    ports:
      - "8000"
    networks:
      - nwfusion
    depends_on:
      - pgfusion
  
  fusion2:
    build:
      dockerfile: ./docker/fusion.dockerfile    
      context: .
    image: wesbarboza/fusion:v1
    container_name: fusion2
    ports:
      - "8000"
    networks:
      - nwfusion
    depends_on:
      - pgfusion
  
  fusion3:
    build:
      dockerfile: ./docker/fusion.dockerfile    
      context: .
    image: wesbarboza/fusion:v1
    container_name: fusion3
    ports:
      - "8000"
    networks:
      - nwfusion
    depends_on:
      - pgfusion            


networks:
  nwfusion:
    driver: bridge

volumes:
  pgdata:
```

no mesmo diretorio do arquivo do docker compose

# buildar as imagens
docker-compose build

# visualizar as imagens

cloud_user@12922475571c:~/fusion$ docker image ls
REPOSITORY                TAG             IMAGE ID       CREATED              SIZE
wesbarboza/nginx_fusion   v1              729163b6260b   10 seconds ago       42.3MB
wesbarboza/fusion         v1              0f6e6dcf5d2c   17 seconds ago       616MB
wesbarboza/pgfusion       v1              66ab9ccda952   About a minute ago   159MB
guniversity/fusion        v1              eeb7fbf61e59   About an hour ago    595MB
guniversity/pgfusion      v1              9bcab115314b   About an hour ago    159MB
<none>                    <none>          2c61fac548d7   2 hours ago          54.8MB
<none>                    <none>          37a4551a028c   2 hours ago          159MB
postgres                  latest          317a302c7480   25 hours ago         374MB
postgres                  13.1-alpine     8c6053d81a45   9 months ago         159MB
python                    3.9.0-alpine    c0c46556c6d3   10 months ago        46.2MB
nginx                     1.19.4-alpine   e5dcd7aa4b5e   11 months ago        21.8MB
alpine                    3.12.1          d6e46aa2470d   12 months ago        5.57MB


docker-compose up -d 


cloud_user@12922475571c:~/fusion$ docker-compose up -d
Creating network "fusion_nwfusion" with driver "bridge"
Creating volume "fusion_pgdata" with default driver
Creating pgfusion ... done
Creating fusion1  ... done
Creating fusion2  ... done
Creating fusion3  ... done
Creating nginx    ... done


docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS                                            NAMES
7de5d3f4fcbd   wesbarboza/nginx_fusion:v1   "nginx -g 'daemon of…"   16 seconds ago   Up 14 seconds   443/tcp, 0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx
93682e64b04f   wesbarboza/fusion:v1         "/bin/sh -c 'gunicor…"   19 seconds ago   Up 16 seconds   0.0.0.0:49155->8000/tcp, :::49155->8000/tcp      fusion1
cba57105cad7   wesbarboza/fusion:v1         "/bin/sh -c 'gunicor…"   19 seconds ago   Up 16 seconds   0.0.0.0:49156->8000/tcp, :::49156->8000/tcp      fusion3
1ffef76ea773   wesbarboza/fusion:v1         "/bin/sh -c 'gunicor…"   19 seconds ago   Up 16 seconds   0.0.0.0:49154->8000/tcp, :::49154->8000/tcp      fusion2
a9c251dd2c94   wesbarboza/pgfusion:v1       "docker-entrypoint.s…"   20 seconds ago   Up 19 seconds   0.0.0.0:49153->5432/tcp, :::49153->5432/tcp      pgfusion


ao entrar na pagina ocorreu erro

executar comando:

docker exec -it fusion1 python manage.py migrate

docker-compose down //para todos os containers

## Docker e Microserviços

Escalabilidade 

Vertical (recursos e capacidade da maquina que estamos utilizando)
horizontal: quanto replicamos a mesma maquina ou serviço

Aplicação Monolítica

toda base de codico contida em um só lugar, bloco dividido em 3 partes (apresentação, business, data access)


## Docker Cloud

alterar docker-compose.yml

ports nginx >>>> 80:80


docker-compose build

docker-compose -d 