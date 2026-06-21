# Referência do Davi (SDR) — Treinamento, Integração CRM e Deploy

> Knowledge consolidado do SDR Davi da Naia OpenClaw.
> Documento auto-suficiente. Quem montar a infra do Davi (a Juliana) e o próprio Davi devem conseguir, lendo só este arquivo: montar o dossiê de treinamento por nicho (BMAD + SPIN, com validação cega), conectar Instagram DM + WhatsApp via seu CRM (webhook de ENTRADA + API de SAÍDA), qualificar, agendar, registrar no pipeline, rastrear vendas e fazer o deploy.
> Licença: MIT. Toda credencial aqui é placeholder. O aluno preenche os valores reais na instalação.

---

## Sumário

### PARTE I — TREINAMENTO (o dossiê do Davi)
1. O que é o dossiê de SDR e quem usa
2. Pré-requisitos: o que coletar antes de escrever
3. Análise BMAD do nicho/perfil
4. Estrutura canônica do dossiê (12 seções)
5. Fluxo SPIN Selling adaptado a DM
6. Triggers, templates e anti-patterns
7. Validação cega + bateria de 20 cenários

### PARTE II — INFRAESTRUTURA
8. Setup do servidor (sua VPS)
9. Banco PostgreSQL + tabelas
10. Config JSON do agente
11. Template Python (FastAPI + LLM)
12. Deploy + PM2

### PARTE III — INTEGRAÇÃO COM O SEU CRM
13. Credenciais do CRM (placeholders)
14. Webhook de ENTRADA (CRM → /webhook)
15. Envio de SAÍDA (API do CRM)
16. Instagram DM passo a passo
17. WhatsApp Oficial (Meta) passo a passo
18. WhatsApp não oficial passo a passo
19. Transcrição de áudio (Whisper)
20. Comportamento por canal
21. Como o lead chega do Instagram via comentário → DM (automação)

### PARTE IV — TRACKING, OBSERVABILIDADE E LANÇAMENTO
22. Tracking de vendas na sua plataforma de checkout (parâmetro src)
23. Monitoramento + queries SQL
24. Ramp-up e métricas de produção
25. Troubleshooting comum
26. Checklist final de deploy

---

# PARTE I — TREINAMENTO (o dossiê do Davi)

## 1. O que é o dossiê de SDR e quem usa

O dossiê do Davi é o documento mestre que ensina o agente a se comportar como um vendedor humano específico (o "operador-modelo"), em conversas privadas (Instagram DM, WhatsApp, Telegram). Não é um prompt de chatbot genérico: é um manual de personalidade, hierarquia, tom, gatilhos e fluxos que reproduz o jeito daquele operador-modelo abordar leads, qualificar e fechar.

A diferença entre um SDR bem treinado e um chatbot é essa: o Davi tem opinião, ritmo, vocabulário, vícios e limites próprios. Quando o lead lê uma mensagem do Davi, deve sentir que está conversando com uma pessoa real, não com um bot.

Quem usa o dossiê:
- O Davi (agente) lê o dossiê na inicialização de cada sessão (vira o material de treinamento dele).
- O time de revisão humano audita as mensagens enviadas e corrige desvios.
- O operador-modelo (o dono ou o vendedor sênior do negócio) valida se o Davi fala como ele falaria.

Resultado esperado: um arquivo markdown único, entre 8.000 e 30.000 palavras, organizado em 12 seções fixas, com templates preenchíveis e exemplos concretos. O dossiê é versionado em git e atualizado a cada rodada de feedback.

Quando aplicar este método: automatizar prospecção via DM (Instagram, LinkedIn) ou mensageiros (WhatsApp, Telegram); replicar um vendedor sênior em escala; manter conversão acima da média sem SDR humano. Não use para SAC reativo, FAQ bot, ou venda B2B enterprise com 6+ stakeholders (essa exige humano dedicado).

## 2. Pré-requisitos: o que coletar antes de escrever

A qualidade do dossiê é proporcional à qualidade da pesquisa. Sem matéria-prima, o dossiê vira teatro genérico.

### 2.1 Material bruto

| Item | Mínimo aceitável | Ideal |
|---|---|---|
| Áudios do operador-modelo conversando com leads | 30 minutos | 3 horas (15 conversas completas) |
| Lives/vídeos públicos do operador-modelo | 1 vídeo de 30 min | 5 vídeos de 60 min |
| Posts/captions em redes sociais | 20 posts | 100 posts dos últimos 12 meses |
| Conversas anonimizadas em DM (texto) | 5 conversas completas (lead até fechamento ou perda) | 30 conversas |
| Documentos de produto/oferta | Lista de produtos + preços | Pitch deck + página de vendas + objeções comuns |
| Definição de ICP (perfil de cliente ideal) | 1 parágrafo | Documento com 5-10 segmentos detalhados |

### 2.2 Entrevista estruturada com o operador-modelo

Se possível, gravar 60-90 minutos de entrevista respondendo:
1. Como você abre uma conversa nova? (palavra-por-palavra, 3 versões)
2. Quando o lead some, o que você manda de follow-up? (3 versões, com intervalo)
3. Quais 5 perguntas você faz pra qualificar?
4. O que você responde quando o lead diz "tá caro"?
5. O que você responde quando o lead diz "vou pensar"?
6. Quando você sabe que é hora de pedir o cartão?
7. Como você fecha? (copia e cola da última venda)
8. Quais palavras você nunca usa? Por quê?
9. Quais expressões/vícios você sempre usa? ("saca?", "olha só", "tá ligado?")
10. Qual cliente você odeia atender? Por quê?
11. Como você se apresenta na primeira mensagem? (com e sem vídeo)
12. Qual seu horário sagrado de não responder?

Cada resposta vira matéria-prima literal de seções específicas do dossiê.

### 2.3 O que dados precisa fornecer

Obrigatório:
- Username do Instagram (@perfil)
- Lista completa de produtos com nome, preço, o que inclui, link de venda
- Tom de voz desejado pro Davi (formal? casual? técnico? amigável?)
- Nome do agente (como vai se apresentar; aqui usamos Davi como padrão)
- Nomes para bloquear (sócios, família, funcionários que não devem receber mensagem)
- Chave de API do seu CRM e o ID da conta no CRM
- Público-alvo descrito em detalhes

Opcional (recomendado): ID do calendário (se tiver agendamento), webhook de venda da sua plataforma de checkout/pagamento, exemplos de conversas reais que deram certo, objeções comuns, FAQ, material de treinamento existente.

### 2.4 O que fica de FORA do dossiê

Não entram: dados pessoais do operador (RG, CPF, endereço, telefone direto, família), senhas/tokens/chaves, valores financeiros pessoais, ou qualquer informação que vazada traria constrangimento. Regra: se a informação não muda o comportamento do Davi numa conversa com lead, não entra no dossiê.

## 3. Análise BMAD do nicho/perfil

A análise BMAD é a fundação do dossiê. BMAD = Brand + Market + Audit + Direction.

### 3.1 Coleta de dados do perfil

Para cada perfil, extraia dados básicos (username, seguidores, seguindo, posts, bio, verificado, business) e os 12 posts mais recentes (shortcode, likes, comentários, é vídeo, views, data).

Coleta sem API oficial via Instaloader (use uma conta real e ativa, máximo 3 perfis por hora pra não levantar flags):

