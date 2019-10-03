# Ambiente de desenvolvimento docker da Seraph 

Seus projetos devem estar dentro da pasta www

Execute os seguintes comandos:

- cp .env-example .env
- docker-compose up -d

Quando alterar alguma informação no arquivo .env execute os seguintes comandos

- docker-compose stop nome-do-servico
- docker-compose build --no-cache nome-do-servico
- docker-compose up -d nome-do-servico

Se alterar alguma configuração do apache rode apenas

- docker-compose stop nome-do-servico
- docker-compose build nome-do-servico
- docker-compose up -d nome-do-servico

** Os nomes dos seviços são definidos dentro do docker-compose.yml que atualmente são: laravel, cakephp, mysql

