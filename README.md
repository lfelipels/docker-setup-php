Ambiente PHP com Docker Compose
==================================
Configuração de container docker utilizando docker compose para criação de ambiente de desenvolvimento PHP. 

## Serviços inclusos: #

- Postgres
- PHP-FPM contendo os pacotes: 
    - PHP v7.4
    - node v13
    - npm
    - composer
    - git
    - cURL
    - Vim
- NGINX

## Adicione ao seu projeto #

Simplesmente baixe e descompacte o arquivo na raiz do seu projeto, isso criará um arquivo `docker-compose.yml` e uma pasta chamada` phpdocker` contendo os arquivos de configurações dos serviços nginx e php-fpm.

Verifique se a configuração do servidor web no arquivo `phpdocker/nginx/nginx.conf` está correta para o seu projeto. 

A pasta root do servidor web é `public/index.php`.

Você poderá alterar as configurações do servidor php no arquivo `phpdocker\php-fpm\php-ini-overrides.ini`

Nota: você pode colocar os arquivos em outro local do seu projeto. Certifique-se de modificar os locais para o dockerfile do php-fpm e a configuração do nginx no `docker-compose.yml`, se você o fizer.

## Iniciando os serviços #

Entre na pasta do seu projeto e execute `docker-compose up -d` para criação dos contêineres.

## Acessando os serviços #

Você pode acessar sua aplicação via **`localhost`**, se estiver executando os contêineres diretamente. O servidor nginx responde a qualquer nome de host, caso você queira adicionar seu próprio nome de host no seu `/etc/hosts`.

Para acessar um contêiner use `docker-compose exec <nome_serviço> bash`.

Serviços|Acesso|
|------|---------|
|Webserver|[localhost:8080](http://localhost:8080)|
|Postgres | Qualquer cliente de banco de dados ou diretamente via `docker-compose exec postgres psql -U <usuario>`| 


## Comando úteis do docker compose #

  * Iniciando contêineres: `docker-compose up -d`
  * Parando contêineres: `docker-compose stop`
  * Matando contêineres: `docker-compose kill`
  * Visualizando os logs dos contêineres: `docker-compose logs`
  * Executando comandos dentro dos contêineres: `docker-compose exec NOME_SERVICO COMANDO` onde `COMANDO` é o que será executado no contêiner. Exemplos:
    * Abrindo shell do contêiner PHP: `docker-compose exec php-fpm bash`
    * Criando o arquivo package.json com npm:  `docker-compose exec php-fpm npm init -y`
    * Abrindo shell do postgres, `docker-compose exec postgres psql -U <usuario>`

## Recomendações #

É difícil evitar problemas de permissão de arquivo quando se brinca com os contêineres, devido ao fato de que, do ponto de vista do sistema operacional, todos os arquivos criados no contêiner pertencem ao processo que os criou. Para evitar dores de cabeça siga algumas regras simples:

Execute o composer fora do contêiner php, pois isso instalaria todas as suas dependências pertencentes ao seu usuário.
Execute comandos (por exemplo, o console do Symfony ou o artisan do Laravel) diretamente dentro do seu contêiner. Você pode facilmente abrir um shell como descrito acima e fazer o que quiser a partir daí.