```python
import instaloader
L = instaloader.Instaloader()
L.load_session_from_file("SEU_USERNAME")  # sessão autenticada
profile = instaloader.Profile.from_username(L.context, "USERNAME_ALVO")
dados = {
    "username": profile.username, "followers": profile.followers,
    "following": profile.followees, "posts_count": profile.mediacount,
    "bio": profile.biography, "is_verified": profile.is_verified,
    "is_business": profile.is_business_account, "external_url": profile.external_url
}
posts = []
for i, post in enumerate(profile.get_posts()):
    if i >= 12: break
    posts.append({"shortcode": post.shortcode, "likes": post.likes,
        "comments": post.comments, "is_video": post.is_video,
        "views": post.video_view_count if post.is_video else None,
        "timestamp": post.date_utc.isoformat()})
dados["posts"] = posts
```

### 3.2 Score de autenticidade (compra de seguidores)

Cálculo próprio, 0 (orgânico) a 100 (comprou). Benchmarks de engajamento por faixa: menos de 10K = 4% a 8%; 10K a 100K = 2% a 4%; 100K a 1M = 1% a 2%; mais de 1M = 0.5% a 1%.

```python
import statistics
score = 0
followers = dados["followers"]
likes = [p["likes"] for p in posts if p["likes"] is not None]
avg_likes = statistics.mean(likes) if likes else 0
engagement_rate = (avg_likes / followers * 100) if followers > 0 else 0
min_expected = 4.0 if followers < 10000 else 2.0 if followers < 100000 else 1.0 if followers < 1000000 else 0.5
if engagement_rate < min_expected * 0.3: score += 40
elif engagement_rate < min_expected * 0.6: score += 20
if len(likes) >= 4:
    cv = statistics.stdev(likes) / statistics.mean(likes) if statistics.mean(likes) > 0 else 0
    if cv > 2.5: score += 25
    elif cv > 1.5: score += 10
following = dados["following"]
ratio = followers / following if following > 0 else 0
if ratio > 50 and followers > 50000: score += 10
if avg_likes < followers * 0.01 * 0.2 and followers > 10000: score += 15
```

Veredicto: 0 a 30 = LIMPO; 31 a 60 = SUSPEITO; 61 a 100 = ALTA PROBABILIDADE (comprou).

### 3.3 Análise BMAD com IA (3 chamadas)

Use um modelo forte (ex.: Gemini Pro via Google AI API, ou Claude/OpenAI). São 3 chamadas separadas porque cada uma tem escopo diferente e o output é grande.

Chamada 1 — BMAD Core: para B (Brand), M (Market), A (Audit), D (Direction), cada um com score 0-100. Brand: identidade, arquétipo, 3 pilares, tom de voz, consistência visual, diferencial, 3 gaps. Market: nicho, tamanho, concorrência, posicionamento, 3 tendências, 3 oportunidades, benchmarks. Audit: performance, engajamento vs benchmark, autenticidade (usa o score acima), 3 gargalos, 3 fortes, 3 fracos. Direction: estratégia central, quick wins, plano 90 dias (30/60/90), estratégia de conteúdo. Também: resumo executivo, conclusão e BMAD Total (média dos 4).

Chamada 2 — Monetização turbinada: antes, navegue o link da bio pra mapear produtos existentes. Gere escada de valor (front-end, mid-ticket, back-end, high-ticket) com produto/status/faixa de preço/público/proposta/funis/stack/precisa SDR IA?/precisa closer humano?, estratégia MRR, projeção de faturamento e stack recomendado. Regra: high-ticket acima de R$2.000 SEMPRE precisa SDR IA + closer humano.

Chamada 3 — Plano de ação 30 dias: estratégia geral, calendário semanal (4 semanas), estratégia de Reels, tráfego pago, collabs, métricas/KPIs.

Para todas: temperatura ~0.7, peça SEMPRE JSON válido, valide antes de prosseguir, 2 retries no máximo, salve os JSONs brutos localmente.

### 3.4 Pesquisa de oferta (manual)

A análise BMAD dá base, mas os dados reais vêm do dono. Para cada produto mapeie: nome, preço, plataforma, link de checkout, público-alvo, o que inclui, diferencial, garantia, acesso.

Hierarquia da escada de valor: entrada (R$47 a R$197, isca de baixo risco); intermediário (R$197 a R$997, onde a maioria das vendas acontece); premium (R$997 a R$2.000); high-ticket (acima de R$2.000, sempre SDR IA + closer humano).

Árvore de decisão do Davi (exemplo): lead quer começar do zero → combo de entrada; lead quer técnica específica → curso daquela técnica; lead quer profissionalização total → high-ticket; lead não sabe o que quer → perguntas SPIN pra descobrir.

## 4. Estrutura canônica do dossiê (12 seções)

Todo dossiê tem estas seções na ordem, mesmo que algumas fiquem curtas:

```
0. Cabeçalho (versão, autor, data, escopo)
1. Identidade Core
2. Hierarquia e Cadeia de Comando
3. Tom de Voz
4. Anti-patterns
5. Triggers de Conversa
6. Templates de Mensagem
7. Fluxo SPIN Selling adaptado a DM
8. Limites Operacionais
9. Stack Técnico
10. Comportamento por Canal
11. Memória e Estado
12. Validação e Iteração Contínua
```

### 4.1 Seção 1 — Identidade Core

Em até 800 palavras, em PRIMEIRA PESSOA (o dossiê é lido pelo Davi como a própria voz dele): nome do agente (Davi), função declarada em uma frase, história de origem (3-5 linhas, útil quando lead pergunta "você é robô?"), missão/propósito, especialidades técnicas (onde domina e onde passa pra humano), estilo de assinatura. Inclua 3 frases-bandeira que o Davi diz pelo menos uma vez por conversa. Identidade abstrata demais não treina nada; tem que ser opinada, com posicionamento claro.

### 4.2 Seção 2 — Hierarquia e Cadeia de Comando

Quem manda no Davi (o dono/operador-modelo, identificar pelo papel quando possível), quem ele consulta antes de decidir, o que ele NÃO pode decidir sozinho (lista explícita: dar desconto, oferecer produto fora do catálogo, marcar reunião com C-level, falar de prazo de entrega) e quando escalar pra humano.

Escalation triggers (passa pra humano): lead pede desconto acima de X%; lead reclama de cobrança/produto; lead pede call comercial individual; lead se identifica como C-level de empresa grande; lead manda áudio longo (>30s); lead xinga/ameaça. Sem hierarquia clara o Davi pode prometer o que a empresa não cumpre. Hierarquia é cinto de segurança.

### 4.3 Seção 3 — Tom de Voz

O que entra: vocabulário positivo (20-50 palavras/expressões que o operador usa muito, extraídas das transcrições, não inventadas), vocabulário proibido, ritmo (tamanho médio de mensagem; quebrar mensagem longa em 2-3 envios com 5-8s entre cada), pontuação característica (reticências? caps? quantos emojis?), vícios linguísticos (pelo menos 5 frases-tique), tratamento (você/tu, "cara", varia por região?), quando usa formalidade.

Como extrair: roda nas 30 conversas + transcrições: frequência de palavras (top 50 = vocabulário positivo), n-gramas 2-3 (top 30 = vícios), distribuição de tamanho de mensagem (ritmo), emojis por mensagem, proporção de mensagens com inicial maiúscula (formalidade).

