---
name: carrossel
description: Ciclo completo de criação, geração, publicação e automação de carrossel Instagram, com MÚLTIPLOS ESTILOS visuais. Use sempre que pedirem "vamos fazer um carrossel", "pesquise tal notícia e cria um carrossel", "monta carrossel sobre X e posta", ou variações. Estilo padrão (top 1) = Anime Ultra Realista com os personagens da marca. Outros estilos sob demanda: Post do X, Jornal, Notícia, Texto Puro HTML. Cobre as 12 etapas e a regra de modelo de imagem (gpt-image-2 padrão, nano-banana só com autorização).
---

# Carrossel Instagram — Ciclo Completo (multi-estilo)

> Skill mestra pra fazer ponta a ponta um carrossel IG sem a dona orquestrar cada passo.
> **Esta é tarefa operacional: a Naia NÃO executa, delega pra Juliana**, que roda o ciclo e devolve pra Naia entregar pra dona.

---

## ESTILOS DE CARROSSEL (LER PRIMEIRO)

O fluxo das 12 etapas é o MESMO pra todos os estilos. Muda só o tratamento visual de cada card.

| # | Estilo | Quando usar | Como gerar |
|---|--------|-------------|------------|
| **TOP 1 (PADRÃO)** | **Anime Ultra Realista — personagens da marca** | Padrão da marca. Conteúdo educativo, autoridade, branding | `gpt-image-2`, com os personagens canônicos da marca (ver `knowledge/personagens/`) |
| 2 | **Post do X (tweet)** | Provocação, opinião, "screenshot de tweet viral", thread | Mockup de post do X/Twitter (avatar, nome, texto, métricas plausíveis). HTML para imagem ou gpt-image-2 |
| 3 | **Jornal** | Notícia "impressa", autoridade editorial, manchete clássica | Layout de jornal (cabeçalho, colunas, serifada, P&B ou sépia). HTML para imagem ou gpt-image-2 |
| 4 | **Notícia (portal)** | Quebra de notícia tech, "saiu na imprensa", credibilidade | Layout de portal de notícia (logo, headline, foto, lide). HTML para imagem ou gpt-image-2 |
| 5 | **Texto Puro (HTML)** | Densidade de informação, lista, framework, didático limpo | HTML/CSS renderizado para imagem (tipografia forte, fundo sólido, sem personagem) |

**REGRA DE GATILHO:**
- Dona diz **"padrão"** ou **"top 1"** → uso o **Anime Ultra Realista** direto, sem perguntar estilo.
- Dona **não especifica** estilo → assumo o padrão (top 1).
- Dona pede **"me manda os estilos que você tem"** → respondo com a tabela acima e espero ela escolher.
- Dona nomeia um estilo (ex: "faz estilo jornal") → uso aquele.

> Estilos 2 a 5 podem ser renderizados via HTML para imagem (screenshot de um template HTML) quando precisarem de texto pixel-perfeito, ou via gpt-image-2 quando quiser estética ilustrada. Texto Puro é SEMPRE HTML.

---

## REGRA DE MODELO DE IMAGEM (CRÍTICA)

**Padrão: `gpt-image-2` (OpenAI).** Sempre tento primeiro.
- Tamanho: `1024x1280` (4:5 portrait) ou `1536x1024` (paisagem/capa). Quality `high`. Output `jpeg`.
- A chave de API fica numa variável de ambiente (preenchida na instalação), nunca em texto na skill.

**Fallback: `nano-banana-pro` (modelo de imagem do Gemini).** NUNCA uso por conta própria.
Se o gpt-image-2 falhar, o protocolo é OBRIGATÓRIO:
1. **Aviso a dona** que o gpt-image-2 falhou.
2. **Explico o porquê** (erro retornado, rate limit, conteúdo recusado, etc.).
3. **Peço autorização** explícita pra usar o nano-banana como fallback.
4. Só uso o nano-banana **depois do OK** dela.

> Nunca trocar de modelo em silêncio. A dona decide o fallback.

---

## QUANDO ATIVAR ESSA SKILL

Triggers:
- "Vamos fazer um carrossel baseado nesse aqui" + link de post
- "Pesquise tal notícia e cria um carrossel"
- "Monta carrossel sobre X e posta"
- "Carrossel novo sobre [tema] e ativa o comment-to-DM"
- Qualquer pedido que envolva criar + postar + automação no mesmo fluxo

---

## PRÉ-REQUISITOS (ler ANTES de começar)

