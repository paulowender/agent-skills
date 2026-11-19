---
name: reimagine
description: Use quando o usuário pedir para reimaginar, repaginar, redesenhar, refazer ou modernizar uma tela, página ou módulo existente de qualquer sistema, mantendo todas as funcionalidades atuais. Cobre inventário do que existe, protótipos HTML clicáveis, spec, plano, implementação com subagentes, teste ponta a ponta no navegador e entrega em PRs.
---

# Reimagine — Repaginar uma tela/módulo

Refazer a experiência de uma tela do zero se for preciso, **mantendo todas as funcionalidades que existem hoje**. Se o usuário ainda não disse qual é a tela, pergunte antes de qualquer coisa — é a única pergunta a fazer de cara.

## 0. Descobrir o contexto do projeto (antes de tudo)

Não presuma nada; levante e confirme com o usuário só o que não estiver documentado:

- **Repositórios envolvidos** e stack de cada um (front, backend, functions/workers, outros serviços). Leia `CLAUDE.md` / `AGENTS.md`, `README`, `docs/architecture*` e similares.
- **Biblioteca de UI e padrões visuais** já usados no app (design system, componentes base, lib de gráficos). Reaproveite; não traga lib nova sem necessidade.
- **Arquitetura alvo**: onde ficam leitura, escrita e regra de negócio. Se a tela hoje viola a arquitetura alvo (ex.: regra de negócio ou gravação no front), mova para a camada certa durante a repaginação — mesmo que dê mais trabalho.
- **Branches base** de cada repo e fluxo de PR. Uma branch por repo.
- **Ambientes**: o que pode ser publicado para teste (homolog/staging) e o que nunca (produção).
- **Quadro/nota de trabalho** do projeto, se existir: leia no início e atualize a cada etapa (card da tarefa, decisões, pendências, backlog).
- **Fuso horário** do servidor/cron e formato das datas gravadas; o navegador pode estar em outro fuso.

Regras fixas:
- Nunca faça merge, push em branch compartilhada ou deploy em produção sem pedido explícito.
- Nunca escreva valores de chaves/tokens em notas, logs, commits ou respostas.

## Como trabalhar (na ordem)

As skills citadas (`superpowers:*`, `dataviz`) são usadas se estiverem instaladas no agente (Claude Code, Codex etc.). Se alguma não existir, siga o mesmo processo descrito no passo, sem ela.

1. **Inventário do que existe.** Leia a tela atual inteira (rotas, componentes, estado, actions, functions e endpoints que ela usa). Peça acesso ao navegador do usuário (Chrome DevTools MCP ou Playwright) **logo no começo**, enquanto ele está por perto, e percorra a tela. Liste cada funcionalidade, regra e limitação real (ex.: "envio por tal canal não permite agendamento") — nada pode se perder. Identifique componentes compartilhados com outras telas.

2. **Entender a dor.** Pergunte (uma pergunta por vez) o que incomoda na tela hoje e o que o usuário mais faz nela. Use a skill `superpowers:brainstorming`.

3. **Alternativas visuais.** Proponha 2–3 direções e mostre como **protótipos HTML clicáveis** abertos no navegador do usuário, com dados realistas (inspire-se em dados reais de um ambiente de teste, nunca de produção sem permissão). Use como referência os padrões que já existem no app. Itere com o feedback até a aprovação de uma versão completa: lista + detalhe + estados vazio, carregando, erro e progresso.
   - Gráficos: use a skill `dataviz`, valide a paleta no fundo escuro e claro e mostre sempre uma tabela de quantidades junto do gráfico.

4. **Spec e plano.** Escreva o design aprovado em `docs/superpowers/specs/AAAA-MM-DD-<tela>-design.md` (no repo principal da tela) e o plano em `docs/superpowers/plans/` com a skill `superpowers:writing-plans`. Cubra todos os repos envolvidos, os contratos entre eles (endpoints, payloads, campos de documentos/tabelas) e os testes.

5. **Implementação.** Use `superpowers:subagent-driven-development`: um implementador por tarefa, revisão de spec + qualidade a cada tarefa, revisão final da branch inteira. Se o agente não tiver subagentes, execute as tarefas em sequência com a mesma revisão a cada uma. Isole lógica pura em módulos testáveis com o runner de testes que o projeto já usa; endpoints e jobs com seus testes. Registre cada decisão tomada sozinho no ledger como "Ruling: decisão — motivo — custo se errado".

6. **Teste de ponta a ponta de verdade.** Suba front e backend locais você mesmo (em background, acompanhando o log) e use os serviços publicados no ambiente de teste. Faça o fluxo completo no navegador como usuário: criar, editar, salvar, testar, agendar, cancelar, executar, ver resultados. Se a tela dispara algo real (e-mail, mensagem, cobrança), confirme a chegada e as métricas. Antes de dizer que algo funciona, mostre a evidência (tela, rede, log).

7. **Cuidados que costumam morder:**
   - CSS global e componentes compartilhados: mudança em componente usado por outras telas deve ser opt-in.
   - Evitar salvamento automático que gere log/escrita a cada tecla; preferir botão "Salvar" com aviso de alterações não salvas e confirmação ao sair.
   - Não apagar histórico (ex.: cancelado fica registrado com quem e quando).
   - Contar chamadas de rede ao abrir a tela; evitar N chamadas onde uma resolve e refetch desnecessário ao trocar de aba.
   - Datas: converter o fuso do servidor; nunca mostrar duração negativa ou "em poucos segundos" por relógio adiantado.
   - Mobile: colunas que crescem sem limite (agrupar, resumir, tooltip).
   - Acessibilidade básica: foco visível, rótulos, contraste.
   - Se o projeto tiver ferramentas de manutenção pós-edição (ex.: `graphify update .`, codegen, lint), rode depois de mexer no código.

8. **Entrega.** PRs com descrição clara, ordem de merge (normalmente backend → workers/functions → frontend), o que foi testado e o que ficou pendente. Atualize o quadro do projeto com pendências e backlog. Resumo final curto: o que mudou, o que testou, decisões tomadas sozinho, pendências.

## Autonomia

Depois que o usuário aprovar o protótipo e o plano, siga até o fim sem pedir confirmação a cada passo: decida, registre a decisão e continue. Pare só para: deploy em produção, merge/push em branch compartilhada, ação destrutiva ou envolvendo segredo, ou se o caminho ficar impossível de decidir sem o usuário.
