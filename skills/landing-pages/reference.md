# Manual do Agente · Criacao de Landing Pages Multi-Estilo

> Documento operacional para guiar agentes na criacao de landing pages do dono (Cliente Exemplo), em qualquer um dos 10 estilos catalogados.
> Auto-suficiente. Outro agente lendo apenas este arquivo deve conseguir conduzir uma briefing, propor estilos, montar o protocolo e entregar uma landing pronta no ar.
> Versao 2 generica · 27 de abril de 2026.
> Substitui o documento especifico `metodo-landing-terminal-matrix.md`, que continua preservado como referencia historica do estilo Terminal Matrix.

---

## 1. PROTOCOLO DE INICIO (REGRA DE OURO)

Quando o dono pedir uma landing nova, o agente SEMPRE executa este protocolo ANTES de codar uma unica linha.

### 1.1 Coleta dos dados basicos

Antes de propor qualquer estilo, perguntar e registrar:

1. Tema ou produto da landing (o que esta sendo vendido ou comunicado).
2. CTA principal (link de checkout, formulario, agendamento, etc).
3. Dominio desejado (ex: `meusite.seudominio.com.br`, `nome.seudominio.com.br`).
4. Pixel e tracking (Meta Pixel ID, GA4, GTM, etc).
5. Foto ou material visual disponivel (avatares, fotos de bastidor, logos).
6. Prazo (entregar hoje, esta semana, mes que vem).
7. Avatar de cliente (publico-alvo concreto, com idade, contexto, vocabulario).

Registrar tudo num bloco de contexto antes de seguir.

### 1.2 Apresentacao das 10 opcoes de estilo

Apresentar a lista da secao 3 numa mensagem visualmente clara, cada estilo com:

- Nome do estilo
- Vibe em 1 frase
- Exemplo no ar (URL real quando existir)
- Paleta resumida
- Quando se encaixa

Formato sugerido na hora de mostrar pro dono:

```
01 · TERMINAL MATRIX
Vibe: hacker, dev, autoridade tecnica
Quando: produtos tech, IA, dev
Paleta: preto + verde neon + vermelho alerta
Exemplo: seusite.com.br

02 · EDITORIAL TECH PREMIUM
Vibe: revista premium dark, alta densidade textual com elegancia
Quando: thought leadership, propostas corporate
...
```

### 1.3 Confirmacao da escolha

Pedir pro dono:

- Escolher 1 dos 10 estilos pela numeracao
- Ou pedir uma combinacao explicita (ex: "estilo 01 + componente glass do estilo 03")
- Ou pedir variantes do mesmo estilo (ex: 2 versoes do estilo 02 com cabecalhos diferentes)

Se o dono pedir combinacao ou variante, formalizar por escrito antes de partir pra implementacao.

### 1.4 OK explicito antes de codar

Confirmar a escolha + dados basicos numa mensagem unica e aguardar OK explicito. Sem OK, nao seguir.

### 1.5 Recomendacao opcional: prototipo no Google Stitch

Antes de codar, abrir https://stitch.withgoogle.com/ e gerar variantes visuais da landing no estilo escolhido. Ver detalhes na secao 5.

---

## 2. STACK PADRAO

A stack abaixo vale para os 10 estilos. Apenas paleta, fontes e componentes mudam.

### 2.1 Tecnologia

- **HTML estatico puro single-file.** Um unico `index.html` com todo CSS e JS inline. Sem framework, sem build step, sem bundler. Carrega instantaneo.
- **Hospedagem:** Vercel. Preview por commit, custom domain via CNAME, free tier resolve.
- **DNS:** Cloudflare. CNAME apontando para `cname.vercel-dns.com`. **SEMPRE proxied=false** (Vercel quebra com proxy CF ligado).
- **Repositorio:** GitHub privado. Branch `main`. Commits com `author/committer = usuario@exemplo.com` (regra rigida do Vercel para os dominios `.seudominio.com.br` e `.seudominio.com.br`).
- **Animacoes:** GSAP via CDN quando precisar de timeline complexo, ou puro CSS keyframes quando bastar.
- **Fontes:** Google Fonts via CDN. Cada estilo lista as fontes obrigatorias na secao 3.

### 2.2 O que NAO usar

- Sem Node.js no projeto
- Sem `npm install`
- Sem `package.json`
- Sem TypeScript
- Sem React/Vue/Svelte
- Sem Tailwind (CSS direto, controle total)
- Sem build step

Vercel detecta o repositorio como estatico automaticamente quando nao tem `package.json`. Servir `index.html` da raiz.

### 2.3 Variaveis de ambiente necessarias

Tenha em maos antes de comecar:

```
GITHUB_TOKEN          (PAT classic com escopo repo)
VERCEL_TOKEN          (token de deploy do Vercel)
VERCEL_TEAM_ID        <VERCEL_TEAM_ID>
CLOUDFLARE_API_TOKEN  (com permissao Zone:DNS:Edit)
CLOUDFLARE_ZONE_ID    (zone do dominio raiz, ex: seudominio.com.br)
```

---

## 3. CATALOGO DOS 10 ESTILOS

Cada estilo segue o mesmo template de descricao. O agente apresenta os 10 ao dono e aguarda escolha.

### 3.1 TERMINAL MATRIX

- **Vibe:** hacker, dev IDE, autoridade tecnica.
- **Quando usar:** produtos tech, cursos IA/dev, comunidades de programacao, apostilas tecnicas, propostas B2B premium, slide decks tecnicos.
- **Quando NAO usar:** consumidor leigo, e-commerce de varejo, saude popular, infoprodutos de baixo ticket pra audiencia mainstream.
- **Paleta:** `#0a0a0a` fundo + `#00ff41` verde neon + `#dc2626` vermelho alerta + `#22d3ee` cyan info.
- **Tipografia:** JetBrains Mono · Fira Code · Geist Mono · VT323 (display CRT) · Space Grotesk (titulo opcional).
- **Componentes-ancora:** terminal card com header de aba, prompt card com botao copiar, sidebar navegavel com atalhos, matrix canvas background, scanlines CRT overlay, hero com video em frame terminal.
- **Variantes internas:** terminal puro · terminal + landing comercial · terminal slides · terminal apostila/doc.
- **Exemplo no ar:** https://seusite.com.br · https://aula1.seudominio.com.br · https://apostila.seudominio.com.br
- **Repo de referencia:** `seu-usuario/imersao-cliente-ai` · `seu-usuario/aula1-comunidade-exemplo` · `seu-usuario/apostila-comunidade-exemplo`
- **Backup do metodo especifico:** `metodo-landing-terminal-matrix.md` na raiz desse repositorio (ou `seu-usuario/metodo-landing-terminal-matrix`).

### 3.2 EDITORIAL TECH PREMIUM

- **Vibe:** revista premium dark, alta densidade textual com elegancia.
- **Quando usar:** thought leadership, propostas corporate, conteudo intelectual longo, manifestos de marca tech.
- **Quando NAO usar:** ofertas de impulso (ticket baixo, decisao rapida), consumidor leigo.
- **Paleta:** `#0a0a0a` fundo + `#d97757` ambar Claude (acento) + `#e8e6df` off-white + `#f0ece4` papel.
- **Tipografia:** Instrument Serif italic (titulos) · Geist (corpo) · Geist Mono (codigo e ticker).
- **Componentes-ancora:** dropcap inicial, ticker animado horizontal, numeros editoriais grandes, pull-quote em italico, eyebrow uppercase, secoes com regua superior.
- **Exemplo no ar:** https://imersao-maio-editorial.seudominio.com.br
- **Repo de referencia:** `seu-usuario/imersao-maio-editorial`