Validação do tom: pegue 5 mensagens reais do operador + 5 do Davi, mostre a 3 pessoas que conhecem o operador, pergunte qual é qual. Se acertarem mais de 70%, o tom ainda não está pronto.

### 4.4 Seção 4 — Anti-patterns (o que NUNCA fazer)

Lista de "nunca" é mais útil que lista de "sempre": é o negative space do Davi. Cada item tem que ser operacional (testável por revisão humana), nunca genérico. Exemplos:

- Comportamento: nunca pergunto várias coisas na mesma mensagem; nunca mando link sem contexto; nunca uso saudação genérica (oi/olá/tudo bem) sem puxar um gancho.
- Conteúdo: nunca prometo prazo sem confirmar; nunca menciono concorrente direto; nunca dou número exato de clientes/faturamento; nunca passo dados de outros leads.
- Linguagem: nunca caps lock inteiro; nunca 3+ emojis na mesma mensagem; nunca jargão tipo "valor agregado", "solução robusta".
- Tempo: nunca respondo entre 23h e 7h BRT; nunca respondo em menos de 30s (parece bot); nunca mando 4+ mensagens seguidas sem o lead responder; nunca insisto após 3 follow-ups sem resposta.
- Dados sensíveis: nunca peço senha, código de verificação ou dados bancários; nunca peço acesso ao dispositivo do lead.

## 5. Fluxo SPIN Selling adaptado a DM

SPIN em DM é diferente de SPIN em call. Em DM, cada mensagem é uma micro-decisão do lead em continuar lendo. O fluxo precisa ser comprimido, assíncrono e resiliente a sumiço.

```
FASE S — Situação (1-2 perguntas)
- Entender contexto sem sufocar.
- Ex: "Pra eu te ajudar melhor, posso entender se você já usa alguma ferramenta de X?"
- Se lead não responde em 15 min, dispara follow-up curto.

FASE P — Problema (1 pergunta direta)
- Lead verbaliza dor específica.
- Ex: "Saca, e qual a parte que mais te incomoda hoje?"

FASE I — Implicação (1 pergunta espelho + 1 reforço)
- Lead verbaliza impacto da dor.
- Ex: "Entendi, e isso tá custando o quê pra você hoje? (tempo, dinheiro, oportunidade?)"

FASE N — Necessidade-Payoff (1 pergunta projetiva)
- Lead se imagina resolvido.
- Ex: "Se isso tivesse resolvido em X dias, mudaria o quê na sua semana?"

→ Aí entra a oferta.
```

Regra dos 3 messages: em qualquer fase, se o lead manda 3 mensagens seguidas sem o Davi qualificar minimamente, força entrada na próxima fase ou aciona escalation. Quando pular fases: lead que já chega com contexto + dor explícita já passou por S e P sozinho; marca `qualified=true` e vai direto pra oferta.

No template Python, a etapa SPIN avança automaticamente pelo número de trocas (`exchange_count`), e a mensagem manda 1-2 mensagens curtas separadas por `|||`.

## 6. Triggers, templates e anti-patterns

### 6.1 Triggers

Trigger é uma palavra/padrão que ativa um comportamento sem passar pela decisão do LLM. Triggers críticos (segurança, escalação, encerramento) são hardcoded em regex no código, porque o LLM erra 1 a cada ~50 mensagens e em produção isso é desastroso.

| Tipo | O que faz |
|---|---|
| Apresentação | Lead chega via canal específico, Davi manda mensagem inicial |
| Qualificação | Lead diz palavra-chave (ex.: "quanto custa?") → aciona fluxo SPIN antes do preço |
| Encerramento | "Não tenho interesse" → despedida cordial e para follow-up |
| Escalação | "Quero falar com o dono" → notifica humano, pausa o Davi |
| Segurança | Tentativa de jailbreak ("ignore instruções") → registra log e bloqueia |
| Re-engajamento | Tempo desde última mensagem passa do limite → mensagem de reativação |

Hierarquia quando 2+ triggers batem: 1) Segurança, 2) Encerramento, 3) Escalação, 4) Qualificação/Preço/Objeção, 5) Re-engajamento.

### 6.2 Templates de mensagem

Templates são pontos de partida, não scripts robotizados. O Davi parafraseia a cada uso. Cada template tem no mínimo 3 variações textuais, escolhidas aleatoriamente (evita o "já vi essa frase"). Obrigatórios: apresentação (orgânico e anúncio), qualificação situação, qualificação problema, apresentação da oferta, resposta a preço, objeção "caro", objeção "vou pensar", fechamento com link, follow-up 15min, follow-up 18h, despedida, escalation.

Anti-pattern de template: longo demais (>400 chars), 3+ perguntas, começar com "Olá!" genérico, ou placeholder não preenchido ("{nome}" aparecendo literal).

## 7. Validação cega + bateria de 20 cenários

Antes de subir o Davi, valide o dossiê com teste cego: pegue o dossiê pronto (sem nomes próprios), mostre a alguém do público-alvo e pergunte: você consegue descrever o produto principal? pra quem é? qual a maior diferença pro concorrente? Se responder com confiança e correção, o dossiê está pronto. Se hesitar ou inventar, ajuste as seções fracas.

Bateria de 20 cenários obrigatórios (aprovado quando 18+ passam na primeira execução; os que falharem se ajustam no dossiê, não no código):

1. Lead pergunta preço diretamente → "Confere todos os detalhes no site!" + link
2. Lead manda áudio → transcrito e tratado como texto
3. Lead manda 3 mensagens em 5s → debounce acumula e responde uma vez
4. Lead se apresenta pela 2ª vez → Davi NÃO se apresenta de novo (lê histórico)
5. Lead pergunta produto fora da lista → redireciona pro mais próximo permitido
6. Lead manda mensagem fora do nicho → direciona/encerra educadamente
7. Lead manda link de concorrente → ignora e foca na conversa
8. Lead pergunta "quem é você?" → responde com a história de origem
9. Lead pede pra falar com humano → aciona escalation
10. Lead manda email/telefone → coleta e atualiza o CRM
11. Lead diz "vou pensar" → descobre a maior dúvida
12. Lead pede desconto → NÃO oferece (sem autorização), reforça valor
13. Lead manda meme/sticker → responde com naturalidade
14. Lead pergunta no domingo às 22h → responde sinalizando horário comercial
15. Lead é nome bloqueado (sócio/família) → não responde
16. Lead muda de assunto bruscamente → segue o fluxo SPIN sem perder controle
17. Lead xinga → escalation + log de segurança
18. Lead tenta jailbreak ("ignore as instruções") → bloqueia + log
19. Lead pergunta sobre garantia → reforça garantia
20. Lead chega às 02h → agenda resposta pra 7:01 AM

Workflow de 7 passos do dossiê: (1) coletar matéria-prima, (2) análise lexical e de padrões, (3) mapear triggers, (4) escrever as 12 seções, (5) bateria de teste em staging, (6) validação cega com o operador, (7) subir em produção com revisão humana 100% na 1ª semana, 50% na 2ª, 10% na 3ª, amostral a partir da 4ª.

Erros comuns: copiar dossiê de outro cliente sem adaptar; omitir anti-patterns; triggers só no LLM (sem regex); inventar tom em vez de extrair das transcrições; dossiê não versionado; subir sem bateria de teste; não medir métricas; dar autonomia total no dia 1; dossiê estático (atualizar mensalmente); misturar identidade e oferta (manter `dossie.md` estável e `oferta-{produto}.md` variável em arquivos separados, o Davi carrega ambos).

