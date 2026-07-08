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

## Como usar

Abra esta pasta com o Claude Code e rode:

```
/ideia-app <sua ideia de app em 1-3 frases>
```

Ele devolve o relatório completo: ideia em 1 frase, veredito de mercado, nota, MVP + prompt de construção, caminho de publicação, plano de receita e checklist de segurança.

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