### 3.3 GLASS PREMIUM APPLE

- **Vibe:** macOS Big Sur, frosted glass, soft shadows, polish premium Apple-like.
- **Quando usar:** SaaS premium, dashboards, produtos high-end, paginas de produto enterprise.
- **Quando NAO usar:** marca jovem disruptiva, produto antiquado, audiencia conservadora.
- **Paleta:** gradient escuro (`#0f172a` -> `#1e1b4b` -> `#0c4a6e`) + blur 20-40px + brilhos sutis nas bordas + branco a 8-12%.
- **Tipografia:** SF Pro · Inter · Geist (corpo).
- **Componentes-ancora:** glass cards com `backdrop-filter: blur(28px)`, bordas com glow lento (animacao 20s), particulas ambient flutuantes, gradientes radiais suaves.
- **Exemplo no ar:** agents.seudominio.com.br (versao premium glass).
- **Repo de referencia:** `seu-usuario/agents-dashboard` (a confirmar quando criado).

### 3.4 NYOS EDITORIAL

- **Vibe:** editorial tech minimalista, tipografia de catalogo, alto contraste claro/escuro.
- **Quando usar:** portfolio individual, catalogo de servicos, estetica sofisticada, manifesto de agencia.
- **Quando NAO usar:** landings de impulso emocional, infoprodutos com vibe energetica.
- **Paleta:** `#fafafa` fundo claro + `#0a0a0a` tipografia + 1 acento neon a escolher (ex: `#3aff8b`, `#fb5607`, `#22d3ee`).
- **Tipografia:** Inter Display 700/900 · Sohne · GT America (display).
- **Componentes-ancora:** hero tipografico (titulo gigante centrado), grade editorial 12 colunas, numeros grandes destacados, cabecalho com logo monoline.
- **Exemplo no ar:** a popular quando o primeiro estilo NYOS for ao ar.
- **Repo de referencia:** a popular.

### 3.5 BRUTALIST TECH

- **Vibe:** cores chapadas, fontes geometricas pesadas, layouts assimetricos, anti-design intencional.
- **Quando usar:** marca jovem, produto disruptivo, contracorrente, statement de identidade forte.
- **Quando NAO usar:** publico corporativo, B2B tradicional, oferta de premium suave.
- **Paleta:** 2 cores chapadas saturadas (ex: amarelo `#facc15` + preto `#0a0a0a`, ou vermelho `#dc2626` + branco `#ffffff`) + uma neutra de apoio.
- **Tipografia:** Space Grotesk 900 · Archivo Black · IBM Plex Mono (codigo e detalhes).
- **Componentes-ancora:** blocos grandes contrastantes ocupando metade da tela, bordas grossas (4-8px) sem antialias, sombras chapadas (`box-shadow: 8px 8px 0 #000`), elementos em rotacao leve (-2deg ate 2deg).
- **Exemplo no ar:** a popular.
- **Repo de referencia:** a popular.

### 3.6 CYBERPUNK NEON

- **Vibe:** paleta saturada vibrante, glitch effects, gradients neon, futurismo distopico.
- **Quando usar:** gaming, NFT/Web3, eventos tech disruptivos, comunidades cyber.
- **Quando NAO usar:** publico corporativo serio, oferta financeira sobria, B2B tradicional.
- **Paleta:** `#0d0221` deep purple + `#ff006e` magenta + `#00f5ff` cyan + `#fb5607` orange.
- **Tipografia:** Orbitron · Rajdhani · VT323 (display) · IBM Plex Mono (corpo).
- **Componentes-ancora:** glitch animation em titulos, scanline gradient horizontal, holographic shimmer em cards, vaporwave grids floor com perspective.
- **Exemplo no ar:** a popular.
- **Repo de referencia:** a popular.

### 3.7 MINIMALIST CLEAN

- **Vibe:** whitespace generoso, tipografia como protagonista, zero distracao, foco no conteudo.
- **Quando usar:** servico B2B, consultoria, portfolio individual, paginas de marca pessoal sobria.
- **Quando NAO usar:** ofertas que precisam de prova abundante e densidade visual, infoprodutos energeticos.
- **Paleta:** 90% off-white (`#fafafa`) + 5% texto preto (`#0a0a0a`) + 5% acento (1 cor a escolher).
- **Tipografia:** Inter · Sohne · Aeonik (corpo e display).
- **Componentes-ancora:** hero centrado com max-width 65ch, secoes alternadas com bottom-line CTA, ilustracoes de linha fina.
- **Exemplo no ar:** a popular.
- **Repo de referencia:** a popular.

### 3.8 CINEMATIC DARK

- **Vibe:** dramatico, fotos full-bleed, tipografia grande sobre imagens, vibe filme.
- **Quando usar:** produtos premium emocionais, lancamentos, eventos imersivos, marcas com narrativa forte.
- **Quando NAO usar:** servico tecnico documental, oferta de baixo ticket, ausencia de material visual de alta qualidade.
- **Paleta:** preto profundo `#000` + cinzas neutros `#262626 / #404040 / #737373` + 1 acento vibrante (ex: `#facc15`, `#fb5607`, `#22d3ee`).
- **Tipografia:** Tenor Sans · Ogg · Suisse Works (display dramatico).
- **Componentes-ancora:** hero image full-bleed com overlay de gradiente, parallax sutil em scroll, captions de "scene" no canto inferior, transicoes lentas (1-2s) entre secoes.
- **Exemplo no ar:** a popular.
- **Repo de referencia:** a popular.

### 3.9 RETRO SYNTHWAVE 80s

- **Vibe:** gradient sunset 80s, grids perspective, palmeiras digitais, neon glow chromatico.
- **Quando usar:** nostalgia tech, marcas com pegada retro, infoprodutos com vibe energetica, eventos de lancamento.
- **Quando NAO usar:** publico corporativo, B2B serio, ofertas de servico sobrio.
- **Paleta:** gradient rosa -> roxo -> azul (sunset) `#f72585 -> #7209b7 -> #3a0ca3` + sun grid amarelo `#ffd60a` + black contrast.
- **Tipografia:** Outrun (display) · Audiowide · IBM Plex Mono (detalhes).
- **Componentes-ancora:** sunset gradient background, grid floor com perspective transform, neon outlined text, chrome shine em titulos.
- **Exemplo no ar:** a popular.
- **Repo de referencia:** a popular.

### 3.10 MAGAZINE HERO

- **Vibe:** capa de revista digital, hero ocupa 100vh com manchete grande, sub-text editorial.
- **Quando usar:** artigos longos, lancamentos com historia, content marketing premium, paginas de manifesto.
- **Quando NAO usar:** call-to-action de impulso rapido, audiencia que chega ja decidida (oferta direta).
- **Paleta:** 1 foto ou asset grande dominante + tipografia em camadas (preto/branco/1 acento).
- **Tipografia:** Playfair Display (titulos serif) · Crimson Pro (corpo serif) · IBM Plex Sans (eyebrow e meta).
- **Componentes-ancora:** hero magazine 100vh com manchete grande, 2-3 secoes de artigo longo, callouts ilustrados, indice lateral em desktop.
- **Exemplo no ar:** a popular.
- **Repo de referencia:** a popular.

---

## 4. DESIGN SYSTEM UNIVERSAL

Os principios abaixo valem para os 10 estilos, sem excecao. Sao regras do dono e nao podem ser quebradas.

