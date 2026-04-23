# Transcrição Instantânea — Como usar

Protótipo v1 de PWA (Progressive Web App) de transcrição de voz para texto.

## O que tem aqui

- `index.html` — o app em si (interface + lógica)
- `manifest.json` — metadados do PWA (permite instalar na tela inicial)
- `sw.js` — service worker que faz a interface funcionar offline
- `icon.svg` — ícone do app

## Como testar localmente (no computador)

A Web Speech API exige HTTPS **ou** localhost. Para testar no seu computador:

### Opção 1 — Python (mais rápida)

Abra um terminal na pasta `transcricao/` e rode:

```bash
python3 -m http.server 8000
```

Depois abra no Chrome: http://localhost:8000

### Opção 2 — Node

```bash
npx serve
```

## Como publicar para usar no celular

O celular precisa acessar via HTTPS. Três opções gratuitas:

### GitHub Pages (recomendado, 5 minutos)

1. Crie uma conta em github.com (se ainda não tem)
2. Crie um repositório público novo, por exemplo `transcricao`
3. Faça upload dos 4 arquivos (index.html, manifest.json, sw.js, icon.svg)
4. Vá em Settings → Pages → Source: "Deploy from branch" → main → / (root) → Save
5. Em ~1 minuto o site fica em `https://SEU-USUARIO.github.io/transcricao/`

### Netlify Drop (ainda mais simples)

1. Acesse https://app.netlify.com/drop
2. Arraste a pasta `transcricao/` inteira
3. Pronto — já te dão uma URL HTTPS

### Vercel

1. Instale a CLI: `npm i -g vercel`
2. Na pasta, rode: `vercel`
3. Segue o wizard

## Como instalar no iPhone (como "app")

1. Abra a URL no **Safari** (não funciona pelo Chrome do iPhone — o Chrome iOS usa WebKit mas não expõe todas as APIs de PWA)
2. Toque no botão de compartilhar (o quadradinho com seta pra cima)
3. Role e escolha **"Adicionar à Tela de Início"**
4. Dê um nome e toque em Adicionar

Agora vira um ícone na tela inicial, abre em tela cheia sem barra do Safari.

## Como instalar no Android

1. Abra a URL no **Chrome**
2. Vai aparecer um banner "Adicionar à tela inicial" automaticamente. Se não aparecer, toque no menu (⋮) → "Instalar app" ou "Adicionar à tela inicial"

## Funcionalidades

### Fluxo de conversa bidirecional

O app foi desenhado para conversas entre uma pessoa surda e uma ouvinte:

1. **Pessoa ouvinte fala** → toca no microfone verde → fala aparece como bolha cinza à esquerda ("Ouviu")
2. **Pessoa surda responde** → toca no teclado → digita a resposta → toca em "Falar em voz alta" → app lê o texto com voz sintética para o ouvinte, e mensagem aparece como bolha verde à direita ("Você")

O histórico fica na tela em formato de chat, fácil de acompanhar.

### Recursos

- Transcrição em tempo real (texto provisório em cinza claro, finalizado em branco)
- **Resposta por texto com leitura em voz alta** (text-to-speech) para o ouvinte
- **Frases rápidas** pré-prontas ("Sim", "Não", "Pode repetir?" etc.) para resposta veloz
- **Pausa automática** do microfone quando o app está falando (evita capturar a própria voz sintética)
- 8 idiomas para reconhecimento (padrão pt-BR)
- Seleção de voz para leitura em voz alta (vozes do sistema)
- Ajuste de velocidade da fala (0.6x a 1.6x)
- Ajuste de tamanho da fonte (18-56px)
- **Salvar / compartilhar em .txt** (botão ↗) — abre o menu de compartilhamento do sistema para enviar por WhatsApp, e-mail, salvar em Arquivos/Drive, AirDrop etc. Se o navegador não suportar, faz download direto do .txt.
- Copiar conversa inteira para área de transferência (formatada com "Eu:" / "Eles:")
- Limpar conversa
- Tela mantida acordada durante uso
- Reinício automático do reconhecimento se parar (comum no Safari iOS)

## Limitações conhecidas (v1)

**Esta versão NÃO é offline para o reconhecimento.** O áudio é enviado aos servidores do Google (Chrome/Android) ou Apple (Safari iOS) para transcrição. Para transcrição 100% offline, precisaríamos da v2 com Whisper/Transformers.js.

**Safari iOS** tende a parar o reconhecimento após alguns segundos de silêncio. O código tenta reiniciar automaticamente, mas pode gerar pequenos cortes.

**Chrome Android** costuma ser o mais estável e preciso para pt-BR nesta versão.

**Qualidade** depende muito do microfone, ruído ambiente e clareza da fala.

## Próximos passos sugeridos

- v2: integrar Whisper via Transformers.js (funciona offline de verdade)
- Histórico de sessões (salvar transcrições no IndexedDB)
- Compartilhamento direto (Web Share API)
- Modo claro/escuro
- Destaque visual de palavras-chave
- Tradução automática em tempo real