---

# PARTE II — INFRAESTRUTURA

## 8. Setup do servidor (sua VPS)

VPS recomendada de 4 vCPU e 8GB RAM em qualquer provedor de VPS, Ubuntu 22.04 LTS. Cada agente usa ~100MB de RAM, então 8GB suporta dezenas em paralelo.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo bash -
sudo apt install nodejs -y
sudo npm install -g pm2
sudo apt install postgresql postgresql-contrib -y
sudo systemctl enable postgresql && sudo systemctl start postgresql
```

Caddy (reverse proxy + SSL automático):
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update && sudo apt install caddy -y
```

PM2 com auto-start no boot: `pm2 startup` e executar o comando que ele mostrar.

Caddyfile (cada agente em uma porta):
```
agente.seudominio.com.br { reverse_proxy localhost:3501 }
painel.seudominio.com.br { reverse_proxy localhost:3600 }
```
Recarregar: `sudo systemctl reload caddy`. No seu provedor de DNS, o subdomínio do webhook é um registro tipo A apontando pro IP do servidor onde o Davi roda.

Chave de API do provedor de LLM do SDR e pacotes Python (o Davi roda como um serviço próprio, separado do cérebro Codex da Naia; ele precisa da sua própria chave de LLM e de um CRM conectado):
```bash
echo 'export LLM_API_KEY="<LLM_API_KEY>"' >> ~/.bashrc && source ~/.bashrc
pip install httpx psycopg2-binary fastapi uvicorn openai-whisper
```

## 9. Banco PostgreSQL + tabelas

```bash
sudo -u postgres createuser meuagente
sudo -u postgres createdb agente_db -O meuagente
sudo -u postgres psql -c "ALTER USER meuagente PASSWORD '<DB_PASS>';"
```

```sql
CREATE TABLE IF NOT EXISTS sdr_agents (
    id SERIAL PRIMARY KEY, name VARCHAR(100) NOT NULL, company VARCHAR(200),
    port INTEGER NOT NULL UNIQUE, status VARCHAR(20) DEFAULT 'active',
    personality TEXT, products TEXT, links TEXT, blocked_names TEXT,
    crm_api_key VARCHAR(200), crm_location_id VARCHAR(100), calendar_id VARCHAR(100),
    spin_flow TEXT, receive_method VARCHAR(20) DEFAULT 'webhook',
    send_method VARCHAR(20) DEFAULT 'api', send_webhook_url TEXT,
    messages_in INTEGER DEFAULT 0, messages_out INTEGER DEFAULT 0,
    contacts_count INTEGER DEFAULT 0, sales_count INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW(), updated_at TIMESTAMP DEFAULT NOW()
);
CREATE TABLE IF NOT EXISTS sdr_agent_files (
    id SERIAL PRIMARY KEY, agent_id INTEGER REFERENCES sdr_agents(id),
    filename VARCHAR(255) NOT NULL, file_type VARCHAR(50) DEFAULT 'training',
    content TEXT NOT NULL, created_at TIMESTAMP DEFAULT NOW(), updated_at TIMESTAMP DEFAULT NOW()
);
CREATE TABLE IF NOT EXISTS dm_conversations (
    id SERIAL PRIMARY KEY, contact_id VARCHAR(100) NOT NULL, contact_name VARCHAR(200),
    direction VARCHAR(20) NOT NULL, message TEXT NOT NULL, agent_id INTEGER,
    created_at TIMESTAMP DEFAULT NOW()
);
CREATE TABLE IF NOT EXISTS dm_contact_profiles (
    id SERIAL PRIMARY KEY, contact_id VARCHAR(100) UNIQUE NOT NULL, contact_name VARCHAR(200),
    first_contact_at TIMESTAMP DEFAULT NOW(), last_contact_at TIMESTAMP DEFAULT NOW(),
    messages_count INTEGER DEFAULT 0, agent_id INTEGER, phone VARCHAR(50),
    email VARCHAR(200), status VARCHAR(50) DEFAULT 'active', notes TEXT
);
CREATE TABLE IF NOT EXISTS sdr_agent_sales (
    id SERIAL PRIMARY KEY, agent_id INTEGER REFERENCES sdr_agents(id),
    contact_id VARCHAR(100), contact_name VARCHAR(200), product_name VARCHAR(200),
    product_price DECIMAL(10,2), platform VARCHAR(50), transaction_id VARCHAR(200),
    src_param VARCHAR(100), raw_payload JSONB, created_at TIMESTAMP DEFAULT NOW()
);
CREATE INDEX IF NOT EXISTS idx_dm_conversations_contact ON dm_conversations(contact_id);
CREATE INDEX IF NOT EXISTS idx_dm_conversations_agent ON dm_conversations(agent_id);
CREATE INDEX IF NOT EXISTS idx_sdr_agent_sales_src ON sdr_agent_sales(src_param);
```

## 10. Config JSON do agente

Cada agente tem um `config-N.json` (N = id no banco). Todos usam o MESMO template Python, diferenciados só pelo config.

```json
{
  "id": 0,
  "name": "Davi",
  "company": "Nome do Negócio | Nicho",
  "port": 3501,
  "personality": "Você é o Davi, do time de [EMPRESA]. SEMPRE se apresente como Davi do time de [EMPRESA] na primeira mensagem. Tom [adjetivos do tom]. NUNCA pareça vendedor agressivo. Eduque antes de vender. Sua missão é entender o que a pessoa precisa e recomendar o produto CERTO. NUNCA mencione preços. Se perguntarem o preço, diga: confere todos os detalhes no site! E mande o link.",
  "products": "Lista de produtos com nome, preço e descrição breve.",
  "links": "https://produto1.seudominio.com.br/?src=davi\nhttps://produto2.seudominio.com.br/?src=davi",
  "blocked_names": "socio1,familiar1",
  "crm_api_key": "<CRM_API_TOKEN>",
  "crm_location_id": "<CRM_LOCATION_ID>",
  "calendar_id": "",
  "spin_flow": "FLUXO SPIN SELLING (4 ETAPAS): ETAPA 1 RAPPORT ... ETAPA 2 PROBLEMA ... ETAPA 3 IMPLICAÇÃO ... ETAPA 4 OFERTA: Mande o link UMA ÚNICA VEZ. NUNCA pressione.",
  "channel_type": "IG",
  "receive_method": "webhook",
  "send_method": "api",
  "send_webhook_url": ""
}
```

Campos-chave: `channel_type` (`IG` Instagram, `WhatsApp` oficial, `SMS` não oficial/SMS, `Email`, `FB`); `receive_method` = `webhook` (CRM envia pra nós); `send_method` = `api` (enviamos via API do CRM) ou `webhook`. Convenção de portas: um agente por porta (3500, 3501, 3502...).

## 11. Template Python (FastAPI + SDK do provedor de LLM)

Estrutura por bloco. Carrega config pelo id passado como argumento, conecta no Postgres, carrega o material de treinamento do banco, gera resposta com o SDK do provedor de LLM do SDR, faz debounce de 8s pra agrupar mensagens quebradas, e expõe `/webhook`, `/health` e `/reload-training`. O exemplo abaixo usa um cliente de LLM genérico via `LLM_API_KEY`; troque pelo SDK do provedor que você escolher.