1. **Texto sempre 2-3x maior que o "normal".** Bullets, cards, sub-headings. Telas grandes ou projetor sao a referencia. Em duvida, aumentar.
2. **Layout 50/50 com personagens (foto/avatar):** texto SEMPRE estritamente na metade direita escura. Nunca sobre a foto.
3. **Multiplos elementos (3+ caixas/bullets):** SEMPRE em coluna vertical, NUNCA linha horizontal quando houver personagens a esquerda.
4. **Mobile responsivo obrigatorio:** testar em viewport menor que 768px. Sem excecao.
5. **Sem flash branco em transicoes:** background do `<html>` e do `<body>` ja deve ser a cor final do estilo.
6. **OG image obrigatoria em landing comercial:** sem `og:image`, link no WhatsApp/Telegram fica sem preview e perde 30% de cliques.
7. **Pixel/tracking obrigatorio se for landing comercial:** sem Meta Pixel ou GA4, nao tem como medir conversao.
8. **CTA principal apontando pra checkout real:** nunca placeholder em producao.
9. **Sem travessoes em nenhum lugar do texto:** virgula, ponto, quebra de linha. Regra absoluta do dono.
10. **Portugues brasileiro 100% do tempo:** sem expressoes traduzidas literalmente do ingles.

### 4.1 Container e ritmo vertical padrao

```css
.container {
  max-width: 1200px;        /* 1640px se houver sidebar */
  margin: 0 auto;
  padding: 0 24px;
}

section {
  padding: 80px 0;          /* minimo */
  /* 100-120px em hero/CTA principal */
}

.card {
  padding: 24px;            /* pequenos */
  /* 32-48px em cards grandes/hero */
}

.grid { gap: 24px; }          /* ou 32px em grids generosos */
```

### 4.2 Escala tipografica obrigatoria

Cada estilo escolhe a familia de fontes na secao 3, mas a escala numerica a seguir vale para todos.

| Elemento | Tamanho minimo | Observacao |
|---|---|---|
| H1 hero | 80-120px | NUNCA pequeno. Texto pra ser lido em projetor/TV. |
| H2 sections | 48-72px |  |
| H3 subsections | 32-48px |  |
| Body texto longo | 18-24px |  |
| Bullets / cards | 32-48px | 2-3x maior que default. |
| Footer / detalhes | 16-20px | piso absoluto. |
| Code inline | 12.5-14px | proporcional ao contexto. |

Line-height: 1.55-1.6 em corpo, 1.0-1.2 em titulos grandes. Letter-spacing: titulos em caixa alta ganham 1-2px.

---

## 5. GOOGLE STITCH (PROTOTIPAGEM)

URL: https://stitch.withgoogle.com/

Ferramenta do Google que gera designs e mockups visuais de UIs a partir de prompts em linguagem natural. Permite iterar prototipos rapidamente antes de partir pro codigo.

### 5.1 Quando usar

- Antes de escrever HTML/CSS, pra ter direcao visual clara.
- Quando precisa de variantes pra apresentar pro cliente/dono escolher.
- Pra extrair tokens de design (paleta, espacamento, tipografia) que combinem com o estilo escolhido.
- Pra comparar layouts (ex: 50/50 personagens vs fullscreen 2 colunas) antes de implementar.

### 5.2 Como usar no fluxo

1. Acessar stitch.withgoogle.com.
2. Criar projeto novo.
3. Prompt detalhado, ja com o estilo escolhido. Exemplos:
   - Estilo 01 Terminal Matrix: "Landing page terminal hacker style, dark background `#0a0a0a`, neon green accents `#00ff41`, JetBrains Mono typography, hero with [conteudo], CTA button with glow, scanlines CRT overlay."
   - Estilo 02 Editorial Tech Premium: "Landing dark editorial magazine style, background `#0a0a0a`, accent `#d97757`, serif italic Instrument Serif headings, ticker animation, dropcap, pull-quote section."
   - Estilo 03 Glass Premium Apple: "Landing premium glass UI, gradient background `#0f172a -> #1e1b4b`, frosted glass cards with `backdrop-filter blur(28px)`, soft glow borders, SF Pro typography."
4. Iterar com edicoes incrementais (pode ajustar elementos individuais via natural language).
5. Exportar artifacts (HTML/CSS, design tokens, ou screenshots de referencia).
6. Levar o resultado como referencia visual pra implementacao no HTML monolitico.

### 5.3 Beneficio

Acelera fase de design em 60-80%. Em vez de escrever CSS no escuro, ve a tela primeiro, ajusta visualmente, e depois replica no HTML monolitico do projeto.

### 5.4 Casos reais

- `agents.seudominio.com.br` (dashboard premium glass) foi prototipado no Stitch antes da implementacao final.
- Iteracoes de slides comparativos (V1 Terminal vs V2 Editorial) usam Stitch pra mostrar direcoes diferentes pro dono escolher antes de implementar.
- Cards de pricing (`service.seucrm.com.br`) foram iterados no Stitch antes do CSS final.

### 5.5 Limitacoes

- Stitch produz HTML/CSS com classes do Tailwind ou similar. Voce vai REIMPLEMENTAR esse output no padrao monolitico do projeto (sem Tailwind, sem build), copiando apenas a essencia visual.
- Nao substitui o codigo final, e so etapa de design.

### 5.6 Integracao com MCP

O Stitch pode ser acessado via MCP (Model Context Protocol) se configurado no ambiente do Claude Code, permitindo que o agente gere prototipos via comando. Se MCP do Stitch nao estiver disponivel, usar a interface web normal.

---

## 6. WORKFLOW DE DEPLOY

Os passos abaixo sao estilo-agnosticos. Apenas o conteudo do `index.html` muda conforme o estilo escolhido.

### Passo 0: Prototipar layout no Google Stitch (5-15min)

Quando o estilo for novo ou a estrutura for nao-trivial. Pular quando o componente ja existe no apendice (secao 10) e a estrutura e direta.

### Passo 0b: Variaveis de ambiente

Conforme listado em 2.3.

### Passo 1: Setup local

```bash
mkdir nome-da-landing && cd nome-da-landing
git init -b main
printf "node_modules\n.env\n.DS_Store\n*.log\n" > .gitignore
```

### Passo 2: Criar o `index.html`

Para cada estilo, ja existem fontes, paleta e componentes na secao 3. Montar o HTML monolitico com:

- `<head>` com `<title>`, `<meta name="description">`, `<meta property="og:*">`.
- `<link>` das fontes do estilo escolhido.
- `<style>` inline com as variaveis CSS da paleta do estilo.
- `<body>` com as secoes (hero, beneficios, prova, pricing, CTA, footer).
- `<script>` inline com canvas/animacoes/clipboard quando o estilo pedir.

Salve e abra localmente para testar antes de subir.

### Passo 3: Criar repo GitHub PRIVADO

```bash
curl -X POST -H "Authorization: token $GITHUB_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/user/repos \
  -d '{
    "name":"nome-da-landing",
    "private":true,
    "description":"Landing [estilo escolhido]",
    "auto_init":false
  }'
```

Anote o `id` numerico do repo na resposta JSON. Vai precisar no passo 8.

### Passo 4: Push inicial

```bash
git add index.html vercel.json .gitignore README.md
git -c user.email=usuario@exemplo.com -c user.name="Cliente Exemplo" \
    commit -m "init: landing [estilo]"

git remote add origin https://$GITHUB_TOKEN@github.com/seu-usuario/nome-da-landing.git
git push -u origin main
```

