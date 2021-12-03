## Arquivos docker para Laravel

Projeto contendo apenas o docker-compose.yml e Dockerfile para o setup incial de um projeto laravel com banco relacional postgres e nginx.

Copie os arquivos para o seu projeto Laravel para configurá-lo.

## Observações e setup

#### docker-compose.yml

1. No arquivo docker-compose.yml estão os serviços que o docker usará para configurar o seu projeto. Note que no service 'nginx' estamos consumindo a porta 9099 do seu localhost. Certifique que esta porta esteja disponível ou mude a porta.

```
docker-compose.yml

ports:
  - "9099:80"
```

2. No mesmo arquivo existem dois argumentos de usuário que você precisa trocar, de acordo com o usuario da sua máquina. Essa configuração se dá para dentro do container todas as pastas e arquivos gerados pelo composer e artisan pertencerem ao mesmo usuario, e não ao root.

```
build:
  args:
    user: jose_santos
    uid: 1001
```

#### Dockerfile

1. O arquivo Dokcerfile é responsável por buildar e criar a imagem 'laravel'. Nela estamos usando php 8.0. Certifique-se que seu projeto suporta esta versão ou mude no arquivo.
```
Dockerfile

FROM php:8.0-fpm
```


2. No Dokcerfile estão sendo instaladas as extensões básicas para rodar um projeto laravel. Caso precise de mais alguma, configure no arquivo.
```
# Install PHP extensions
RUN apt-get install -y libpq-dev \
  && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
  && docker-php-ext-install pdo pdo_pgsql pgsql mbstring exif pcntl bcmath gd
```


### Subir a aplicação

Uma vez copiado os arquivos para a raiz do seu projeto laravel, rode os comandos para subir e configurar sua aplicação:

```
docker-compose build && docker-compose up -d
```
Será montada a imagem do laravel e subir os serviços da aplicação.

Dentro do container do laravel, você pode rodar os comandos artisan e do composer. Exemplo:

```
docker exec -it laravel-app bash
composer install
```


### Encerrar aplicação

Caso queira encerrar a aplicação, basta rodar o comando dentro da pasta:

```
docker-compose down && docker rmi laravel
```
