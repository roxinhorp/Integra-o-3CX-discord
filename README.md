# Bot de Integração 3CX-Discord

Este projeto recria e otimiza um bot Discord em Python que se integra com o 3CX para enviar notificações de chamadas diretamente para o DM (mensagem privada) do atendente no Discord.

## Funcionalidades

- Recebe webhooks do 3CX com detalhes de chamadas.
- Envia mensagens privadas no Discord para o atendente correspondente com as informações da chamada.
- Fácil configuração via variáveis de ambiente.

## Pré-requisitos

- Python 3.8 ou superior
- Uma aplicação de bot no Discord com as permissões necessárias (leitura de mensagens, envio de mensagens, acesso a membros do servidor).
- Um servidor 3CX configurado para enviar webhooks.

## Configuração do Bot Discord

1.  **Crie uma Aplicação de Bot no Discord:**
    *   Vá para o [Portal do Desenvolvedor do Discord](https://discord.com/developers/applications).
    *   Clique em "New Application" (Nova Aplicação).
    *   Dê um nome à sua aplicação e clique em "Create" (Criar).
2.  **Adicione um Bot à Aplicação:**
    *   Na barra lateral esquerda, clique em "Bot".
    *   Clique em "Add Bot" (Adicionar Bot) e confirme.
    *   **Copie o Token do Bot:** Clique em "Reset Token" (Redefinir Token) e copie o token gerado. **Guarde-o em segurança, ele será usado no arquivo `.env`.**
3.  **Ative os Privilégios de Gateway (Intents):**
    *   Na seção "Privileged Gateway Intents", ative as opções:
        *   `PRESENCE INTENT`
        *   `SERVER MEMBERS INTENT`
        *   `MESSAGE CONTENT INTENT`
4.  **Convide o Bot para o seu Servidor:**
    *   Na barra lateral esquerda, clique em "OAuth2" -> "URL Generator".
    *   Em "SCOPES", selecione `bot`.
    *   Em "BOT PERMISSIONS", selecione as seguintes permissões:
        *   `Send Messages` (Enviar Mensagens)
        *   `Read Message History` (Ler Histórico de Mensagens)
        *   `Manage Webhooks` (Gerenciar Webhooks) - *Opcional, mas recomendado para futuras expansões*
        *   `View Channels` (Ver Canais)
        *   `Manage Channels` (Gerenciar Canais) - *Opcional*
    *   Copie o URL gerado e cole-o no seu navegador para convidar o bot para o seu servidor Discord.

## Configuração do 3CX

O 3CX enviará os dados da chamada para o seu bot via webhook. Você precisará configurar um modelo de integração CRM.

1.  **Importe o Template XML:**
    *   Acesse o Console de Administração do 3CX.
    *   Vá em `Configurações` → `Integração de CRM`.
    *   Clique em `Adicionar` → `Importar Template`.
    *   Selecione o arquivo `3cx_discord_template.xml` fornecido neste projeto.
2.  **Preencha os Parâmetros:**
    *   **WebhookUrl:** `http://SEU_IP_PUBLICO:5000/webhook/3cx` (Substitua `SEU_IP_PUBLICO` pelo IP público ou domínio do servidor onde o bot estará rodando. A porta padrão é `5000`, mas pode ser alterada no `.env`).
    *   **WebhookSecret:** A mesma chave secreta que você definirá no seu arquivo `.env`.
3.  **Ative a Integração:** Certifique-se de ativar a integração para que o 3CX comece a enviar os webhooks.

## Instalação e Execução

1.  **Clone o Repositório (ou baixe os arquivos):**
    ```bash
    git clone <URL_DO_SEU_REPOSITORIO>
    cd 3cx-discord-bot
    ```
    (Se você baixou os arquivos, navegue até a pasta onde eles estão).

2.  **Crie o arquivo `.env`:**
    *   Renomeie `.env.example` para `.env`.
    *   Abra o arquivo `.env` e preencha as variáveis:
        ```ini
        DISCORD_TOKEN="SEU_TOKEN_DO_BOT_DISCORD"
        WEBHOOK_SECRET="SUA_CHAVE_SECRETA_PARA_WEBHOOK"
        WEBHOOK_PORT=5000
        ```
        *   `DISCORD_TOKEN`: O token que você copiou do Portal do Desenvolvedor do Discord.
        *   `WEBHOOK_SECRET`: Uma chave secreta de sua escolha para proteger seu webhook. Use a mesma chave no 3CX.
        *   `WEBHOOK_PORT`: A porta em que o servidor webhook será executado. Padrão é `5000`.

3.  **Instale as Dependências:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Execute o Bot:**
    ```bash
    python main.py
    ```

    Você deverá ver mensagens como:
    ```
    Bot logado como SeuBotDiscord#1234
    Servidor Webhook rodando na porta 5000
    ```

## Como Parar o Bot

No terminal onde o bot está rodando, pressione `Ctrl + C`.

## Como Reiniciar o Bot

1.  Pressione `Ctrl + C` para parar o bot.
2.  Após a interrupção, execute novamente: `python main.py`.

## Mapeamento de Agentes 3CX para Usuários Discord

Atualmente, o bot tenta encontrar o usuário do Discord pelo `agent_name` recebido do 3CX, comparando com o nome de exibição ou nome de usuário dos membros do servidor. Para um mapeamento mais robusto, especialmente em ambientes maiores ou com nomes não-únicos, considere implementar:

-   Um arquivo de configuração (ex: JSON, YAML) para mapear ramais 3CX para IDs de usuário Discord.
-   Um comando no Discord para os usuários se registrarem com seu ramal 3CX.

## Solução de Problemas

-   **Bot não loga:** Verifique se o `DISCORD_TOKEN` está correto e se as Intents Privilegiadas estão ativadas no Portal do Desenvolvedor do Discord.
-   **Webhook não funciona:**
    *   Verifique se o `WEBHOOK_SECRET` no `.env` e no 3CX são idênticos.
    *   Certifique-se de que o `SEU_IP_PUBLICO` está correto e que a porta `5000` (ou a porta configurada) está acessível externamente (firewall, redirecionamento de porta).
    *   Verifique os logs do bot para mensagens de erro.
-   **Bot não envia DM:** Verifique se o bot tem permissão para enviar mensagens no servidor e se o usuário do Discord não bloqueou o bot ou desativou DMs de membros do servidor.

---