**CRITICO:** o `-c user.email=usuario@exemplo.com` no commit e OBRIGATORIO. Vercel rejeita deploy de commits cujo email nao esta vinculado a conta. Ja queimou meio dia debugando isso antes.

### Passo 5: Criar projeto Vercel

```bash
curl -X POST "https://api.vercel.com/v9/projects?teamId=<VERCEL_TEAM_ID>" \
  -H "Authorization: Bearer $VERCEL_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name":"nome-da-landing",
    "framework":null,
    "gitRepository":{
      "type":"github",
      "repo":"seu-usuario/nome-da-landing"
    }
  }'
```

Anote o `id` do projeto na resposta. Use ele nos passos seguintes.

### Passo 6: Adicionar dominio customizado

```bash
curl -X POST "https://api.vercel.com/v10/projects/PROJECT_ID/domains?teamId=<VERCEL_TEAM_ID>" \
  -H "Authorization: Bearer $VERCEL_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"nome.seudominio.com.br"}'
```

### Passo 7: Criar CNAME no Cloudflare

```bash
curl -X POST "https://api.cloudflare.com/client/v4/zones/$CLOUDFLARE_ZONE_ID/dns_records" \
  -H "Authorization: Bearer $CLOUDFLARE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "type":"CNAME",
    "name":"nome",
    "content":"cname.vercel-dns.com",
    "ttl":1,
    "proxied":false
  }'
```

**CRITICO:** `proxied=false`. Se ligar o proxy laranja do Cloudflare, o Vercel nao consegue emitir o certificado SSL e a landing fica fora.

### Passo 8: Disparar deploy manual

```bash
LAST_SHA=$(git rev-parse HEAD)
REPO_ID=ID_NUMERICO_DO_PASSO_3

curl -X POST "https://api.vercel.com/v13/deployments?teamId=<VERCEL_TEAM_ID>" \
  -H "Authorization: Bearer $VERCEL_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"name\":\"nome-da-landing\",
    \"target\":\"production\",
    \"gitSource\":{
      \"type\":\"github\",
      \"repoId\":$REPO_ID,
      \"ref\":\"main\",
      \"sha\":\"$LAST_SHA\"
    }
  }"
```

**CRITICO:** o `repoId` e numerico (nao use o slug `seu-usuario/nome-da-landing`). Pegue do JSON do passo 3 ou de `GET /repos/seu-usuario/nome-da-landing`.

Sem esse passo o deploy automatico do Vercel nao conecta na primeira vez. Mesmo com gitRepository registrado, precisa disparar 1 deploy manual para o webhook do Vercel passar a escutar push futuros.

### Passo 9: Validar

```bash
dig +short nome.seudominio.com.br CNAME
# deve retornar: cname.vercel-dns.com.

curl -I https://nome.seudominio.com.br/
# esperar: HTTP/2 200, content-type: text/html

echo | openssl s_client -servername nome.seudominio.com.br -connect nome.seudominio.com.br:443 2>/dev/null | openssl x509 -noout -dates
```

Se HTTP retornar 404 ou 308 em loop, a CNAME esta errada ou proxied=true. Volte no Cloudflare e ajuste.

### 6.1 Estrutura minima do repo

```
nome-da-landing/
├── index.html          (single-file, 50-150KB tipico)
├── vercel.json         (config estatico, opcional)
├── .gitignore
├── assets/             (fotos/icons, opcional)
│   ├── og-image.png    (1200x630, OpenGraph)
│   └── favicon.ico
└── README.md           (descricao curta + link)
```

`.gitignore` minimo:

```
node_modules
.env
.env.local
.DS_Store
*.log
```

`vercel.json` opcional (caso queira `cleanUrls` e cache headers):

```json
{
  "cleanUrls": true,
  "trailingSlash": false,
  "headers": [
    {
      "source": "/assets/(.*)",
      "headers": [
        { "key": "Cache-Control", "value": "public, max-age=31536000, immutable" }
      ]
    },
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    }
  ]
}
```

Sem `package.json`. Vercel detecta como estatico automatico.

---

## 7. REGRAS INVIOLAVEIS

Lista de erros conhecidos. Quebrar qualquer uma destas custa horas de debug.

1. **Repos SEMPRE privados.** Regra global do dono. `"private":true` no payload do GitHub.
2. **Commits SEMPRE com `author/committer = usuario@exemplo.com`.** Vercel bloqueia deploy de commits cujo email nao esta vinculado a conta. Use `git -c user.email=usuario@exemplo.com commit ...` ou configure global na primeira vez.
3. **ZERO travessoes em qualquer texto.** Use virgula, ponto ou quebra de linha. Regra absoluta do dono.
4. **ZERO sycophancy.** Sem "Que pergunta excelente!", sem "Como assistente de IA", sem entusiasmo artificial.
5. **Texto SEMPRE 2-3x maior que parece "normal".** Bullets, cards, sub-headings.
6. **Quando layout 50/50 com personagens:** texto SEMPRE estritamente na metade direita escura. Nunca sobre a foto.
7. **Elementos multiplos (3+ caixas/bullets):** SEMPRE em coluna vertical, NUNCA linha horizontal quando ha personagens a esquerda.
8. **Vercel teamId correto:** `<VERCEL_TEAM_ID>`. NAO use o `VERCEL_ORG_ID` do `.env` solto, esse e o `creatorId` (id pessoal), nao o team.
9. **Cloudflare CNAME SEMPRE proxied=false.** Vercel nao funciona com proxy CF ligado.
10. **Auto-deploy do Vercel nao conecta na primeira vez.** Sempre dispare 1 deploy manual via API apos o `git push` inicial.
11. **Nunca usar `package.json` em projeto puramente estatico.** Vercel pode interpretar errado e tentar build Next.js.
12. **OG image obrigatoria em landing comercial.** Sem `og:image`, link no WhatsApp/Telegram fica sem preview e perde 30% de cliques.
13. **Pixel/tracking obrigatorio se for landing comercial.** Sem Meta Pixel ou GA, nao tem como medir conversao.
14. **CTA principal apontando pra checkout real.** Nunca placeholder em producao.
15. **Recriar fiel ao original.** Se esta integrando uma landing existente em outra, recrie EXATAMENTE igual. Nunca simplificar campos, nunca cortar secoes sem ordem explicita.
16. **Apresentar os 10 estilos antes de codar.** Nao escolher por conta propria. O dono escolhe.
17. **Aguardar OK explicito.** Sem "pode fazer", sem "ok", sem "vai", nao sair codando.

---

## 8. CHECKLIST DE QUALIDADE (antes de entregar)

Marque cada item antes de mandar para o dono.

- [ ] Repo e privado (`"private": true`).
- [ ] Commits estao com `author = usuario@exemplo.com`.
- [ ] HTTP 200 no dominio final em `curl -I https://nome.seudominio.com.br/`.
- [ ] HTTPS valido (cert SSL emitido pelo Vercel, nao expirado).
- [ ] Landing carrega em menos de 2 segundos (Lighthouse > 85 em Performance).
- [ ] Mobile responsivo (testado em viewport menor que 768px).
- [ ] Atalhos de teclado funcionando (em apostila/doc/slides quando aplicavel).
- [ ] Pixel/tracking configurado (se landing comercial).
- [ ] Meta Pixel disparando `PageView` no carregamento.
- [ ] CTA principal apontando para checkout real (nao `#`).
- [ ] OG image carregando (testar com WhatsApp/Telegram preview).
- [ ] Sem texto sobre personagens (regra slides).
- [ ] Sem flash branco em transicoes.
- [ ] Sem `console.error` no DevTools.
- [ ] Video embed funciona quando aplicavel (Tella/Loom/YouTube).
- [ ] Botoes "COPIAR PROMPT" testados em Chrome e Safari (quando aplicavel).
- [ ] Sidebar navegacao testada com mouse e teclado (quando aplicavel).
- [ ] Texto 2-3x maior que default em cards/bullets.
- [ ] Sem travessoes em nenhum lugar do texto.
- [ ] Variante Stitch revisada (se aplicavel) antes do codigo final.
- [ ] Estilo final corresponde a 1 dos 10 do catalogo (ou combinacao explicitamente aprovada).

