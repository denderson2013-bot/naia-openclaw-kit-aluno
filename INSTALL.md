# INSTALL.md — Instalação da Naia OpenClaw (kit do aluno)

> Agente (Claude Code ou Codex) que está executando: este arquivo é pra VOCÊ seguir, passo a passo.
> Objetivo: deixar uma **Naia OpenClaw** rodando 24/7 na VPS Ubuntu do aluno, no Telegram dele, com Codex (OpenAI) como cérebro, com a personalidade da Naia + 3 subagentes (Juliana, Paulo, Davi/SDR) + 4 skills.

## PERSISTÊNCIA (LER PRIMEIRO)
Se um comando der erro, NÃO pare: diagnostique com `openclaw config validate` e `openclaw doctor`, corrija e continue. Só considere concluído quando a Naia responder de verdade no Telegram. Não invente sintaxe: se um comando do OpenClaw divergir, rode o `--help` dele (`openclaw agents --help`, `openclaw skills --help`) e adapte. Testado com OpenClaw 2026.6.5.

## REGRA DE LLM
Esta Naia roda com **GPT Codex via OAuth** (assinatura ChatGPT Plus do aluno, sem API key). NÃO peça API key de LLM.

---

## ETAPA 0 — Coletar dados (uma pergunta por vez)
| Variável | Pergunta | Ex |
|---|---|---|
| `VPS_IP` | "IP público da sua VPS?" | 72.60.10.20 |
| `VPS_USER` | "Usuário (geralmente root)?" | root |
| `VPS_PASS` | "Senha da VPS?" (nunca logar) | — |
| `OWNER_NAME` | "Seu nome (vai ser a dona da Naia)?" | João |
| `TELEGRAM_BOT_TOKEN` | "Token do bot (do @BotFather)?" | 123:AAH... |
| `TELEGRAM_USER_ID` | "Seu ID no Telegram (do @userinfobot)?" | 123456789 |
| `TELEGRAM_BOT_USERNAME` | "Username do bot (sem @)?" | minha_naia_bot |

No modo remoto (você roda no PC do aluno), use `sshpass` pra SSH:
```bash
ssh_run(){ sshpass -p "$VPS_PASS" ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null "$VPS_USER@$VPS_IP" "$@"; }
ssh_run_tty(){ sshpass -p "$VPS_PASS" ssh -tt -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null "$VPS_USER@$VPS_IP" "$@"; }
ssh_run "uname -a && head -3 /etc/os-release"
```
Daqui em diante, cada bloco `bash` roda na VPS (via `ssh_run`, ou `ssh_run_tty` no login OAuth do Codex).

---

## ETAPA 1 — Bootstrap (Node + OpenClaw)
Instala Node 22, Python, ffmpeg, git e o OpenClaw CLI oficial:
```bash
curl -fsSL https://raw.githubusercontent.com/denderson2013-bot/agente-openclaw-telegram-setup-alunos-denderson/main/bootstrap.sh | bash
```
Valida:
```bash
node --version        # v22+
openclaw --version    # 2026.6.x
```
> Se `openclaw: command not found`: `export PATH="$(npm config get prefix)/bin:$PATH"` ou rerode o bootstrap.

---

## ETAPA 2 — Baixar o kit da Naia (repo público)
```bash
rm -rf /opt/naia-kit
git clone --depth 1 https://github.com/denderson2013-bot/naia-openclaw-kit-aluno.git /opt/naia-kit
ls /opt/naia-kit   # workspace/ subagents/ skills/
```

---

## ETAPA 3 — Config base do OpenClaw (Telegram + gateway)
> NUNCA escreva o openclaw.json na mão. Use `openclaw config patch` (valida na escrita).
```bash
mkdir -p /root/.openclaw && chmod 700 /root/.openclaw
cat > /tmp/oc-tg.json <<JSON
{ "channels": { "telegram": {
  "enabled": true, "botToken": "$TELEGRAM_BOT_TOKEN",
  "dmPolicy": "allowlist", "groupPolicy": "allowlist",
  "allowFrom": ["$TELEGRAM_USER_ID"] } } }
JSON
openclaw config patch --file /tmp/oc-tg.json && rm -f /tmp/oc-tg.json
openclaw config set gateway.mode local      # OBRIGATÓRIO (sem isso o gateway não sobe)
openclaw config validate                    # "Config valid"
```

---

## ETAPA 4 — Autenticar o Codex (OAuth, sem API key)
VPS é headless → use device-code:
```bash
openclaw models auth login --provider openai --device-code
```
O CLI imprime uma **URL** (`https://auth.openai.com/codex/device`) e um **Code**. Capture os dois e mande pro aluno:
> "Abre `https://auth.openai.com/codex/device` no navegador do seu PC já logado no ChatGPT Plus, digita o código **<CODE>**, autoriza. Não precisa colar nada de volta — o CLI detecta sozinho."

