# agent-skills

Skills para o [Claude Code](https://claude.com/claude-code), [Codex](https://github.com/openai/codex) e [Antigravity](https://antigravity.google), distribuídas como marketplace de plugins.

## Instalar

### Claude Code

```
/plugin marketplace add paulowender/agent-skills
/plugin install reimagine@paulowender-skills
```

Para atualizar depois: `/plugin marketplace update paulowender-skills`.

### Codex

```
codex plugin marketplace add paulowender/agent-skills
codex plugin add reimagine@paulowender-skills
```

Para a etapa de implementação com subagentes, habilite no `~/.codex/config.toml`:

```toml
[features]
multi_agent = true
```

### Antigravity (`agy`)

```sh
agy plugin link paulowender-skills https://github.com/paulowender/agent-skills
agy plugin install reimagine@paulowender-skills
```

Ou a partir de um clone local:

```sh
agy plugin install ./plugins/reimagine
```

### Outros agentes

Copie a pasta `plugins/<plugin>/skills/<skill>` para a pasta de skills do seu agente. O `SKILL.md` segue o formato aberto de Agent Skills.

## Skills

### reimagine

Reimagina e repagina uma tela ou módulo existente de qualquer sistema, sem perder nenhuma funcionalidade. Fluxo:

1. Descobre o contexto do projeto (repos, stack, arquitetura, branches, ambientes) a partir do `CLAUDE.md` / `AGENTS.md` e da documentação.
2. Faz o inventário completo da tela atual, inclusive navegando nela.
3. Entende a dor do usuário.
4. Propõe 2–3 direções como protótipos HTML clicáveis.
5. Escreve spec e plano.
6. Implementa com subagentes e revisão por tarefa.
7. Testa ponta a ponta no navegador, com evidência.
8. Entrega em PRs com ordem de merge e pendências.

Uso: peça "reimaginar/repaginar a tela X". No Claude Code também dá para rodar `/reimagine`; no Codex, `$reimagine`.

Funciona melhor com o plugin [superpowers](https://github.com/obra/superpowers) instalado (brainstorming, writing-plans, subagent-driven-development) e um MCP de navegador (Chrome DevTools ou Playwright).

Dica: coloque no `CLAUDE.md` (Claude Code) ou `AGENTS.md` (Codex/Antigravity) do seu projeto as branches base, ambientes de teste, portas locais e onde fica o quadro de tarefas. A skill lê isso no passo 1.

## Licença

MIT