Se algum item falhar, corrigir antes de declarar pronto.

---

## 9. CASOS REAIS DOCUMENTADOS

Tabela de exemplos no ar, com estilo, URL ao vivo, repo e proposito. Servem de referencia visual e tecnica.

| Estilo | Nome | URL ao vivo | Repo | Proposito |
|---|---|---|---|---|
| 01 Terminal Matrix | Imersao original | seusite.com.br | `seu-usuario/imersao-cliente-ai` | Landing original que inaugurou o estilo |
| 01 Terminal Matrix | Service Exemplo | service.seucrm.com.br | `seu-usuario/agencia-exemplo-digital` | Redesign Matrix da agencia (full landing) |
| 01 Terminal Matrix | Propostas | propostasdevendas.seudominio.com.br | `seu-usuario/propostas-de-vendas` | Hub de propostas comerciais |
| 01 Terminal Matrix (slides) | Imersao Maio Terminal | imersao-maio-terminal.seudominio.com.br | `seu-usuario/imersao-maio-terminal` | Variante slide deck terminal |
| 01 Terminal Matrix (apostila) | Apostila Aula 1 | apostila.seudominio.com.br | `seu-usuario/apostila-comunidade-exemplo` | Apostila navegavel (sidebar) |
| 01 Terminal Matrix | Comunidade v3 | comunidade.seudominio.com.br | `seu-usuario/comunidade-exemplo-v3` | Landing comunidade |
| 01 Terminal Matrix (book) | Book Imersao | book.seudominio.com.br | `seu-usuario/book-imersao-claude-openclaw` | Book tecnico navegavel |
| 02 Editorial Tech Premium | Imersao Maio Editorial | imersao-maio-editorial.seudominio.com.br | `seu-usuario/imersao-maio-editorial` | Variante editorial premium |
| 03 Glass Premium Apple | Agents Dashboard | agents.seudominio.com.br | (a confirmar) | Dashboard premium glass |

Use estes como referencia visual, mas NAO copie texto. Cada landing tem oferta diferente.

Estilos 04 a 10 aguardam primeira producao. Ao criar a primeira landing de cada estilo, atualizar essa tabela com a URL e o repo.

---

## 10. APENDICE · BIBLIOTECA DE COMPONENTES REUSAVEIS

Os 7 componentes abaixo estao prontos pra colar no `index.html`. Cada um indica em quais dos 10 estilos faz sentido (matriz de compatibilidade no final).

### 10.1 Terminal Card

Card monospace com header de aba simulando janela terminal macOS. Compatibilidade: 01, 06 (com cores cyber).

```html
<style>
.terminal-card {
  background: #0a0a0a;
  border: 1px dashed rgba(0, 255, 65, 0.4);
  border-radius: 8px;
  overflow: hidden;
  transition: border-color 0.3s, box-shadow 0.3s;
  font-family: 'JetBrains Mono', monospace;
}
.terminal-card:hover {
  border-color: rgba(0, 255, 65, 0.7);
  box-shadow: 0 0 30px rgba(0, 255, 65, 0.4);
}
.terminal-header {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 10px 16px;
  background: rgba(0, 255, 65, 0.03);
  border-bottom: 1px dashed rgba(0, 255, 65, 0.4);
}
.terminal-dot { width: 12px; height: 12px; border-radius: 50%; display: inline-block; }
.terminal-dot.red { background: #ff5f57; }
.terminal-dot.yellow { background: #febc2e; }
.terminal-dot.green { background: #28c840; }
.terminal-header-title {
  font-size: 12px;
  color: #666;
  margin-left: 12px;
  letter-spacing: 0.5px;
}
.terminal-body {
  padding: 24px;
  color: #e8e6df;
  font-size: 14px;
  line-height: 1.6;
}
.terminal-body .prompt::before { content: "$ "; color: #00ff41; }
.terminal-body .cursor::after {
  content: "▮";
  animation: blink 1s steps(2) infinite;
  color: #00ff41;
}
@keyframes blink { 50% { opacity: 0; } }
</style>

<div class="terminal-card">
  <div class="terminal-header">
    <span class="terminal-dot red"></span>
    <span class="terminal-dot yellow"></span>
    <span class="terminal-dot green"></span>
    <span class="terminal-header-title">video.player.tella ./assista.mp4</span>
  </div>
  <div class="terminal-body">
    <p class="prompt">conteudo do card aqui</p>
    <p>linha simples sem prompt</p>
    <p class="cursor">aguardando input</p>
  </div>
</div>
```

Variantes: troque a cor da borda para cyan (`rgba(34, 211, 238, 0.4)`) ou vermelho (`rgba(220, 38, 38, 0.4)`) para hierarquizar.

### 10.2 Prompt Card com botao Copiar

Bloco de codigo com botao "COPIAR PROMPT" no canto superior direito. Funciona com clipboard API moderna e fallback. Compatibilidade: 01, 02 (com paleta editorial), 06.

```html
<style>
.prompt-card {
  position: relative;
  background: #000;
  border: 1px solid rgba(0, 255, 65, 0.4);
  border-left: 3px solid #00ff41;
  border-radius: 6px;
  padding: 20px 24px 24px;
  margin: 20px 0;
  font-family: 'JetBrains Mono', monospace;
}
.prompt-card pre {
  margin: 0;
  padding-right: 140px;
  color: #00ff41;
  font-size: 14px;
  line-height: 1.6;
  white-space: pre-wrap;
  word-break: break-word;
}
.prompt-copy-btn {
  position: absolute;
  top: 12px;
  right: 12px;
  background: #00ff41;
  color: #000;
  border: none;
  padding: 8px 14px;
  font-family: inherit;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 1px;
  cursor: pointer;
  border-radius: 3px;
  transition: all 0.2s;
}
.prompt-copy-btn:hover {
  background: #1aff67;
  box-shadow: 0 0 12px rgba(0, 255, 65, 0.6);
}
.prompt-copy-btn.copied {
  background: #22d3ee;
  color: #000;
}
</style>

<div class="prompt-card">
  <button class="prompt-copy-btn" onclick="copyPrompt(this)">COPIAR PROMPT</button>
  <pre>Texto do prompt aqui. Pode ter quebras de linha.
Pode ter mais de uma linha.
E variaveis tipo {nome_do_cliente}.</pre>
</div>

<script>
function copyPrompt(btn) {
  const text = btn.parentElement.querySelector('pre').textContent;
  if (navigator.clipboard && navigator.clipboard.writeText) {
    navigator.clipboard.writeText(text).then(() => mark(btn));
  } else {
    const ta = document.createElement('textarea');
    ta.value = text;
    document.body.appendChild(ta);
    ta.select();
    try { document.execCommand('copy'); mark(btn); } catch (e) {}
    document.body.removeChild(ta);
  }
}
function mark(btn) {
  const old = btn.textContent;
  btn.textContent = 'COPIADO';
  btn.classList.add('copied');
  setTimeout(() => { btn.textContent = old; btn.classList.remove('copied'); }, 1800);
}
</script>
```