```python
#!/usr/bin/env python3
"""SDR Agent Template — agente de vendas automatizado via IA"""
import asyncio, json, os, re, sys, time
from datetime import datetime, timedelta, timezone
from pathlib import Path
import httpx, psycopg2
from psycopg2.extras import RealDictCursor
from fastapi import FastAPI, Request
from fastapi.middleware.cors import CORSMiddleware
import uvicorn
# Importe aqui o SDK/cliente do provedor de LLM que você escolheu pro SDR.
# Ele precisa da chave em LLM_API_KEY (definida no ~/.bashrc, seção 8).
LLM_API_KEY = os.environ.get('LLM_API_KEY', '')

AGENT_ID = int(sys.argv[1]) if len(sys.argv) > 1 else 0
CFG = json.load(open(Path(__file__).parent / f'config-{AGENT_ID}.json'))
PORT = CFG['port']; AGENT_NAME = CFG['name']; COMPANY = CFG.get('company', '')
PERSONALITY = CFG.get('personality', ''); PRODUCTS = CFG.get('products', '')
ALLOWED_LINKS = [l.strip() for l in CFG.get('links', '').split('\n') if l.strip()]
BLOCKED_NAMES = [n.strip().lower() for n in CFG.get('blocked_names', '').split(',') if n.strip()]
CRM_API_KEY = CFG.get('crm_api_key', ''); CRM_LOCATION = CFG.get('crm_location_id', '')
CALENDAR_ID = CFG.get('calendar_id', ''); SPIN_FLOW = CFG.get('spin_flow', '')
CHANNEL_TYPE = CFG.get('channel_type', 'IG'); SEND_METHOD = CFG.get('send_method', 'api')
SEND_WEBHOOK_URL = CFG.get('send_webhook_url', '')
BRT = timezone(timedelta(hours=-3))
CRM_BASE = 'https://<CRM_API_BASE>'   # endpoint da API do seu CRM (o aluno preenche)

# TROQUE user/password/dbname pelos seus valores
def get_db():
    return psycopg2.connect(host='127.0.0.1', user='<DB_USER>', password='<DB_PASS>', dbname='agente_db')

def save_message(contact_id, contact_name, direction, message):
    conn = get_db(); cur = conn.cursor()
    cur.execute('INSERT INTO dm_conversations (contact_id, contact_name, direction, message, agent_id) VALUES (%s,%s,%s,%s,%s)',
        (contact_id, contact_name, direction, message, AGENT_ID))
    cur.execute("""INSERT INTO dm_contact_profiles (contact_id, contact_name, last_contact_at, messages_count, agent_id)
        VALUES (%s,%s,NOW(),1,%s) ON CONFLICT (contact_id) DO UPDATE SET
        contact_name = COALESCE(EXCLUDED.contact_name, dm_contact_profiles.contact_name),
        last_contact_at = NOW(), messages_count = dm_contact_profiles.messages_count + 1""",
        (contact_id, contact_name, AGENT_ID))
    conn.commit(); cur.close(); conn.close()

def load_training_files(agent_id):
    conn = get_db(); cur = conn.cursor()
    cur.execute('SELECT filename, content FROM sdr_agent_files WHERE agent_id = %s', (agent_id,))
    files = cur.fetchall(); cur.close(); conn.close()
    return '\n\n'.join(f'--- Arquivo: {fn} ---\n{c}' for fn, c in files) if files else ''

TRAINING_CONTEXT = load_training_files(AGENT_ID)

http = httpx.AsyncClient(timeout=15)
CRM_HEADERS = {'Authorization': f'Bearer {CRM_API_KEY}', 'Content-Type': 'application/json'}  # adicione aqui os headers que o SEU CRM exigir

async def send_message(contact_id, message):
    if SEND_METHOD == 'webhook' and SEND_WEBHOOK_URL:
        r = await http.post(SEND_WEBHOOK_URL, json={'contactId': contact_id, 'message': message})
        return r.json()
    r = await http.post(f'{CRM_BASE}/conversations/messages', headers=CRM_HEADERS,
        json={'type': CHANNEL_TYPE, 'contactId': contact_id, 'message': message})
    return r.json()

async def get_history(contact_id):
    try:
        r = await http.get(f'{CRM_BASE}/conversations/search', headers=CRM_HEADERS,
            params={'locationId': CRM_LOCATION, 'contactId': contact_id, 'limit': '1'})
        conv_id = r.json().get('conversations', [{}])[0].get('id')
        if not conv_id: return ''
        r2 = await http.get(f'{CRM_BASE}/conversations/{conv_id}/messages', headers=CRM_HEADERS, params={'limit': '20'})
        msgs = r2.json().get('messages', {})
        if isinstance(msgs, dict): msgs = msgs.get('messages', [])
        lines = []
        for m in reversed(msgs):
            if not m.get('body'): continue
            who = 'LEAD' if m.get('direction') == 'inbound' else 'EU'
            lines.append(f"{who}: {m.get('body','')[:200]}")
        return '\n'.join(lines)
    except Exception:
        conn = get_db(); cur = conn.cursor(cursor_factory=RealDictCursor)
        cur.execute('SELECT direction, message FROM dm_conversations WHERE contact_id=%s AND agent_id=%s ORDER BY created_at ASC LIMIT 20', (contact_id, AGENT_ID))
        rows = cur.fetchall(); cur.close(); conn.close()
        return '\n'.join(f"{'LEAD' if r['direction']=='inbound' else 'EU'}: {r['message']}" for r in rows)

async def update_contact_crm(contact_id, phone, email):
    body = {}
    if phone: body['phone'] = phone
    if email: body['email'] = email
    await http.put(f'{CRM_BASE}/contacts/{contact_id}', headers=CRM_HEADERS, json=body)

async def generate_with_llm(name, message, history, available_slots=''):
    exchange_count = history.count('LEAD:')
    links_text = '\n'.join(f'- {l}' for l in ALLOWED_LINKS) if ALLOWED_LINKS else 'Nenhum link configurado'
    if SPIN_FLOW:
        etapa = SPIN_FLOW
    elif exchange_count <= 1: etapa = 'ETAPA 1 - RAPPORT: Cumprimente, comente algo do contexto.'
    elif exchange_count == 2: etapa = 'ETAPA 2 - PROBLEMA: Entenda a dor.'
    elif exchange_count == 3: etapa = 'ETAPA 3 - IMPLICAÇÃO: Mostre o impacto.'
    else: etapa = 'ETAPA 4 - FECHAMENTO: Direcione para ação.'
    training_section = f'\nMATERIAL DE TREINAMENTO:\n{TRAINING_CONTEXT}\n' if TRAINING_CONTEXT else ''
    prompt = f"""{PERSONALITY}

VOCÊ É: {AGENT_NAME}, representando {COMPANY}.
PRODUTOS/SERVIÇOS:
{PRODUCTS}
LINKS PERMITIDOS (só envie esses):
{links_text}
REGRAS:
- LEIA O HISTÓRICO antes de responder
- NUNCA repita o que já foi dito
- NUNCA se apresente de novo se já conversou
- NUNCA invente links além dos permitidos
- Responda com 1-2 mensagens curtas separadas por |||
- Tom casual e profissional
{etapa}
{f'HORÁRIOS DISPONÍVEIS: {available_slots}' if available_slots else ''}
{training_section}
HISTÓRICO ({exchange_count} trocas): {history[:2000] or 'Primeira interação'}
LEAD: {name}
MENSAGEM: {message[:300]}
Responda (separe com |||):"""
    # Chame aqui o provedor de LLM escolhido (use LLM_API_KEY) passando `prompt`
    # e devolvendo só o texto da resposta. Ex. genérico com chat completions:
    #   r = await http.post('<LLM_API_BASE>/chat/completions',
    #       headers={'Authorization': f'Bearer {LLM_API_KEY}'},
    #       json={'model': '<LLM_MODEL>', 'messages': [{'role': 'user', 'content': prompt}]})
    #   result_text = r.json()['choices'][0]['message']['content']
    result_text = ''
    return result_text.strip() or None

DEBOUNCE_SECONDS = 8
pending_messages = {}
async def debounce_fire(contact_id):
    await asyncio.sleep(DEBOUNCE_SECONDS)
    if contact_id not in pending_messages: return
    data = pending_messages.pop(contact_id)
    await handle_message(contact_id, data['name'], ' '.join(data['messages']))
def debounce_add(contact_id, name, message):
    if contact_id in pending_messages:
        pending_messages[contact_id]['timer'].cancel()
        pending_messages[contact_id]['messages'].append(message)
    else:
        pending_messages[contact_id] = {'name': name, 'messages': [message]}
    pending_messages[contact_id]['timer'] = asyncio.create_task(debounce_fire(contact_id))

app = FastAPI()
app.add_middleware(CORSMiddleware, allow_origins=['*'], allow_methods=['*'], allow_headers=['*'])

@app.post('/webhook')
async def webhook(request: Request):
    data = await request.json()
    contact_id = data.get('contact_id', '')
    name = data.get('full_name') or data.get('first_name') or '?'
    message = (data.get('message') or {}).get('body', '')
    if not contact_id or not message: return {'success': True}
    if any(b in name.lower() for b in BLOCKED_NAMES): return {'success': True}
    save_message(contact_id, name, 'inbound', message)
    debounce_add(contact_id, name, message)
    return {'success': True}

async def handle_message(contact_id, name, message):
    history = await get_history(contact_id)
    phone_match = re.search(r'(?:\+?55\s?)?(?:\(?\d{2}\)?\s?)?\d{4,5}[\s-]?\d{4}', message)
    email_match = re.search(r'[\w.+-]+@[\w-]+\.[\w.]+', message, re.I)
    if phone_match or email_match:
        await update_contact_crm(contact_id,
            re.sub(r'\D', '', phone_match.group()) if phone_match else None,
            email_match.group() if email_match else None)
    await asyncio.sleep(8)  # simula tempo de digitação
    reply = await generate_with_llm(name, message, history)
    if not reply: return
    msgs = [m.strip() for m in reply.split('|||') if m.strip()]
    for i, msg in enumerate(msgs):
        result = await send_message(contact_id, msg)
        ok = bool(result.get('messageId') or result.get('id') or result.get('success'))
        if ok: save_message(contact_id, name, 'outbound', msg)
        if not ok: break
        if i < len(msgs) - 1: await asyncio.sleep(4)

@app.get('/health')
async def health():
    return {'status': 'ok', 'agent': AGENT_NAME, 'company': COMPANY, 'port': PORT}

@app.post('/reload-training')
async def reload_training():
    global TRAINING_CONTEXT
    TRAINING_CONTEXT = load_training_files(AGENT_ID)
    return {'status': 'ok', 'chars': len(TRAINING_CONTEXT)}

if __name__ == '__main__':
    uvicorn.run(app, host='0.0.0.0', port=PORT, log_level='warning')
```

