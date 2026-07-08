# bateia

> A bateia é a peneira do garimpeiro: separa a pepita do cascalho.
> Este repositório faz o mesmo com ideias de app — peneira as validadas e as transforma em micro-apps lançados e monetizados, usando Claude Code.

**Blueprint base** para o ciclo completo: achar ideia validada → planejar → construir → publicar → vender → blindar.

## O método (resumo)

1. **Garimpar** — não inventar: encontrar apps já validados (apps similares muito baixados no Ocidente; rankings da China via Sensor Tower/Qimai) e filtrar por 3 critérios: simples de criar, fácil de replicar, faz sentido no Brasil.
2. **Planejar (80%)** — gerar o plano antes de codar. Opus para planejar, Sonnet para executar. Sem plano vira AI slop.
3. **Construir (20%)** — em blocos, dialogando com o Claude Code, com uma skill de design (brand guidelines públicas, estilo Duolingo — inspirar, não copiar).
4. **Publicar** — web (Vercel/Netlify) → PWA → lojas. Supabase como banco de MVP.
5. **Vender** — assinatura recorrente via Pix (ex.: R$ 9,90/mês na Cakto), webhooks configurados, testes reais de compra.
6. **Blindar** — nada de segredo hardcoded, `.env` fora do git, `/admin` invisível, LGPD respeitada.

O detalhamento vive na skill [`ideia-app`](.claude/skills/ideia-app/SKILL.md) e no seu [`videos.json`](.claude/skills/ideia-app/videos.json) (playbook extraído de transcrições reais).

## Como usar (passo a passo)

```bash
# 1. Clone e entre na pasta
git clone https://github.com/stpedr/bateia.git meu-app
cd meu-app

# 2. Abra o Claude Code aqui dentro
claude
```

```
# 3. Dentro do Claude Code, rode a skill orquestradora
/ideia-app app que lembra idosos de tomar remédio e avisa a família se não tomarem
```

Ele devolve o relatório completo: ideia em 1 frase, veredito de mercado, nota (x/10), MVP + prompt de construção, caminho de publicação, plano de receita e checklist de segurança — terminando com **a única ação a fazer hoje**.

```
# 4. Aprovado o plano? Construa em blocos, dentro da mesma pasta
> construa o bloco 1 do plano

# 5. Antes de qualquer deploy (obrigatório)
/security-review
```

> Dica: clone uma cópia **por app** (`meu-app-1`, `meu-app-2`…). O repo é o molde; cada app nasce de uma cópia dele.

## Como funciona por trás (os basics)

Não tem mágica — são só **3 mecanismos do Claude Code** que este repo explora:

**1. `CLAUDE.md` = memória permanente do projeto.**
Toda vez que você abre o Claude Code numa pasta, ele lê o `CLAUDE.md` dela antes de qualquer coisa. É assim que as convenções (MVP de 3 telas, Vercel/Supabase, Pix, "nunca deployar sem security-review") valem em *toda* sessão sem você repetir nada. Mudou de opinião sobre uma convenção? Edite o `CLAUDE.md` e pronto.

**2. `.claude/skills/*/SKILL.md` = comandos ensináveis.**
Cada subpasta de `.claude/skills/` com um `SKILL.md` vira automaticamente um slash command (`/ideia-app`, `/blueprint`…). O arquivo é só Markdown com um cabeçalho:

```markdown
---
name: ideia-app
description: quando usar esta skill (o Claude lê isso para decidir ativá-la sozinho)
---
Instruções que o Claude segue quando a skill roda.
O que você digitar depois do comando entra em $ARGUMENTS.
```

Ou seja: **uma skill é um prompt versionado no git**. Dá para editar, commitar, compartilhar — e uma skill pode mandar o Claude invocar outra (a `ideia-app` chama `market-research` na validação e `security-review` na blindagem). É por isso que copiar a pasta de uma skill de outro repo (como fizemos com o ECC) basta para "instalá-la".

**3. Arquivos de dados ao lado da skill = conhecimento consultável.**
O `videos.json` fica junto do `SKILL.md` e guarda o playbook extraído dos vídeos (ferramentas, preços, critérios). A skill manda o Claude lê-lo quando precisa dos detalhes. Quer evoluir o método? Edite o JSON — a skill nem precisa mudar.

O ciclo completo, portanto:

```
você digita /ideia-app <ideia>
   └─ Claude lê SKILL.md (o roteiro) + videos.json (o conhecimento)
        ├─ Etapa 2 → invoca market-research (skill ECC)
        ├─ Etapa 4 → invoca blueprint (skill ECC)
        └─ Etapa 7 → invoca security-review (skill ECC)
   └─ sai o relatório com o plano do app
```

## Integração ECC — os 3 passos

Este blueprint integra o [ECC](https://github.com/affaan-m/ECC) (MIT) em três níveis:

### Passo 1 — Skills curadas (já incluído ✅)

Cinco skills do ECC vendorizadas em [`.claude/skills/`](.claude/skills/), disponíveis como slash commands:

| Skill | Papel no ciclo |
|---|---|
| `/market-research` | Validar a ideia e mapear concorrentes (etapa Garimpar) |
| `/blueprint` | Estruturar o plano de construção (etapa Planejar) |
| `/brand-discovery` | Definir marca/posicionamento antes do design |
| `/security-review` | Blindar o **código do app** antes de lançar (secrets, auth, pagamentos) — invocada pela `ideia-app` |
| `/security-scan` | Blindar a **config do agente** (`.claude/`: hooks, MCP, settings) via AgentShield |

Licença original preservada em [`.claude/skills/ECC-LICENSE`](.claude/skills/ECC-LICENSE).

### Passo 2 — Plugin ECC completo (opcional)

Para ter as ~160 skills do ECC (custa contexto — use se precisar):

```
/plugin marketplace add affaan-m/ECC
/plugin install ecc@ecc
```

Instale **apenas dos canais oficiais** (repo `affaan-m/ECC`, npm `ecc-universal` / `ecc-agentshield`) — existem mirrors não oficiais com malware.

### Passo 3 — GitHub App (opcional, para produção)

Instale o [ECC Tools GitHub App](https://github.com/apps/ecc-tools) neste repositório para auditoria automática de PRs (tier gratuito disponível). Requer um clique do dono do repo na interface do GitHub.

## Estrutura

```
bateia/
├── README.md            ← você está aqui
├── CLAUDE.md            ← instruções que o Claude Code lê em toda sessão
└── .claude/skills/
    ├── ideia-app/        ← orquestrador do método (playbook dos vídeos)
    ├── market-research/  ← ECC
    ├── blueprint/        ← ECC
    ├── brand-discovery/  ← ECC
    ├── security-review/  ← ECC
    ├── security-scan/    ← ECC
    └── ECC-LICENSE
```

## Regra de ouro

**Nunca lance sem rodar `/security-review`.** O dono do app responde pelo vazamento (LGPD: notificação obrigatória aos clientes + multa de 5% do faturamento anual, à vista).

## Licenças

- Este repositório: MIT ([LICENSE](LICENSE)).
- Skills `market-research`, `blueprint`, `brand-discovery`, `security-review`, `security-scan`: © Affaan Mustafa, MIT, vendorizadas do [ECC](https://github.com/affaan-m/ECC).