### 10.3 Sidebar Navegavel (apostila, book, doc)

Sidebar fixa esquerda com sections numeradas, hover glow, active highlight, atalhos de teclado e busca. Compatibilidade: 01 (variante apostila), 02 (com tipografia editorial).

```html
<style>
.app {
  display: grid;
  grid-template-columns: 340px 1fr;
  min-height: 100vh;
  max-width: 1640px;
  margin: 0 auto;
}
@media (max-width: 1000px) {
  .app { grid-template-columns: 1fr; }
}
.sidebar {
  position: sticky;
  top: 0;
  align-self: start;
  height: 100vh;
  overflow-y: auto;
  border-right: 1px solid rgba(0, 255, 136, 0.25);
  background: linear-gradient(180deg, rgba(0,10,5,0.92), rgba(0,5,2,0.92));
  backdrop-filter: blur(6px);
  padding: 20px 14px 40px;
}
@media (max-width: 1000px) {
  .sidebar {
    position: fixed;
    left: -360px;
    width: 340px;
    z-index: 200;
    transition: left 0.25s ease;
  }
  .sidebar.show { left: 0; }
}
.sidebar .brand {
  padding: 14px 12px;
  border: 1px solid rgba(0, 255, 136, 0.55);
  border-radius: 6px;
  background: rgba(0, 255, 136, 0.04);
  margin-bottom: 18px;
}
.sidebar .brand .logo {
  font-family: 'VT323', monospace;
  font-size: 30px;
  color: #00ff88;
  text-shadow: 0 0 10px rgba(0, 255, 136, 0.45);
}
.sidebar .brand .subtitle {
  font-size: 11px;
  color: #6faf8a;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-top: 4px;
}
.sidebar .search input {
  width: 100%;
  padding: 10px 12px;
  background: #020;
  color: #00ff88;
  border: 1px solid rgba(0, 255, 136, 0.25);
  border-radius: 4px;
  font-family: inherit;
  font-size: 13px;
  outline: none;
  margin-bottom: 16px;
}
.sidebar .search input:focus {
  border-color: #00ff88;
  box-shadow: 0 0 6px rgba(0, 255, 136, 0.35);
}
.nav { list-style: none; padding: 0; margin: 0; }
.nav a {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 12px;
  margin: 1px 0;
  border-radius: 4px;
  color: #6faf8a;
  text-decoration: none;
  font-size: 13px;
  border-left: 2px solid transparent;
  transition: all 0.15s;
}
.nav a:hover {
  color: #00ff88;
  background: rgba(0, 255, 136, 0.06);
  border-left-color: #008844;
}
.nav a.active {
  color: #00ff88;
  background: rgba(0, 255, 136, 0.1);
  border-left-color: #00ff88;
  text-shadow: 0 0 6px rgba(0, 255, 136, 0.45);
}
.nav a .num {
  font-family: 'VT323', monospace;
  font-size: 16px;
  color: #008844;
  min-width: 26px;
}
.nav a.active .num { color: #00ff88; }

.main { padding: 28px 40px 80px; min-width: 0; }
.section { display: none; }
.section.active { display: block; animation: fadeIn 0.4s ease; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: none; } }
</style>

<div class="app">
  <aside class="sidebar" id="sidebar">
    <div class="brand">
      <div class="logo">APOSTILA.v1</div>
      <div class="subtitle">aula 1 fundamentos</div>
    </div>
    <div class="search">
      <input id="search" type="text" placeholder="buscar /" autocomplete="off">
    </div>
    <ul class="nav" id="nav">
      <li><a href="#cap-1" data-id="cap-1"><span class="num">01</span><span class="label">Introducao</span></a></li>
      <li><a href="#cap-2" data-id="cap-2"><span class="num">02</span><span class="label">Fundamentos</span></a></li>
      <li><a href="#cap-3" data-id="cap-3"><span class="num">03</span><span class="label">Pratica</span></a></li>
    </ul>
  </aside>

  <main class="main">
    <section class="section active" id="cap-1">
      <h1>Capitulo 1</h1>
      <p>Conteudo do capitulo 1.</p>
    </section>
    <section class="section" id="cap-2">
      <h1>Capitulo 2</h1>
      <p>Conteudo do capitulo 2.</p>
    </section>
    <section class="section" id="cap-3">
      <h1>Capitulo 3</h1>
      <p>Conteudo do capitulo 3.</p>
    </section>
  </main>
</div>

<script>
(function() {
  const links = document.querySelectorAll('.nav a');
  const sections = document.querySelectorAll('.section');
  const search = document.getElementById('search');

  function activate(id) {
    sections.forEach(s => s.classList.toggle('active', s.id === id));
    links.forEach(a => a.classList.toggle('active', a.dataset.id === id));
    history.replaceState(null, '', '#' + id);
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  links.forEach(a => a.addEventListener('click', (e) => {
    e.preventDefault();
    activate(a.dataset.id);
  }));

  const hash = location.hash.replace('#', '');
  if (hash && document.getElementById(hash)) activate(hash);

  document.addEventListener('keydown', (e) => {
    if (e.target.tagName === 'INPUT') return;
    const ids = Array.from(links).map(a => a.dataset.id);
    const current = ids.findIndex(id => document.getElementById(id).classList.contains('active'));
    if (e.key === 'j' || e.key === 'ArrowDown') {
      e.preventDefault();
      activate(ids[Math.min(current + 1, ids.length - 1)]);
    } else if (e.key === 'k' || e.key === 'ArrowUp') {
      e.preventDefault();
      activate(ids[Math.max(current - 1, 0)]);
    } else if (e.key === 'g') {
      activate(ids[0]);
    } else if (e.key === 'G') {
      activate(ids[ids.length - 1]);
    } else if (e.key === '/') {
      e.preventDefault();
      search.focus();
    } else if (e.key === 'Escape') {
      search.blur();
      search.value = '';
      filter('');
    }
  });

  function filter(q) {
    q = q.trim().toLowerCase();
    links.forEach(a => {
      const txt = a.textContent.toLowerCase();
      a.parentElement.style.display = (!q || txt.includes(q)) ? '' : 'none';
    });
  }
  search.addEventListener('input', (e) => filter(e.target.value));
})();
</script>
```

Atalhos: `J/K` ou setas para proxima/anterior, `G/Shift+G` para topo/fim, `/` para focar busca, `ESC` para limpar busca.

### 10.4 Matrix Canvas Background

Canvas com chuva de caracteres em loop sutil no fundo. Compatibilidade: 01, 06 (com paleta cyber).