Referências canônicas (consultar):
- **`knowledge/personagens/`** — padrão atual dos personagens da marca. Referência única pra todo prompt do trio.
- Regra de envio: cards enviados 1 por vez, sequencial.
- Regra de copy: sem promessa numérica pro leitor (case real é OK).
- Regra de acentuação: PT-BR perfeito sempre.

---

## ETAPAS DO CICLO (12 etapas)

### ETAPA 1 — Captura da referência
Se for "baseado nesse aqui" + link:
1. Identificar URL: Instagram (`/p/{shortcode}/` baixa imagens + caption + métricas), TikTok/Reels (yt-dlp), notícia/artigo (WebFetch).
2. Baixar pra uma pasta temporária (`card-N.jpg`).
3. Ler cada card com Read (multimodal): estrutura, estilo, densidade, hook + CTA.
4. Capturar caption original + top comments.

Se for "pesquise tal notícia": WebSearch/WebFetch, resumir 3 a 5 pontos-chave, identificar ângulo viral.

**ENTREGÁVEL:** resumo curto no Telegram (até 300 palavras): o que é a referência, hook, o que vamos pegar, **e qual estilo** (padrão/top 1 ou outro).

---

### ETAPA 2 — Roteiro detalhado

Estrutura padrão (estilo Anime/Trio):
| Card | Função |
|---|---|
| 01 | Capa com os personagens da marca + headline + hook |
| 02-N | Prompts / pilares / insights |
| Meio | Card CASE (quebra de ritmo, personagem em destaque) |
| Último | CTA + palavra-chave pra comment-to-DM |

Pra cada card: tag superior, título, texto principal denso, **descrição visual completa**, badge de numeração, assinatura.

Decisões a perguntar ANTES de aprovar:
1. **Estilo** (se não foi dito): padrão/top 1 (anime) ou outro (X/jornal/notícia/HTML)?
2. **Paleta**: a paleta cyber da marca ou editorial?
3. **Personagens em quais cards**: só na capa ou em vários?
4. **Palavra-chave do CTA**: "QUERO", "ACESSO", etc.?
5. **Destino do botão**: qual link?

**ENTREGÁVEL:** roteiro completo no Telegram + perguntas-chave. **AGUARDAR OK EXPLÍCITO** antes de gerar.

---

### ETAPA 3 — Geração das imagens

**Modelo padrão: `gpt-image-2`** (ver REGRA DE MODELO DE IMAGEM acima, nano-banana só com autorização).
Tamanho `1024x1280`, quality `high`, output `jpeg`.

Padrão de geração: dispara os cards em paralelo (threads), lendo a chave de API da variável de ambiente. Roda em background, acompanha os logs, leva ~2 a 5 min em paralelo.
**Se gpt-image-2 falhar → seguir o protocolo de fallback (avisar + explicar + pedir OK pro nano-banana).**
**Status pra dona a cada 2 min** durante a geração.

**Estilos 2 a 5 (X/Jornal/Notícia/HTML):** quando o estilo for mockup com texto pixel-perfeito, renderizar via HTML para imagem (template HTML + screenshot 1080x1350) em vez de gpt-image-2.

---

### ETAPA 4 — Revisão visual contra checklist

Ler cada card com Read (multimodal). Para o estilo Anime/Trio, validar contra o padrão dos personagens da marca em `knowledge/personagens/`:
- [ ] Personagens fiéis ao padrão canônico da marca
- [ ] Acentuação PT-BR **pixel-perfeita**
- [ ] Densidade de texto adequada, paleta consistente, badge de numeração, assinatura
- [ ] Sem watermark, sem emoji, sem promessa numérica pro leitor

Card que falhar → regenerar APENAS aquele com prompt ajustado.

---

### ETAPA 5 — Overlay manual de logos oficiais (quando precisar)
gpt-image-2 não faz logo pixel-perfect. Gerar versão SEM o logo (espaço reservado), baixar o SVG/PNG oficial, compor via Pillow (paste com posição calculada).

---

### ETAPA 6 — Envio sequencial pra aprovação
**1 card por vez**, ordem 01→N, esperar confirmação, sleep 1s. NUNCA paralelo, NUNCA sendMediaGroup. Os cards são enviados pra dona no Telegram, um a um.
No último, perguntar: "Aprova o carrossel inteiro? Sigo pra legenda + publicação?" **Aguardar OK explícito.**

---

### ETAPA 7 — Legenda do post
1ª pessoa, hook forte na 1ª linha, **sem promessa numérica pro leitor** (case real = OK), sem palavras de coach, 4 a 5 bullets, CTA literal com a palavra-chave, 10 a 12 hashtags balanceadas, acentuação perfeita. Mandar pra dona e aguardar OK.

