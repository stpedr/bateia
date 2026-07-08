# bateia — instruções do projeto

Blueprint para transformar ideias validadas em micro-apps lançados e monetizados. O método completo vive em `.claude/skills/ideia-app/` (playbook + `videos.json` com as fontes).

## Fluxo de trabalho

1. Toda ideia nova entra por `/ideia-app <ideia>` — não comece a codar sem passar por ela.
2. **80% planejamento, 20% execução**: use `/blueprint` para estruturar planos de construção multi-etapa. Planeje com Opus (esforço máximo), execute com Sonnet.
3. Valide mercado com `/market-research` antes de investir tempo em build.
4. Design: inspire-se em brand guidelines públicas (Duolingo, Revolut) — **inspirar, não copiar** (risco judicial). Use `/brand-discovery` para posicionamento.
5. Construa em blocos, aprovando cada um.

## Convenções de build

- Stack padrão de MVP: web app / PWA; banco só se indispensável (Supabase); deploy Vercel/Netlify; AWS só quando crescer.
- MVP tem no máximo 3 telas.
- Pagamentos: assinatura recorrente Pix (Cakto; alternativas Stripe/Mercado Pago) com webhooks. Sempre testar comprando o próprio app antes de lançar.
- Backend leve: confirmação de e-mail + senha + recuperação de senha.

## Segurança (obrigatório antes de qualquer deploy)

- Rode `/security-review` no código: nada de segredo hardcoded, nada de `.env` commitado, chaves de API fora do front.
- Rotas de admin/backoffice não podem ser enumeráveis (`robots.txt`, sitemap).
- `.env*` está no `.gitignore` — mantenha assim.
- Contexto legal: LGPD responsabiliza o dono do app por vazamento (notificação obrigatória + multa de 5% do faturamento anual, à vista).

## Skills vendorizadas do ECC

`market-research`, `blueprint`, `brand-discovery`, `security-review`, `security-scan` vêm do [ECC](https://github.com/affaan-m/ECC) (MIT — licença em `.claude/skills/ECC-LICENSE`). Ao atualizá-las, puxe do repo oficial apenas; não edite localmente sem anotar o desvio neste arquivo.