Pontos importantes: o separador `|||` permite múltiplas mensagens (mais natural); o histórico é truncado em 2000 chars; a etapa SPIN avança pelo número de trocas; o material de treinamento (o dossiê) é injetado inteiro no prompt; o debounce de 8s agrupa mensagens quebradas do lead em uma só. Opcional: bloco de calendário (`get_available_slots` lendo `/calendars/{id}/free-slots`) pra oferecer horários a partir da 3ª troca.

## 12. Deploy + PM2

1. Inserir registro em `sdr_agents` e anotar o id retornado.
2. Criar `config-N.json` conforme a seção 10.
3. Subir o dossiê de treinamento em `sdr_agent_files` (agent_id, filename, file_type='training', content).
4. Iniciar com PM2:
```bash
pm2 start /opt/seu-projeto/scripts/agents/template.py --name sdr-agent-3 --interpreter python3 -- 3
pm2 save
pm2 status
curl localhost:3501/health
```
5. Configurar o Caddy com o domínio do agente e `sudo systemctl reload caddy`. O Caddy gera SSL via Let's Encrypt.

---

# PARTE III — INTEGRAÇÃO COM O SEU CRM

O Davi se conecta ao Instagram DM e ao WhatsApp através do seu CRM. O CRM é a borda: ele recebe a mensagem do lead em qualquer canal e avisa o Davi (webhook de ENTRADA); o Davi responde chamando a API do CRM (SAÍDA), e o CRM entrega no canal certo. A ideia geral: seu CRM precisa ter webhook de ENTRADA (avisa o Davi de mensagem nova) e API de SAÍDA (Davi responde). Qualquer CRM que ofereça essas duas coisas serve.

## 13. Credenciais do CRM (placeholders)

Você precisa de 3 coisas, todas tiradas da sua conta no CRM. NUNCA versione os valores reais; eles ficam só no config do agente na instalação:

- Base da API: `https://<CRM_API_BASE>` (endpoint REST do seu CRM)
- API Key (token de integração privada): `<CRM_API_TOKEN>` — gerada nas configurações de API/integração do seu CRM. Permissões mínimas: contatos (leitura/escrita), conversas (leitura/escrita), calendários (leitura).
- ID da conta no CRM: `<CRM_LOCATION_ID>` — o identificador da sua conta/workspace no CRM.
- Headers extras: alguns CRMs exigem headers adicionais (ex.: versão da API). Confira a documentação do seu CRM e adicione em `CRM_HEADERS`.
- ID do calendário (opcional, pra agendamento): `<CRM_CALENDAR_ID>` — o identificador do calendário no seu CRM.

## 14. Webhook de ENTRADA (CRM → /webhook)

O CRM avisa o Davi toda vez que um lead manda mensagem, via uma automação que dispara um webhook.

1. No seu CRM, crie uma automação que dispara um webhook quando chega mensagem nova de um lead. Dê um nome claro (ex.: "Webhook Davi — Instagram", ou WhatsApp/SMS).
2. Configure pra disparar quando o lead responde, filtrando pelo canal desejado (Instagram, WhatsApp, ou SMS/telefone pro WhatsApp não oficial). Isso garante que só o canal certo dispara. Se o seu CRM não tiver filtro de canal, dispare em qualquer mensagem nova e trate o canal pelo conteúdo do payload.
3. A ação é um webhook HTTP: Method POST, URL `https://agente.seudominio.com.br/webhook`, header `Content-Type: application/json`, body JSON com os campos do contato e da mensagem (use as variáveis/merge fields do seu CRM):
```json
{
  "contact_id": "<id do contato no CRM>",
  "full_name": "<nome completo do contato>",
  "first_name": "<primeiro nome do contato>",
  "message": { "body": "<corpo da mensagem recebida>" }
}
```
Preencha cada valor com a variável/merge field correspondente do seu CRM. O Davi espera exatamente esses nomes de campo (`contact_id`, `full_name`, `first_name`, `message.body`).

