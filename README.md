# Como configurar/instalar/usar o `fail2ban` no `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para configurar/instalar/usar o `fail2ban` no `Linux Ubuntu`.

## _Abstract_

_In this document are contained the main commands and settings to set up/install/use the `fail2ban` on `Linux Ubuntu`._

## Descrição [2]

### `fail2ban`

O `Fail2ban` é uma ferramenta de segurança para sistemas baseados em `Linux` que protege contra ataques de força bruta e tentativas de intrusão. Ele monitora os logs do sistema em busca de padrões de comportamento suspeitos, como tentativas repetidas e falhas de login, e bloqueia automaticamente os endereços IP associados a essas atividades por um período determinado. Isso ajuda a proteger o sistema contra ataques de hackers e garante uma maior segurança ao restringir o acesso de potenciais invasores.

## 1. Como configurar/instalar/usar o `fail2ban` no `Linux Ubuntu` [1][3]

Para configurar/instalar/usar o `fail2ban` no `Linux Ubuntu`, você pode seguir estes passos:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando: `Ctrl + Alt + T`    

2. Certifique-se de que seu sistema esteja limpo e atualizado.

    2.1 Limpar o `cache` do gerenciador de pacotes APT. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo APT e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando: `sudo apt clean` 
    
    2.2 Remover pacotes `.deb` antigos ou duplicados do cache local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando: `sudo apt autoclean`

    2.3 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando: `sudo apt autoremove -y`

    2.4 Buscar as atualizações disponíveis para os pacotes que estão instalados em seu sistema. Digite o seguinte comando e pressione `Enter`: `sudo apt update -y`

    2.5 Para ver a lista de pacotes a serem atualizados, digite o seguinte comando e pressione `Enter`:  `sudo apt list --upgradable`

    2.6 Realmente atualizar os pacotes instalados para as suas versões mais recentes, com base na última vez que você executou `sudo apt update -y`. Digite o seguinte comando e pressione `Enter`: `sudo apt full-upgrade -y`

    2.7 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando: `sudo apt autoremove -y`

    2.8 Remover pacotes `.deb` antigos ou duplicados do cache local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando: `sudo apt autoclean`

Para instalar o `fail2ban` no `Linux Ubuntu`, você seguirá estes passos:

1. **Instalar o Fail2Ban:** Instale o pacote `fail2ban` utilizando o comando: `sudo apt install fail2ban -y`

2. **Verificar o status do serviço:** Após a instalação, verifique se o serviço `fail2ban` está ativo e rodando: `sudo systemctl status fail2ban`

3. **Iniciar o serviço Fail2Ban:** Se o serviço não estiver rodando, inicie-o com: `sudo systemctl start fail2ban`

4. **Habilitar o Fail2Ban para iniciar na inicialização:** Para que o fail2ban inicie automaticamente na inicialização do sistema, habilite-o com: `sudo systemctl enable fail2ban`

5. **Reverificar o status do serviço:** Após a habilitação, verifique se o serviço `fail2ban` está ativo e rodando: `sudo systemctl status fail2ban`


### 1.2 Configurar regras de auditoria

1. **Configurar as regras do Fail2Ban:** Crie ou edite arquivos de configuração personalizados em `/etc/fail2ban/jail.d/`. Por exemplo: `sudo nano /etc/fail2ban/jail.d/your-custom-config.conf`

    1.1 Aqui, você adicionará suas configurações personalizadas, como definir quais serviços monitorar e as ações a serem tomadas após determinado número de falhas:

    ```
    [sshd]
    enabled = true
    port = ssh
    filter = sshd
    logpath = /var/log/auth.log
    maxretry = 3
    bantime = 3600
    ```

2. **Reiniciar o serviço Fail2Ban:** Depois de alterar a configuração, reinicie o serviço para aplicar as mudanças: `sudo systemctl restart fail2ban`
Verificar novamente o status do serviço:

3. Por fim, verifique o status mais uma vez para confirmar que o `fail2ban` está ativo e rodando com as novas configurações: `sudo systemctl status fail2ban`

    Certifique-se de configurar as regras de acordo com as necessidades de segurança do seu servidor para otimizar a proteção contra tentativas de login mal-intencionadas.


### 1.3 Recebendo notificações (NÃO fazer isso para o `Kali Linux`)

Para configurar notificações sobre esses eventos de auditoria, uma abordagem simples pode ser usar um script que monitora os logs de auditoria e envia notificações quando eventos específicos ocorrem. Vou te guiar na criação de um script básico que faz isso e envia um e-mail como notificação.

#### 1.3.1 Pré-requisitos

