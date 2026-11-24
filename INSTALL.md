# Guia de instalação para o agente

Este arquivo é para o **agente de código** (Claude Code ou a aba Code do Claude Desktop) guiar a pessoa na criação do webhook do Instagram. Se você é uma pessoa, o [README](https://github.com/thaleslaray/instagram-webhook-template/blob/main/README.md) é mais simples.

## Regras (valem do começo ao fim)

1. **Antes de começar**, diga à pessoa em uma mensagem o que vai acontecer e espere ela dizer "pode":
   > Vou guiar você para criar o webhook do Instagram na Cloudflare, em 5 passos. Você clica nos botões e digita a senha; eu só explico e confiro o endereço. Não vou pedir nem ver a sua senha. Posso começar?
2. **Nunca peça, leia, cole nem mostre** a senha `VERIFY_TOKEN` nem qualquer token da Meta. A pessoa digita nos campos.
3. **Nunca clique em login** nem aceite termos pela pessoa.
4. **Pare e pergunte** se qualquer coisa falhar.
5. Fale em português do Brasil, em frases curtas e sem jargão.

## Passo 1: conferir o que a pessoa tem

Pergunte se ela tem:

- conta na Cloudflare (a grátis serve, sem cartão);
- conta no GitHub;
- um app na Meta com o Instagram configurado e em modo **Live**.

Se ela não tem o app na Meta, pare. Indique o repositório https://github.com/thaleslaray/instagram-claude e a skill `/instagram-app-meta`, que criam o app. Depois ela volta aqui.

## Passo 2: o botão

Mande este link:

https://deploy.workers.cloudflare.com/?url=https://github.com/thaleslaray/instagram-webhook-template

Explique os passos 2 a 4 do README:

1. Entrar na Cloudflare e conectar o GitHub quando ela pedir.
2. No campo `VERIFY_TOKEN`, inventar uma senha sem espaços e anotar.
3. Clicar em "Create and deploy" e esperar terminar.

Peça para ela copiar o endereço que aparece (algo como `https://instagram-webhook.SEU-NOME.workers.dev`) e colar aqui. Espere. O endereço não é segredo.

## Passo 3: conferir o endereço

Rode, trocando `<endereço>` pelo que a pessoa colou:

```bash
curl -s -o /dev/null -w '%{http_code}\n' "<endereço>"
```

- `403`: certo. É a resposta para quem não tem a senha.
- `200` (ou o navegador mostra "Hello World"): subiu o código padrão, não o do template. Peça para ela apagar o Worker e clicar no botão de novo.
- `523` ou `1010`: espere 1 minuto e rode de novo.

Peça também para ela abrir o endereço no navegador. Deve aparecer `forbidden`.

## Passo 4: ligar na Meta

Guie os passos 6 a 8 do README, um de cada vez, esperando a pessoa confirmar cada um:

- Painel da Meta: app > Casos de uso > Instagram > Personalizar > "API setup with Instagram login" (ou "API setup with Unified login") > passo 3, "Configurar webhooks". Colar o endereço em "URL de callback" e a senha em "Verificar token". Clicar em "Verificar e salvar".
- Assinar o campo `messages`.
- No passo 2 da mesma página, ligar a chave "Assinatura do webhook" da conta do Instagram.

Lembre: a pessoa digita a senha. Você não vê nem pede.

## Passo 5: testar

Peça para alguém mandar uma DM **nova** para a conta. Mensagens antigas, de antes de ligar a assinatura, não podem ser respondidas.

Se você tem o MCP do Instagram, pode listar as conversas para confirmar que a DM chegou. Se não tem, peça para a pessoa responder pelo app do Instagram.

## Problemas

Veja a tabela "Deu errado?" no [README](https://github.com/thaleslaray/instagram-webhook-template/blob/main/README.md#deu-errado).