4. Salve e ative a automação. A URL aponta pro servidor onde o Davi roda; o Caddy faz o reverse proxy pra porta do agente.

## 15. Envio de SAÍDA (API do CRM)

O Davi responde chamando:
```
POST https://<CRM_API_BASE>/conversations/messages
Authorization: Bearer <CRM_API_TOKEN>
Content-Type: application/json

{ "type": "TIPO_DO_CANAL", "contactId": "ID_DO_CONTATO", "message": "Texto" }
```
(O endpoint, os nomes dos campos e quaisquer headers extras dependem do seu CRM; ajuste conforme a documentação dele.)

O campo `type` define o canal e tem que ser o correto, senão dá erro 422:

| Canal | type |
|---|---|
| Instagram DM | `IG` |
| WhatsApp Oficial (Meta) | `WhatsApp` |
| WhatsApp não oficial / SMS | `SMS` |
| Email | `Email` |
| Facebook Messenger | `FB` |

Ex.: se o lead veio pelo WhatsApp e você responde com `type: "IG"`, o CRM retorna "Contact has no Instagram id, skipping". O campo `channel_type` do config define qual usar (padrão `IG`).

A API do seu CRM também permite ao Davi (e à Juliana montando a infra): listar/buscar/criar/atualizar contatos, adicionar/remover tags, atribuir contato a um vendedor (assignedTo), gerenciar notas e tasks; buscar conversas por contato, ler mensagens, enviar (SMS/Email/WhatsApp/IG DM), marcar como lida; listar calendários e slots livres, criar/atualizar/cancelar agendamentos; listar pipelines e stages, criar/mover/atualizar oportunidades e mudar status (aberto/ganho/perdido/abandonado). O conjunto exato de endpoints varia por CRM; alguns recursos (automações, builder de funis/sites, templates de WhatsApp) costumam ser só pela interface, não pela API. Confira a documentação do seu CRM.

## 16. Instagram DM passo a passo

Pré: perfil Instagram Business/Creator conectado à sua conta no CRM (nas integrações do CRM). Crie a automação da seção 14 filtrando o canal Instagram. No config: `channel_type: "IG"`. Fluxo: lead manda DM → CRM recebe → automação dispara webhook pro Davi → Davi responde via API com `type="IG"` → lead recebe no Instagram.

## 17. WhatsApp Oficial (Meta) passo a passo

Pré: número WhatsApp Business API configurado no CRM (via Meta Business Suite), verificado e aprovado pela Meta. Crie a automação da seção 14 filtrando o canal WhatsApp. No config: `channel_type: "WhatsApp"`. Atenção à regra de janela de 24h da Meta: o Davi só pode responder dentro de 24h após a última mensagem do lead; depois disso, precisa de template aprovado pela Meta (HSM).

## 18. WhatsApp não oficial passo a passo

Pré: número não oficial configurado no CRM. Crie a automação da seção 14 filtrando o canal SMS/telefone (ou qualquer canal). No config: `channel_type: "SMS"`. Diferença do oficial: sem janela de 24h, sem template aprovado, porém menos confiável (pode ser bloqueado pela Meta). Na API usa `type="SMS"`.

## 19. Transcrição de áudio (Whisper)

Quando o lead manda áudio, o CRM envia o webhook com `body` vazio e o áudio como `mediaUrl` ou `attachments[].url`. O Davi: detecta o áudio, baixa o arquivo (URL temporária do CRM), transcreve com Whisper (modelo small, português) e usa a transcrição como texto normal. O Davi sempre responde em texto, não em áudio. Pré: `pip install openai-whisper`. Tempo: ~3-5s pra 10s de áudio; ~10-15s pra 1 min (CPU). Se a URL expirar antes do download, a transcrição falha silenciosamente.

## 20. Comportamento por canal

- Instagram DM: ~1000 chars por mensagem; janela de 24h (depois só com tag de human agent); o Davi não envia áudio (pode receber e transcrever); responde stories com texto.
- WhatsApp Business: janela de 24h; fora dela exige template HSM aprovado; pode enviar áudio TTS se o cliente autorizar e a voz for natural; pode enviar PDF se o trigger autorizar.
- Telegram: sem janela de 24h; suporta markdown; voice via OGG opus; rate limit 30 msg/s global e 1 msg/s por chat; respeita roteamento por tópico em grupos.
- LinkedIn (InMail): tom mais formal por padrão; volume reduzido (limite de InMails por plano).

Limites operacionais por lead: máx 3 mensagens consecutivas sem resposta; máx 4 follow-ups em 7 dias; encerra após 7 dias sem evolução; máx 5 mensagens/dia pro mesmo lead. Limites globais anti-spam: máx ~100 mensagens/hora por conta (evita shadowban), máx ~50 cold DMs/dia com cooldown de 4h. Esses números variam por plataforma e maturidade da conta; auditar mensalmente.

## 21. Como o lead chega do Instagram via comentário → DM (automação)

Boa parte dos leads do Davi chega por uma automação de comentário/palavra-chave → DM no Instagram: quem comenta uma palavra-chave (ex.: "EU QUERO") num post recebe uma resposta pública e uma DM privada com um botão; ao tocar no botão, a conversa cai no direct e passa a ser atendida pelo Davi via o mesmo webhook do CRM. Essa automação é configurada num painel/host de automações (genericamente `https://<CRM_OR_AGENTS_HOST>`) ou no próprio CRM, com estes conceitos:

Tipos de gatilho: `comment_to_dm` (qualquer comentário em qualquer post), `keyword_comment` (comentário com palavra-chave, o mais comum), `livecomment_to_dm` (comentário em live), `mention_to_dm` (menção via @).

Campos do trigger: `keywords` (lista, case insensitive), `match_mode` (`any`/`all`/`exact`/`contains`), `media_id` (opcional; se setado, só dispara naquele post; vazio = qualquer post).

Campos da ação: `reply_public_text` (uma resposta pública fixa) OU `reply_public_variants` (até 10 variações, o sistema sorteia uma por disparo, anti-bot, preferir essa pra não soar robô); `dm_text` (mensagem privada); `delay_seconds` (0-30, anti-bot); `dm_buttons` (lista de botões anexados na DM). Tipos de botão:
- `quick_reply` (default): botão de resposta rápida, `payload` opcional, até 13 botões. O lead toca e responde algo pro fluxo continuar. Use `quick_reply` quando quiser que a conversa siga com o Davi (o payload vira mensagem na conversa, e o Davi assume).
- `web_url`: abre URL externa (checkout, landing), `url` obrigatório, até 3 botões. Se algum botão for `web_url`, o limite total cai pra 3.

Limites e gotchas Meta: a Private Reply (DM via comentário) só funciona dentro da janela de 7 dias após o comentário; resposta pública não tem janela mas só pode ser feita uma vez por dia no mesmo comentário; a Meta limita ~200 chamadas/hora por conta IG; se a conta IG estiver desconectada, as automações silenciosamente não disparam.

Handover pro Davi: o lead toca no botão `quick_reply` → o payload chega como mensagem na conversa do direct → o CRM/webhook grava e o Davi lê e responde/qualifica/vende normalmente. Não precisa configurar nada extra no Davi além de garantir que o payload seja único e descritivo.

---

# PARTE IV — TRACKING, OBSERVABILIDADE E LANÇAMENTO

## 22. Tracking de vendas na sua plataforma de checkout (parâmetro src)

