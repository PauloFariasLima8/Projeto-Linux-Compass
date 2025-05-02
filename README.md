# Projeto-Linux-Compass

# Documentação do Projeto: Site com Docker e Nginx

Este documento descreve o processo de criação de um site estático utilizando o Jekyll, sua integração com Docker no GitHub Actions e a configuração do Nginx para servir o site gerado.

---

## 1. Criação do Docker no GitHub

### 1.1. Configuração do Workflow no GitHub Actions
O workflow do GitHub Actions é configurado para gerar os arquivos do site estático utilizando o Jekyll dentro de um contêiner Docker. O arquivo de configuração do workflow deve ser salvo no diretório `.github/workflows/dockerfile.yml`.

#### Exemplo de Configuração:
```yaml
name: Jekyll site CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
    - name: Build the site in the jekyll/builder container
      run: |
        docker run \
        -v ${{ github.workspace }}/Docker:/srv/jekyll \
        -v ${{ github.workspace }}/Docker/_site:/srv/jekyll/_site \
        jekyll/builder:latest /bin/bash -c "chmod -R 777 /srv/jekyll && jekyll build --future"
        '''
        1.2. Passos do Workflow
Checkout do Código: O repositório é clonado no ambiente do GitHub Actions.
Execução do Jekyll: O comando jekyll build é executado dentro de um contêiner Docker para gerar os arquivos do site na pasta _site.
1.3. Como Configurar
Crie o arquivo dockerfile.yml no diretório .github/workflows/.
Faça o commit e o push para o branch main do repositório.
O workflow será acionado automaticamente em cada push ou pull request.
2. Instalação do Nginx para Executar o Site
Após gerar os arquivos do site com o Jekyll, utilizamos o Nginx para servir os arquivos estáticos.

2.1. Criação do Dockerfile
Crie um arquivo chamado Dockerfile no diretório do projeto para configurar o contêiner com Nginx.

Exemplo de Dockerfile:
2.2. Gerar os Arquivos do Jekyll
Antes de construir a imagem Docker com Nginx, certifique-se de que os arquivos do Jekyll foram gerados na pasta _site. Execute o seguinte comando:

2.3. Construir a Imagem Docker
Após gerar os arquivos do Jekyll, construa a imagem Docker com o seguinte comando:

2.4. Executar o Contêiner
Inicie o contêiner com Nginx para servir o site:

O site estará acessível em http://localhost:8080.
3. Resumo do Processo
Criação do Workflow no GitHub Actions:

Configurado para gerar os arquivos do site com Jekyll dentro de um contêiner Docker.
Os arquivos gerados são armazenados na pasta _site.
Configuração do Nginx:

Um Dockerfile é criado para configurar o Nginx como servidor web.
Os arquivos do site são copiados para o diretório padrão do Nginx (/usr/share/nginx/html).
O contêiner é iniciado e o site é servido na porta 8080.
Se precisar de mais informações ou ajustes, é só avisar!