---

### ETAPA 8 — Upload em URL pública
⚠️ NÃO usar Litterbox/Catbox/Imgur (o scraper da Meta bloqueia).
✅ USAR uma hospedagem estática própria (ex: Cloudflare Pages servindo de um repositório de assets do negócio). Subir os cards numa pasta dedicada e validar que respondem 200 antes de publicar.
⚠️ O autor do commit precisa ser um email vinculado à conta de deploy. Esperar o CDN propagar (~30 a 60s). Se a Meta rejeitar o formato: recompress via Pillow (`quality=92, optimize=True`) e adicionar `?v=$(date +%s)` na URL pra furar o cache.

---

### ETAPA 9 — Publicação via Meta Graph API
⚠️ Usar `graph.instagram.com` (NÃO facebook). Token IG do tipo Instagram Login. O token fica guardado criptografado (variável de ambiente + segredo no banco), nunca em texto na skill.
Fluxo (graph.instagram.com): 1) criar N containers `is_carousel_item=true` → 2) esperar cada `status_code=FINISHED` → 3) criar container `media_type=CAROUSEL` com `children` + `caption` → 4) `media_publish` com `creation_id`. Salvar `media_id` e pegar o `permalink`.

---

### ETAPA 10 — Automação comment-to-DM
Configurar a automação de comentário → DM no painel de automações do CRM/sistema do negócio. Definir: `media_id` do post, `keywords`, `reply_public_variants` (5 variações pra não soar bot), `dm_text` (no tom da marca, sem link explícito), `dm_buttons` com **`quick_reply`** (payload único e descritivo), `delay_seconds: 3`.
> Botão `quick_reply` entrega o lead pro fluxo do SDR. `web_url` abre link externo mas NÃO entrega pro fluxo. Pra handover pro SDR use sempre `quick_reply`. (Detalhes dos campos em `agents-automations`/`reference-sdr.md`.)

---

### ETAPA 11 — Validar disparo
Aguardar o 1º comentário real com a palavra-chave (não dá pra simular fielmente, teste com comment fake não fecha o fluxo). Acompanhar os logs/runs da automação. Conferir que a resposta pública e a Private Reply com botões saíram com sucesso.

---

### ETAPA 12 — Handover pro SDR (Davi)
Lead clica no botão → o payload chega como mensagem na conversa IG → webhook do CRM grava a conversa → o SDR (Davi) lê e responde/qualifica/vende. Não precisa configurar nada no SDR (ele já lê IG DMs pelo CRM). Só garantir payload único e descritivo.

---

## GOTCHAS (não repetir)
1. **Token IG ≠ Facebook** — roda em `graph.instagram.com`. Sintoma: "Cannot parse access token".
2. **Litterbox/Catbox** bloqueiam o scraper da Meta. Sintoma: erro 9004. Solução: hospedagem estática própria.
3. **Cache de CDN** serve versão antiga. Solução: `?v=$(date +%s)`.
4. **gpt-image-2 às vezes gera JPEG que a Meta rejeita** (erro 36001). Solução: recompress Pillow `quality=92, optimize=True`.
5. **Logo via prompt nunca é pixel-perfect** — overlay PNG via Pillow.
6. **Private Reply ≠ DM padrão** — o botão vai anexado JUNTO no payload da Private Reply, nunca em 2ª chamada.
7. **Envio sequencial OBRIGATÓRIO** — 1 por vez, sleep 1s, esperar confirmação.
8. **Troca de modelo de imagem NUNCA em silêncio** — gpt-image-2 falhou? Avisa, explica, pede OK pro nano-banana.

---

## CRITÉRIOS DE "ENTREGUE"
- [ ] Estilo definido (padrão/top 1 ou outro escolhido pela dona)
- [ ] Roteiro aprovado
- [ ] Cards gerados e enviados em ordem, todos aprovados
- [ ] Legenda aprovada
- [ ] Post publicado (`media_id` salvo)
- [ ] Automação criada (enabled=true)
- [ ] 1º disparo real com Private Reply + botões confirmado
- [ ] Confirmado pra dona com permalink + id da automação + log

## ESCALAR PRA DONA QUANDO
- Referência não encontrável (link quebrado/post deletado)
- gpt-image-2 falhar (pedir OK pro fallback)
- Meta retornar erro persistente
- Banco/Postgres não responder
- SDR não estiver processando IG DMs
