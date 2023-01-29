FROM php:7.4-fpm AS php-7.4

#estou puxando alguns argumentos que já existem ali no docker-composer.yml
ARG user
ARG uid

#aqui estou copiando o arquivo Composer (que já é pra ter por default nessa imagem) para a pasta /usr/bin/composer para que eu consiga utilizar o comando "composer" de forma global!
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

#aqui estou rodando os comandos / instalando as dependencias que vou precisar que existam nessa imagem
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    #abaixo eu estou limpando todos os dados da instalção / os lixos que foram criados na instalação e o cache...
    &&  apt clean \
    &&  rm -rf /var/lib/apt/lists/*  \ 
    #abaixo estou instalando algumas extensões em PHP:
    &&  docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd sockets \
    #abaixo vou criar o usuário definido/criando acima nos ARG (que por sua vez está puxando do docker-composer.yml) e definindo que esse usuario vai poder rodar o Composer e Artisan comands...
    &&  useradd -G www-data,root -u $uid -d /home/$user $user \
    &&  mkdir -p /home/$user/.composer \
    &&  chown -R $user:$user /home/$user \
    #instalando o redis:
    &&  pecl install -o -f redis \
    #abilitando as extensões:
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis


#aqui eu estou definindo qual é o diretório de trabalho: (observar que deve ser o mesmo lugar que foi definido ali no docoker-composer.yml no parâmetro working_dir)
WORKDIR /var/www

#aqui eu estou definindo qual usuário que irá 'executar' esse container!
USER $user