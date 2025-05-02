# Projeto Compass

Este projeto é uma aplicação simples que utiliza o Nginx como servidor web. O objetivo é demonstrar como configurar um ambiente Docker para servir uma página HTML.

## Estrutura do Projeto

O projeto possui a seguinte estrutura de diretórios:

```
ProjetoCompass
├── Dockerfile
├── nginx
│   ├── nginx.conf
├── src
│   └── index.html
└── README.md
```

## Arquivos do Projeto

- **Dockerfile**: Contém as instruções para construir a imagem Docker. Define a imagem base do Nginx, copia os arquivos de configuração e o conteúdo do site para o diretório apropriado.

- **nginx/nginx.conf**: Configuração do servidor Nginx. Define as regras de roteamento, configurações de servidor e outras opções necessárias para o funcionamento do Nginx.

- **src/index.html**: Página HTML principal do projeto. Contém o conteúdo que será servido pelo Nginx.

## Instruções para Construir e Executar o Contêiner Docker

1. Certifique-se de ter o Docker instalado em sua máquina.

2. Navegue até o diretório do projeto:

   ```
   cd ProjetoCompass
   ```

3. Construa a imagem Docker:

   ```
   docker build -t projeto-compass .
   ```

4. Execute o contêiner Docker:

   ```
   docker run -d -p 80:80 projeto-compass
   ```

5. Acesse a aplicação no seu navegador em `http://localhost`.

## Contribuições

Sinta-se à vontade para contribuir com melhorias ou correções.