No modo remoto, rode esse comando com `ssh_run_tty` e leia o stdout pra extrair URL e Code. Espere `OpenAI device code complete`, depois:
```bash
openclaw models set openai/gpt-5.5
openclaw config validate
openclaw models status --probe              # openai autenticado e acessível
```

---

## ETAPA 5 — Aplicar a persona da Naia (workspace principal)
O workspace padrão do OpenClaw é `/root/.openclaw/workspace`. Substitua a persona genérica pela da Naia e preencha o nome da dona:
```bash
WS=/root/.openclaw/workspace
mkdir -p "$WS"
cp /opt/naia-kit/workspace/SOUL.md      "$WS/SOUL.md"
cp /opt/naia-kit/workspace/AGENTS.md    "$WS/AGENTS.md"
cp /opt/naia-kit/workspace/IDENTITY.md  "$WS/IDENTITY.md"
cp /opt/naia-kit/workspace/USER.md      "$WS/USER.md"
sed -i "s/{{OWNER_NAME}}/$OWNER_NAME/g" "$WS/USER.md" "$WS/IDENTITY.md" "$WS/AGENTS.md"
# remove o BOOTSTRAP.md genérico se existir (a persona já vem pronta)
rm -f "$WS/BOOTSTRAP.md"
openclaw agents set-identity --name "Naia" 2>/dev/null || true
```

---

## ETAPA 6 — Criar os 3 subagentes (Juliana, Paulo, Davi/SDR)
Cada subagente é um agente isolado do OpenClaw com workspace próprio. Para cada um: cria o workspace, copia a persona, e registra com `openclaw agents add ... --non-interactive`.
```bash
for SUB in juliana paulo sdr; do
  WSP="/root/.openclaw/ws-$SUB"
  mkdir -p "$WSP"
  cp /opt/naia-kit/subagents/$SUB/*.md "$WSP"/
  sed -i "s/{{OWNER_NAME}}/$OWNER_NAME/g" "$WSP"/*.md 2>/dev/null || true
  openclaw agents add "$SUB" --workspace "$WSP" --model openai/gpt-5.5 --non-interactive
done
openclaw agents list      # deve listar: main (Naia) + juliana + paulo + sdr
```
> Se `openclaw agents add` pedir flag diferente nesta versão, rode `openclaw agents add --help` e ajuste (workspace + model + non-interactive). NÃO invente.

---

## ETAPA 7 — Instalar as 4 skills
Instala as 4 skills do kit (de diretório local) no diretório compartilhado, pra todos os agentes:
```bash
for SK in conversacao-orquestradora codigo-3-fases landing-pages carrossel; do
  openclaw skills install "/opt/naia-kit/skills/$SK" --global --force
done
openclaw skills list      # deve mostrar as 4
openclaw skills check     # todas "ready"
```
Mapa de uso (a persona de cada agente já referencia): Naia→conversacao-orquestradora; Juliana→landing-pages, carrossel, codigo-3-fases; Paulo→codigo-3-fases, landing-pages. (O SDR/Davi usa a knowledge própria do workspace dele.)

---

## ETAPA 8 — (Opcional) Áudio
Se o aluno deu chave OpenAI (Whisper) e/ou ElevenLabs (voz):
```bash
echo "OPENAI_API_KEY=$OPENAI_API_KEY" >> /root/.openclaw/.env && chmod 600 /root/.openclaw/.env
```
(TTS ElevenLabs: ver ETAPA 4 do SETUP do OpenClaw oficial.)

---

## ETAPA 9 — Subir o gateway (systemd 24/7)
```bash
curl -fsSL https://raw.githubusercontent.com/denderson2013-bot/agente-openclaw-telegram-setup-alunos-denderson/main/systemd/openclaw-gateway.service \
  -o /etc/systemd/system/openclaw-gateway.service
sed -i "s/AGENTE_NAME_PLACEHOLDER/Naia/" /etc/systemd/system/openclaw-gateway.service
OPENCLAW_BIN="$(command -v openclaw || echo /usr/local/bin/openclaw)"
sed -i "s#__OPENCLAW_BIN__#$OPENCLAW_BIN#" /etc/systemd/system/openclaw-gateway.service
systemctl daemon-reload && systemctl enable --now openclaw-gateway
systemctl is-active openclaw-gateway       # active
```

---

## ETAPA 10 — Validar ponta a ponta
```bash
openclaw doctor
openclaw agents list
openclaw models status --probe
```
Peça pro aluno mandar `/start` (ou "oi") pro `@$TELEGRAM_BOT_USERNAME`. Confirme que a Naia respondeu com a personalidade dela. Se não responder:
```bash
journalctl -u openclaw-gateway -n 50 --no-pager
```
- `missing gateway.mode` → ETAPA 3 (`openclaw config set gateway.mode local`).
- `no model` → refaz ETAPA 4 (OAuth Codex).
- `user not in allowFrom` → confere o `TELEGRAM_USER_ID` em `channels.telegram.allowFrom`.

Pronto: Naia OpenClaw no ar, com Juliana, Paulo e Davi (SDR) + as 4 skills, conversando no Telegram do aluno.