```html
<canvas id="matrix-rain"></canvas>

<style>
#matrix-rain {
  position: fixed;
  inset: 0;
  z-index: 0;
  opacity: 0.14;
  pointer-events: none;
}
</style>

<script>
(function() {
  const canvas = document.getElementById('matrix-rain');
  const ctx = canvas.getContext('2d');
  let w, h, columns, drops;
  const chars = 'アァカサタナハマヤャラワガザダバパイィキシチニヒミリヰギジヂビピウゥクスツヌフムユュルグズブヅプエェケセテネヘメレヱゲゼデベペオォコソトノホモヨョロヲゴゾドボポヴッン0123456789ABCDEF<>{}[]/=+-*';

  function resize() {
    w = canvas.width = window.innerWidth;
    h = canvas.height = window.innerHeight;
    const fontSize = 16;
    columns = Math.floor(w / fontSize);
    drops = new Array(columns).fill(1);
    ctx.font = fontSize + 'px JetBrains Mono, monospace';
  }

  function draw() {
    ctx.fillStyle = 'rgba(0, 0, 0, 0.06)';
    ctx.fillRect(0, 0, w, h);
    ctx.fillStyle = '#00ff41';
    for (let i = 0; i < drops.length; i++) {
      const text = chars[Math.floor(Math.random() * chars.length)];
      ctx.fillText(text, i * 16, drops[i] * 16);
      if (drops[i] * 16 > h && Math.random() > 0.975) drops[i] = 0;
      drops[i]++;
    }
  }

  resize();
  window.addEventListener('resize', resize);
  setInterval(draw, 55);
})();
</script>
```

Performance: 55ms de interval = 18 FPS, suficiente para o efeito sem queimar bateria. Opacity 0.14 mantem leitura do conteudo na frente.

### 10.5 Scanlines CRT Overlay

Gradient repetitivo dando vibe monitor antigo CRT. Compatibilidade: 01, 06, 09.

```css
body::before {
  content: "";
  position: fixed;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  background: repeating-linear-gradient(
    0deg,
    rgba(0, 255, 136, 0.03) 0,
    rgba(0, 255, 136, 0.03) 1px,
    transparent 1px,
    transparent 3px
  );
  mix-blend-mode: overlay;
  opacity: 0.6;
}

body::after {
  content: "";
  position: fixed;
  inset: 0;
  z-index: 2;
  pointer-events: none;
  background: radial-gradient(
    ellipse at center,
    transparent 55%,
    rgba(0, 0, 0, 0.6) 100%
  );
}
```

O `::before` cria as scanlines horizontais. O `::after` cria a vinheta arredondada simulando a curvatura do CRT. Use os dois juntos.

### 10.6 Hero com Video Embed

Layout 2 colunas (texto + iframe Tella/Loom/YouTube em frame terminal). Compatibilidade: 01, 02 (com paleta editorial), 03 (com glass), 08 (com vibe cinematic).

```html
<style>
.hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 48px;
  align-items: center;
  padding: 100px 24px;
  max-width: 1200px;
  margin: 0 auto;
}
@media (max-width: 900px) {
  .hero { grid-template-columns: 1fr; gap: 32px; }
}
.hero-text h1 {
  font-family: 'JetBrains Mono', monospace;
  font-size: clamp(48px, 6vw, 96px);
  font-weight: 800;
  color: #00ff41;
  line-height: 1.05;
  text-shadow: 0 0 18px rgba(0, 255, 65, 0.5);
  margin: 0 0 24px;
}
.hero-text p {
  font-size: 22px;
  color: #e8e6df;
  line-height: 1.5;
  margin: 0 0 32px;
}
.hero-video {
  border: 1px dashed rgba(0, 255, 65, 0.4);
  border-radius: 8px;
  overflow: hidden;
  background: #0a0a0a;
}
.hero-video .terminal-header {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 10px 16px;
  background: rgba(0, 255, 65, 0.04);
  border-bottom: 1px dashed rgba(0, 255, 65, 0.4);
  font-family: 'JetBrains Mono', monospace;
  font-size: 12px;
  color: #666;
}
.hero-video .terminal-dot { width: 11px; height: 11px; border-radius: 50%; }
.hero-video .terminal-dot.red { background: #ff5f57; }
.hero-video .terminal-dot.yellow { background: #febc2e; }
.hero-video .terminal-dot.green { background: #28c840; }
.hero-video .frame { aspect-ratio: 16 / 9; background: #000; }
.hero-video iframe { width: 100%; height: 100%; border: 0; display: block; }
</style>

<section class="hero">
  <div class="hero-text">
    <h1>Comunidade<br>Exemplo</h1>
    <p>Aprenda a construir e operar agentes de IA que vendem, escalam e nao dormem. Rota direta de zero a producao.</p>
    <a href="#cta" class="btn">ASSINAR ACESSO</a>
  </div>
  <div class="hero-video">
    <div class="terminal-header">
      <span class="terminal-dot red"></span>
      <span class="terminal-dot yellow"></span>
      <span class="terminal-dot green"></span>
      <span style="margin-left:12px">video.player.tella ./pitch.mp4</span>
    </div>
    <div class="frame">
      <iframe src="https://www.tella.tv/video/SEU_VIDEO_ID/embed" allow="autoplay" allowfullscreen></iframe>
    </div>
  </div>
</section>
```

Quando o estilo escolhido for 02 ou 03 ou 08, troque a borda dashed verde por borda solida do estilo (ex: 1px solid rgba(217, 119, 87, 0.4) para o ambar Claude do estilo 02).

### 10.7 Pricing Cards comparativos

2-3 cards lado a lado com plano/preco/features, glow nos destaques, badge "MAIS VENDIDO" opcional. Compatibilidade: todos os 10 (cores e fontes adaptadas ao estilo).

```html
<style>
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  max-width: 1100px;
  margin: 60px auto;
  padding: 0 24px;
}
.pricing-card {
  position: relative;
  background: #0a0a0a;
  border: 1px dashed rgba(0, 255, 65, 0.4);
  border-radius: 8px;
  padding: 36px 28px;
  font-family: 'JetBrains Mono', monospace;
  transition: all 0.3s;
}
.pricing-card:hover {
  border-color: rgba(0, 255, 65, 0.7);
  box-shadow: 0 0 30px rgba(0, 255, 65, 0.3);
  transform: translateY(-4px);
}
.pricing-card.featured {
  border-color: #00ff41;
  box-shadow: 0 0 24px rgba(0, 255, 65, 0.5);
}
.pricing-badge {
  position: absolute;
  top: -12px;
  left: 50%;
  transform: translateX(-50%);
  background: #00ff41;
  color: #000;
  padding: 4px 12px;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 2px;
  border-radius: 3px;
}
.pricing-name {
  font-size: 14px;
  color: #6faf8a;
  text-transform: uppercase;
  letter-spacing: 3px;
  margin-bottom: 12px;
}
.pricing-price {
  font-size: 56px;
  color: #00ff41;
  font-weight: 800;
  line-height: 1;
  margin-bottom: 6px;
  text-shadow: 0 0 12px rgba(0, 255, 65, 0.5);
}
.pricing-price small {
  font-size: 18px;
  color: #6faf8a;
  font-weight: 400;
}
.pricing-period { font-size: 13px; color: #666; margin-bottom: 28px; }
.pricing-features {
  list-style: none;
  padding: 0;
  margin: 0 0 32px;
}
.pricing-features li {
  padding: 10px 0 10px 22px;
  position: relative;
  color: #e8e6df;
  font-size: 14px;
  border-bottom: 1px dashed rgba(0, 255, 65, 0.15);
}
.pricing-features li::before {
  content: "✓";
  position: absolute;
  left: 0;
  color: #00ff41;
  font-weight: 700;
}
.pricing-cta {
  display: block;
  text-align: center;
  padding: 14px;
  background: transparent;
  border: 1px solid #00ff41;
  color: #00ff41;
  font-family: inherit;
  font-weight: 700;
  font-size: 13px;
  letter-spacing: 2px;
  text-decoration: none;
  text-transform: uppercase;
  border-radius: 4px;
  transition: all 0.2s;
}
.pricing-cta:hover {
  background: #00ff41;
  color: #000;
  box-shadow: 0 0 18px rgba(0, 255, 65, 0.6);
}
.pricing-card.featured .pricing-cta { background: #00ff41; color: #000; }
.pricing-card.featured .pricing-cta:hover { background: #1aff67; }
</style>

<div class="pricing-grid">
  <article class="pricing-card">
    <div class="pricing-name">// basic</div>
    <div class="pricing-price"><small>R$</small>97<small>/mes</small></div>
    <div class="pricing-period">cobranca mensal recorrente</div>
    <ul class="pricing-features">
      <li>Acesso a todas as aulas</li>
      <li>Comunidade no Discord</li>
      <li>Apostilas em PDF</li>
    </ul>
    <a href="#" class="pricing-cta">ASSINAR</a>
  </article>

  <article class="pricing-card featured">
    <span class="pricing-badge">MAIS VENDIDO</span>
    <div class="pricing-name">// pro</div>
    <div class="pricing-price"><small>R$</small>297<small>/mes</small></div>
    <div class="pricing-period">cobranca mensal recorrente</div>
    <ul class="pricing-features">
      <li>Tudo do plano Basic</li>
      <li>Mentorias semanais ao vivo</li>
      <li>Templates dos agentes</li>
      <li>Suporte prioritario</li>
    </ul>
    <a href="#" class="pricing-cta">ASSINAR PRO</a>
  </article>

  <article class="pricing-card">
    <div class="pricing-name">// enterprise</div>
    <div class="pricing-price">custom</div>
    <div class="pricing-period">implementacao sob medida</div>
    <ul class="pricing-features">
      <li>Tudo do plano Pro</li>
      <li>Setup dos agentes na sua infra</li>
      <li>SLA dedicado</li>
    </ul>
    <a href="#" class="pricing-cta">FALAR COM TIME</a>
  </article>
</div>
```

