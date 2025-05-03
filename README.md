# Projeto-Linux-Compass

# Documentação do Projeto: Site com Docker e Nginx

Este documento descreve o processo de criação de um site estático utilizando html, css, javascript e Bootstrap, a criação de uma Docker com NGINX para executar o site localmente e a utilização de ferramentas como UpTimeReboot com Webhook para verificar se o site está offline e notificar via Discord. Para o desenvolvimento do trabalho foi usado um Sistema OS Linux de Base Debian. Desta forma, o primeiro procedimento será a instalação do Debian em uma Máquina Virtual - VM.

## Sumário
1. [Introdução](#documentação-do-projeto-site-com-docker-e-nginx)
2. [Instalação do Debian em uma Máquina Virtual](#instalação-do-debian-em-uma-máquina-virtual)
3. [Configuração do Ambiente de Desenvolvimento](#configuração-do-ambiente-de-desenvolvimento)
4. [Desenvolvimento do Site Estático](#desenvolvimento-do-site-estático)
5. [Criação e Execução do Docker](#criação-e-execução-do-docker)
6. [Monitoramento com UpTimeReboot e Webhook](#monitoramento-com-uptimereboot-e-webhook)
7. [Instalação do Python e Dependências](#instalação-do-python-e-dependências)
8. [Criação do Script de Monitoramento](#criação-do-script-de-monitoramento)

---

## Instalação do Debian em uma Máquina Virtual

1. **Download da Imagem ISO do Debian**  
   - Acesse o site oficial do Debian ([https://www.debian.org/](https://www.debian.org/)) e baixe a imagem ISO mais recente.

2. **Configuração da Máquina Virtual**  
   - Utilize um software como VirtualBox ou VMware.  
   - Crie uma nova VM com as seguintes configurações mínimas:  
     - **Memória RAM:** 2 GB  
     - **Disco Rígido:** 20 GB  
     - **Processador:** 2 núcleos  
   - Conecte a imagem ISO baixada à VM.

3. **Instalação do Debian**  
   - Inicie a VM e siga o assistente de instalação do Debian.  
   - Configure o idioma, fuso horário e particionamento do disco.  
   - Instale o sistema com o ambiente mínimo necessário.

---

## Configuração do Ambiente de Desenvolvimento

1. **Atualização do Sistema**  
   - Execute os comandos abaixo para atualizar os pacotes:  
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Instalação de Ferramentas Essenciais**  
   - Instale ferramentas como `git`, `curl` e editores de texto:  
     ```bash
     sudo apt install git curl vim -y
     ```

3. **Configuração do NGINX**  
   - Instale o NGINX:  
     ```bash
     sudo apt install nginx -y
     ```
   - Configure o NGINX para servir o site estático. Edite o arquivo de configuração em `/etc/nginx/sites-available/default` para apontar para o diretório do site.

4. **Criação do Dockerfile**  
   - Crie um arquivo `Dockerfile` com o seguinte conteúdo:  
     ```dockerfile
     FROM nginx:latest
     COPY ./site /usr/share/nginx/html
     ```

---

## Desenvolvimento do Site Estático

1. **Estruturação do Projeto**  
   - A estrutura do site no projeto é organizada da seguinte forma:  
     ```
     site/
     ├── [index.html](http://_vscodecontentref_/0)          # Página principal do site
     ├── css/                # Diretório para arquivos de estilo
     │   └── styles.css      # Arquivo de estilos CSS
     ├── js/                 # Diretório para scripts JavaScript
     │   └── scripts.js      # Arquivo de scripts JS
     ├── images/             # Diretório para imagens utilizadas no site
     │   └── logo.png        # Exemplo de imagem
     └── videos/             # Diretório para vídeos (se aplicável)
         └── exemplo.mp4     # Exemplo de vídeo
     ```

2. **Criação do Conteúdo**  
   - Desenvolva o site utilizando HTML, CSS, JavaScript e Bootstrap.  
   - Certifique-se de que o site seja responsivo e funcional.

3. **Testes Locais**  
   - Abra o arquivo `index.html` no navegador para verificar o funcionamento.

4. **Visualização Online**  
   - O site criado por mim para esse projeto pode ser acessado através do seguinte link:  
     [https://paulofariaslima8.github.io/Projeto-Linux-Compass/](https://paulofariaslima8.github.io/Projeto-Linux-Compass/)

---

## Criação e Execução do Docker

1. **Construção da Imagem Docker**  
   - No diretório do projeto, execute:  
     ```bash
     docker build -t meu-site-nginx .
     ```

2. **Execução do Container**  
   - Inicie o container com o comando:  
     ```bash
     docker run -d -p 8080:80 meu-site-nginx
     ```
   - Acesse o site no navegador em `http://localhost:8080`.

---

## Monitoramento com UpTimeReboot e Webhook

1. **Configuração do UpTimeReboot**  
   - Crie uma conta no UpTimeReboot ([https://uptimerobot.com/](https://uptimerobot.com/)).  
   - Adicione um monitor para o site, configurando a URL do container.

2. **Integração com Webhook**  
   - Configure um Webhook no UpTimeReboot para enviar notificações ao Discord.  
   - No Discord, crie um Webhook no canal desejado e copie a URL.  
   - Adicione a URL do Webhook no UpTimeReboot.

3. **Testes de Monitoramento**  
   - Simule uma falha no site para verificar se as notificações estão funcionando corretamente.

---

## Instalação do Python e Dependências

1. **Atualização do Sistema**  
   - Certifique-se de que o sistema está atualizado:  
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Instalação do Python e Pip**  
   - Instale o Python 3 e o gerenciador de pacotes Pip:  
     ```bash
     sudo apt install python3 python3-pip -y
     ```

3. **Instalação da Biblioteca `requests`**  
   - Instale a biblioteca necessária para o script:  
     ```bash
     pip3 install requests
     ```

---

## Criação do Script de Monitoramento

1. **Criação do Arquivo do Script**  
   - Crie um arquivo chamado `site_monitor.py` no diretório do projeto:  
     ```bash
     nano site_monitor.py
     ```

2. **Adicione o Código do Script**  
   - Cole o seguinte código no arquivo criado:
     ```python
     #!/usr/bin/env python3
     """
     Monitor de Site com Alerta no Discord
     Autor: Seu Nome
     Data: $(date +%Y-%m-%d)
     """

     import requests
     import time
     import logging
     from datetime import datetime
     import json

     # Configurações (edite estas variáveis)
     CONFIG = {
         "site_url": "http://localhost",  # URL do site a ser monitorado
         "nginx_status_url": "http://localhost/nginx_status",  # URL do status do Nginx
         "check_interval": 60,  # Intervalo de verificação em segundos
         "timeout": 10,  # Timeout da requisição em segundos
         "discord_webhook": "https://discord.com/api/webhooks/SEU_WEBHOOK",  # Webhook do Discord
         "max_retries": 3,  # Número de tentativas antes de alertar
         "log_file": "/var/log/site_monitor.log"  # Arquivo de log
     }

     # Configuração de logging
     logging.basicConfig(
         filename=CONFIG['log_file'],
         level=logging.INFO,
         format='%(asctime)s - %(levelname)s - %(message)s',
         datefmt='%Y-%m-%d %H:%M:%S'
     )

     def check_nginx_status():
         """Verifica o status do Nginx"""
         try:
             response = requests.get(CONFIG['nginx_status_url'], timeout=CONFIG['timeout'])
             return response.status_code == 200
         except requests.RequestException as e:
             logging.error(f"Erro ao verificar Nginx: {str(e)}")
             return False

     def check_site_status():
         """Verifica se o site está respondendo"""
         try:
             response = requests.get(CONFIG['site_url'], timeout=CONFIG['timeout'])
             return response.status_code == 200
         except requests.RequestException as e:
             logging.error(f"Erro ao verificar o site: {str(e)}")
             return False

     def send_discord_alert(message):
         """Envia mensagem de alerta para o Discord"""
         payload = {
             "content": f"🚨 **ALERTA DE MONITORAMENTO** 🚨\n{message}",
             "username": "Site Monitor",
             "embeds": [{
                 "title": "Detalhes do Problema",
                 "description": message,
                 "color": 16711680,  # Vermelho
                 "timestamp": datetime.utcnow().isoformat()
             }]
         }

         try:
             response = requests.post(
                 CONFIG['discord_webhook'],
                 data=json.dumps(payload),
                 headers={"Content-Type": "application/json"}
             )
             if response.status_code != 204:
                 logging.error(f"Erro ao enviar para Discord: {response.text}")
         except Exception as e:
             logging.error(f"Falha ao enviar alerta para Discord: {str(e)}")

     def main():
         logging.info("Iniciando monitoramento...")
         failure_count = 0

         while True:
             nginx_ok = check_nginx_status()
             site_ok = check_site_status()

             if not nginx_ok or not site_ok:
                 failure_count += 1
                 logging.warning(f"Falha detectada (Tentativa {failure_count}/{CONFIG['max_retries']})")

                 if failure_count >= CONFIG['max_retries']:
                     message = ""
                     if not nginx_ok:
                         message += "❌ Nginx não está respondendo\n"
                     if not site_ok:
                         message += f"❌ Site {CONFIG['site_url']} está offline\n"
                     message += f"🕒 Última verificação: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}"

                     send_discord_alert(message)
                     failure_count = 0  # Reset após alerta
             else:
                 failure_count = 0
                 logging.info("Tudo operacional")

             time.sleep(CONFIG['check_interval'])

     if __name__ == "__main__":
         try:
             main()
         except KeyboardInterrupt:
             logging.info("Monitoramento encerrado pelo usuário")
         except Exception as e:
             logging.critical(f"Erro fatal: {str(e)}")
             send_discord_alert(f"⚠️ O monitoramento parou inesperadamente: {str(e)}")
     ```

3. **Torne o Script Executável**  
   - Dê permissão de execução ao script:  
     ```bash
     chmod +x site_monitor.py
     ```

4. **Teste o Script**  
   - Execute o script para verificar se está funcionando:  
     ```bash
     ./site_monitor.py
     ```

---


