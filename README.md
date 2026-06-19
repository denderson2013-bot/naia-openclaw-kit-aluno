# Naia OpenClaw — kit do aluno

Kit oficial pra instalar uma **Naia OpenClaw** na sua VPS: uma agente de IA com a personalidade da Naia (orquestradora), rodando 24/7 no seu Telegram, com o **GPT Codex** (sua assinatura ChatGPT Plus, via OAuth) como cérebro.

Ela vem com um time treinado:
- **Naia** — a orquestradora. Conversa com você, entende, planeja, delega pro time e te entrega pronto. Fica 100% disponível.
- **Juliana** — executora. Faz o trabalho pesado (sites, landing pages, carrosséis, organização) pra Naia ficar livre.
- **Paulo** — desenvolvedor. Código, sites e automações, com método de 3 fases.
- **Davi (SDR)** — vendedor. Atende seus leads no Instagram e WhatsApp via CRM (webhook + API), método SPIN.

E 4 skills: conversação-orquestradora, código em 3 fases, criação de landing pages e criação de carrosséis.

## Como instalar
Você não precisa saber programar. Quem instala é o **Claude Code** ou o **Codex** rodando no seu PC:
1. Tenha a VPS (Ubuntu), o bot do Telegram (criado no @BotFather) e a assinatura ChatGPT Plus.
2. Abra o Claude Code **ou** o Codex.
3. Cole o prompt:
   - Claude Code → `prompt-claude.txt`
   - Codex → `prompt-codex.txt`
4. Responda o que ele perguntar. Ele instala tudo e a Naia acorda no seu Telegram.

O roteiro técnico completo que o instalador segue está em [`INSTALL.md`](INSTALL.md).

## Estrutura
- `workspace/` — persona da Naia orquestradora (SOUL, AGENTS, IDENTITY, USER)
- `subagents/` — Juliana, Paulo, Davi (SDR)
- `skills/` — as 4 skills
- `INSTALL.md` — roteiro de instalação (OpenClaw + Codex OAuth + Telegram + kit)
- `prompt-claude.txt` / `prompt-codex.txt` — o que você cola no instalador

Instala o **OpenClaw oficial** (https://github.com/openclaw/openclaw) e aplica este kit por cima. Feito por Denderson.ai.