### 10.8 Animacoes universais

Estes blocos servem para qualquer estilo (mudando apenas as cores/glows do estilo escolhido).

#### 10.8.1 Cursor Blink (estilos 01, 06)

```css
.cursor::after {
  content: '▮';
  display: inline-block;
  margin-left: 4px;
  color: #00ff41;
  animation: blink 1s steps(2) infinite;
}
@keyframes blink { 50% { opacity: 0; } }
```

#### 10.8.2 Glow Pulse (estilos 01, 03, 06, 09)

```css
.glow-pulse {
  animation: glowPulse 2.4s ease-in-out infinite;
}
@keyframes glowPulse {
  0%, 100% {
    box-shadow: 0 0 8px rgba(0, 255, 65, 0.35), 0 0 18px rgba(0, 255, 65, 0.15);
  }
  50% {
    box-shadow: 0 0 16px rgba(0, 255, 65, 0.7), 0 0 36px rgba(0, 255, 65, 0.35);
  }
}
```

#### 10.8.3 GSAP Stagger Entrance (todos)

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script>
gsap.registerPlugin(ScrollTrigger);

gsap.from('.terminal-card', {
  opacity: 0,
  y: 32,
  duration: 0.8,
  stagger: 0.12,
  ease: 'power3.out',
  scrollTrigger: {
    trigger: '.pricing-grid',
    start: 'top 80%',
    once: true
  }
});

gsap.from('.hero-text h1', {
  opacity: 0,
  y: 24,
  duration: 1,
  ease: 'power4.out',
  delay: 0.2
});
</script>
```

#### 10.8.4 Typing Effect (estilos 01, 06)

```html
<h1 id="typewriter"></h1>

<script>
(function() {
  const el = document.getElementById('typewriter');
  const text = 'sudo iniciar agente';
  let i = 0;
  function type() {
    if (i <= text.length) {
      el.textContent = text.slice(0, i);
      i++;
      setTimeout(type, 70);
    } else {
      el.classList.add('cursor');
    }
  }
  type();
})();
</script>
```

#### 10.8.5 Glitch ocasional (estilos 01, 06)

```css
.glitch {
  position: relative;
  animation: glitch 4s infinite;
}
@keyframes glitch {
  0%, 90%, 100% { transform: translate(0); }
  92% { transform: translate(-2px, 0); text-shadow: 2px 0 #ff3355, -2px 0 #00e5ff; }
  94% { transform: translate(2px, 0); text-shadow: -2px 0 #ff3355, 2px 0 #00e5ff; }
  96% { transform: translate(0); }
}
```

### 10.9 Matriz de compatibilidade dos componentes

| Componente | 01 Matrix | 02 Editorial | 03 Glass | 04 NYOS | 05 Brutalist | 06 Cyber | 07 Minimal | 08 Cinematic | 09 Synthwave | 10 Magazine |
|---|---|---|---|---|---|---|---|---|---|---|
| 10.1 Terminal Card | x |  |  |  |  | x |  |  |  |  |
| 10.2 Prompt Card | x | x |  |  |  | x |  |  |  |  |
| 10.3 Sidebar Navegavel | x | x |  | x |  |  |  |  |  | x |
| 10.4 Matrix Canvas | x |  |  |  |  | x |  |  |  |  |
| 10.5 Scanlines CRT | x |  |  |  |  | x |  |  | x |  |
| 10.6 Hero Video | x | x | x | x | x | x | x | x | x | x |
| 10.7 Pricing Cards | x | x | x | x | x | x | x | x | x | x |
| 10.8.1 Cursor Blink | x |  |  |  |  | x |  |  |  |  |
| 10.8.2 Glow Pulse | x |  | x |  |  | x |  |  | x |  |
| 10.8.3 GSAP Stagger | x | x | x | x | x | x | x | x | x | x |
| 10.8.4 Typing Effect | x |  |  |  |  | x |  |  |  |  |
| 10.8.5 Glitch | x |  |  |  |  | x |  |  |  |  |

Quando o componente nao esta na matriz para o estilo escolhido, ou ele nao se aplica ou precisa de adaptacao significativa de cores/fontes.

---

## 11. NOTAS FINAIS PARA O AGENTE QUE FOR USAR ESTE DOCUMENTO

Voce e um agente especialista em landing pages multi-estilo. Suas regras de operacao:

1. **Antes de codar, planeje em texto.** Liste as secoes que vai ter, o esqueleto, e os componentes da secao 10 que vai usar. Submeta para revisao antes de comecar HTML.
2. **Apresente os 10 estilos.** Use o protocolo da secao 1. Nao escolha por conta propria. O dono escolhe.
3. **Aguarde OK explicito.** Sem aprovacao, nao codar.
4. **Use single-file `index.html`.** Nao fragmente CSS/JS em arquivos separados a menos que o arquivo passe de 200KB.
5. **Reuse os componentes da secao 10 sem reinventar.** A biblioteca ja esta pronta. Sua funcao e montar, nao redesenhar.
6. **Teste local primeiro.** Abra o HTML no navegador antes de fazer push. Se quebrou local, vai quebrar em producao tambem.
7. **Siga o workflow da secao 6 na ordem.** Pular passos = horas de debug.
8. **Reporte para a Orquestradora (orquestradora) ao final** com URL no ar, URL do repo, tempo total, qualquer pendencia. Nao reporte direto para o dono.
9. **Use portugues brasileiro 100% do tempo.** Sem expressoes traduzidas literalmente do ingles.
10. **Sem travessoes. Em hipotese alguma.** Virgula, ponto, quebra. Sempre.
11. **Atualize a tabela da secao 9 quando criar uma landing nova de algum dos estilos 04 a 10.** Adicione URL ao vivo, repo e proposito.

Pronto. Boa entrega.