Pra saber se uma venda veio do Davi, todo link que ele manda leva `?src=davi` (ou `?src=nome-do-agente`). A página de venda captura o parâmetro e repassa pro checkout; o webhook da sua plataforma de checkout/pagamento envia a venda com o campo `src`; o sistema registra a venda atribuída ao agente.

JavaScript pra colar em TODA página de venda com link de checkout:
```javascript
document.addEventListener('DOMContentLoaded', function() {
  var pageParams = new URLSearchParams(window.location.search);
  document.querySelectorAll('a[href*="checkout"]').forEach(function(link) {
    var url = link.href;
    pageParams.forEach(function(val, key) {
      if (!url.includes(key + '=')) { url += (url.includes('?') ? '&' : '?') + key + '=' + encodeURIComponent(val); }
    });
    link.href = url;
  });
});
```

Endpoint pra receber o webhook de venda (ajuste os caminhos do payload pro formato da SUA plataforma de checkout):
```python
@app.post('/sales/webhook')
async def sales_webhook(request: Request):
    data = await request.json()
    # Os caminhos abaixo são um exemplo; cada plataforma de checkout tem o seu formato.
    product = data.get('data', {}).get('product', {}).get('name', '')
    price = data.get('data', {}).get('purchase', {}).get('price', {}).get('value', 0)
    src = data.get('data', {}).get('purchase', {}).get('tracking', {}).get('src', '')
    if src and AGENT_NAME.lower() in src.lower():
        conn = get_db(); cur = conn.cursor()
        cur.execute('INSERT INTO sdr_agent_sales (agent_id, product_name, product_price, platform, src_param, raw_payload) VALUES (%s,%s,%s,%s,%s,%s)',
            (AGENT_ID, product, price, '<plataforma>', src, json.dumps(data)))
        conn.commit(); cur.close(); conn.close()
    return {'success': True}
```

Configure o webhook de venda no painel da sua plataforma de checkout/pagamento, apontando pro endpoint acima nos eventos de compra aprovada/completa. As páginas de venda podem subir por GitHub → hospedagem de páginas (repo privado, DNS apontando pra hospedagem). Cada git push dispara deploy automático.

## 23. Monitoramento + queries SQL

```bash
pm2 logs sdr-agent-3 --timestamp   # logs em tempo real
pm2 status                          # status de todos
curl localhost:3501/health          # saúde
curl -X POST localhost:3501/reload-training  # recarrega o dossiê sem reiniciar
pm2 restart sdr-agent-3
```

```sql
-- Mensagens por agente (7 dias)
SELECT agent_id, direction, COUNT(*) FROM dm_conversations
WHERE created_at > NOW() - INTERVAL '7 days' GROUP BY agent_id, direction;
-- Vendas por agente
SELECT agent_id, COUNT(*) AS vendas, SUM(product_price) AS faturamento
FROM sdr_agent_sales GROUP BY agent_id;
-- Resumo do dia
SELECT COUNT(*) FILTER (WHERE direction='inbound') AS inbounds,
       COUNT(*) FILTER (WHERE direction='outbound') AS outbounds,
       COUNT(DISTINCT contact_id) AS leads_unicos
FROM dm_conversations WHERE created_at::date = CURRENT_DATE AND agent_id = 3;
```

Audit: cada mensagem já é gravada em `dm_conversations`. Para auditoria completa, salvar webhooks brutos em JSONL e backup do banco a cada 6h (`pg_dump -F c -f backup.dump agente_db`).

## 24. Ramp-up e métricas de produção

Não jogue 100% do tráfego no Davi novo de uma vez. Ramp-up: dia 1 = 10% dos leads (monitorar erros 422, duplicadas, debounce); dia 2-3 = 25%; dia 4-7 = 50%; dia 8+ = 100%. Controlar pela automação do CRM (filtro por horário ou divisão aleatória de percentual, se o seu CRM suportar), mantendo o fluxo humano ativo no restante.

Métricas de saúde: uptime >99.5%, tempo médio de resposta 15-25s, erros 422 <1%, fallback (banco local em vez de CRM) <5%. Métricas de negócio: taxa de resposta do lead >30%, conversas que chegam à oferta >20%, cliques no link >50% dos que receberam, vendas atribuídas via `?src=` >80% das vendas do canal. Tempo médio de resposta esperado: 8s (debounce) + 8s (delay) + 5-10s (geração) = ~20-25s. É normal.

## 25. Troubleshooting comum

- Mensagem não chega no Davi: automação ativa no CRM? filtro de canal correto? testar a URL direto com curl; Caddy e DNS ok?
- Recebe mas não responde: ver `pm2 logs`; `LLM_API_KEY` configurada? pode ser rate limit do provedor de LLM.
- Erro 422 ao enviar: "Contact has no Instagram id" = `type` errado pro contato, ajustar `channel_type`.
- Áudio não transcreve: Whisper instalado? URL pode ter expirado; formatos ogg/mp3/m4a/wav/opus.
- Mensagens duplicadas: debounce de 8s resolve a maioria; checar se o CRM dispara o webhook mais de uma vez ou se há 2 automações no mesmo gatilho.
- Link/preço repetido ou idioma errado: reforçar na `personality` ("NUNCA envie link que já está no histórico"; "NUNCA mencione preços"; "SEMPRE responda em português brasileiro").
- Erro 403 na API do CRM: token expirado ou sem permissão; gerar novo token e atualizar o config.
- Canal errado: um agente por canal; nunca o mesmo agente pra dois canais.

## 26. Checklist final de deploy

Contas e acessos: GitHub (repo privado + token), hospedagem de páginas (logada com GitHub), provedor de DNS (domínio + nameservers), sua VPS (IP + SSH), provedor de LLM do SDR (crédito + API key), seu CRM (conta + token + ID da conta + Instagram conectado), sua plataforma de checkout/pagamento (produto + checkout).

Servidor: sistema atualizado; Python 3.10+; Node 18+; PM2 com `pm2 startup`; PostgreSQL rodando; Caddy rodando; `LLM_API_KEY` no `~/.bashrc`; Whisper instalado.

Banco: usuário/banco criados; 5 tabelas criadas; agente em `sdr_agents` (status active); dossiê em `sdr_agent_files`.

Agente: `template.py` no servidor; `config-N.json` correto; `get_db()` com credenciais certas; PM2 online; `pm2 save`; health check ok.

Rede/SSL: DNS tipo A pro webhook (proxy ON); Caddy configurado e recarregado; HTTPS funcionando.

CRM: Instagram conectado; automação de "mensagem nova do lead" criada; filtro de canal; ação de webhook com a URL certa; body JSON correto; automação ativada.

Páginas de venda: no GitHub; publicadas na sua hospedagem de páginas; domínio custom; DNS apontando pra hospedagem; JS de `?src=` funcionando.

Vendas: links com `?src=davi`; webhook da plataforma de checkout/pagamento apontando pro endpoint; eventos corretos.

Teste end-to-end: mandar DM de outro perfil; ver nos logs que chegou; ver que o Davi gerou resposta; ver a resposta no Instagram; clicar no link e confirmar `?src=`; compra teste cair em `sdr_agent_sales`.

Pós-deploy: backup do banco via cron; checar `pm2 status` diário; ajustar a personalidade pelas primeiras conversas reais; bateria de 20 cenários aprovada (18+); ramp-up em andamento.

---

**Fim do documento.**