1. **Enviar e-mails:** Para este exemplo, usaremos o `mail` para enviar e-mails. Certifique-se de que você tem um cliente de e-mail de linha de comando instalado. Se não tiver, você pode instalar o `mailutils` (que inclui o comando `mail`) no Ubuntu: `sudo apt-get install mailutils -y`

1.1 **Configurar o `mail`:** Certifique-se de que o `mail` esteja configurado para enviar e-mails. Isso pode exigir a configuração de um servidor SMTP.

1.2 Irá abrir uma tela chamada `Postfix Configuration`, selecione a opção e aperte `Enter` para confirmar: `Internet Site`

1.3 Selecione o `hostname` desejado e aperte `Enter` para confirmar. 


2. Configurar o `fail2ban` para Enviar Notificações por E-mail
Edite o arquivo de configuração do `fail2ban` para definir o endereço de e-mail para o qual as notificações devem ser enviadas e ativar as ações de e-mail.

    2.1 **Configurar o Destinatário dos E-mails:** Você precisa editar o arquivo `jail.local` ou o arquivo de configuração específico para a prisão (`jail`) que você configurou. Se `jail.local` não existir, crie-o a partir de `jail.conf`:
    
    ```
    sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
    sudo nano /etc/fail2ban/jail.local
    ```

3. **Editar as Configurações de Ação:** No arquivo `jail.local`, você pode configurar as ações para enviar e-mails. Por exemplo, para a prisão SSH, você encontrará uma linha como:

    ```
    [sshd]
    ...
    action = %(action_mwl)s
    ```

    A variável `%(action_mwl)s` é uma ação predefinida que bane o IP e envia um e-mail com quemis e log lines. Certifique-se de que as linhas `destemail` e `sendername` estão configuradas corretamente:

    ```
    destemail = seuemail@exemplo.com
    sendername = Fail2Ban
    ```

4. **Configurar o Remetente dos E-mails:** Se necessário, configure o endereço do remetente (`sender`) nos arquivos de ação do `fail2ban` localizados em `/etc/fail2ban/action.d/`. Por exemplo, no arquivo sendmail-common.local: `sudo nano /etc/fail2ban/action.d/sendmail-common.local`

    4.1 **Adicione ou altere a linha:**

    ```
    [Definition]
    sender = edendenis@gmail.com
    ```

5. **Testar as Configurações de Notificação:** Depois de configurar o `fail2ban` e o Postfix, você pode testar se as notificações por e-mail estão funcionando corretamente. Você pode forçar um banimento manualmente ou realizar uma ação que dispare um banimento para testar se o `fail2ban` envia o e-mail.

6. **Reiniciar o `fail2ban`:** Não se esqueça de reiniciar o serviço `fail2ban` após fazer alterações nas configurações: `sudo systemctl restart fail2ban`

7. **Teste a configuração novamente:** Use o `fail2ban-client` para verificar a configuração por erros: `sudo fail2ban-client -t`

8. **Reinicie o serviço Fail2Ban:** Se não houver erros relatados, tente reiniciar o serviço novamente: `sudo systemctl restart fail2ban`

9. **Verifique o status do serviço:** Após tentar reiniciar, verifique o status novamente para ver se o serviço está rodando: `sudo systemctl status fail2ban`

Com essas configurações, o `fail2ban` enviará automaticamente um e-mail de notificação sempre que realizar uma ação de banimento, sem a necessidade de um script adicional ou configurações do cron. Isso torna o fail2ban uma ferramenta poderosa e conveniente para monitorar a segurança do seu servidor em tempo real.

### 1.2 Código completo para configurar/instalar/usar

Para configurar/instalar/usar o `fail2ban` no `Linux Ubuntu` sem precisar digitar linha por linha, você pode seguir estas etapas:

1. Abra o `Terminal Emulator`. Você pode fazer isso pressionando: `Ctrl + Alt + T`

2. Digite o seguinte comando e pressione `Enter`:

    ```
    sudo apt clean
    sudo apt autoclean
    sudo apt autoremove -y
    sudo apt update -y
    sudo apt autoremove -y
    sudo apt autoclean
    sudo apt list --upgradable
    sudo apt full-upgrade -y
    sudo apt install fail2ban -y
    ```
    

## Referências

[1] OPENAI. ***Monitoramento de Acesso no Linux.*** Disponível em: <https://chat.openai.com/c/7772a507-88e7-46c8-ad7a-64a2d9011843> (texto adaptado). ChatGPT. Acessado em: 21/02/2024 12:00.

[2] OPENAI. ***Vs code: editor popular.*** Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). ChatGPT. Acessado em: 21/02/2024 12:00.

