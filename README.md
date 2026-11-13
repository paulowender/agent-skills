# claude-skills

Skills para o [Claude Code](https://claude.com/claude-code), distribuídas como marketplace de plugins.

## Instalar

No Claude Code:

```
/plugin marketplace add paulowender/claude-skills
/plugin install repaginar-tela@paulowender-skills
```

Para atualizar depois: `/plugin marketplace update paulowender-skills`.

## Skills

### repaginar-tela

Repagina uma tela ou módulo existente de qualquer sistema, sem perder nenhuma funcionalidade. Fluxo:

1. Descobre o contexto do projeto (repos, stack, arquitetura, branches, ambientes) a partir do `CLAUDE.md` e da documentação.
2. Faz o inventário completo da tela atual, inclusive navegando nela.
3. Entende a dor do usuário.
4. Propõe 2–3 direções como protótipos HTML clicáveis.
5. Escreve spec e plano.
6. Implementa com subagentes e revisão por tarefa.
7. Testa ponta a ponta no navegador, com evidência.
8. Entrega em PRs com ordem de merge e pendências.

Uso: peça "repaginar a tela X" ou rode `/repaginar-tela`.

Funciona melhor com o plugin [superpowers](https://github.com/obra/superpowers) instalado (brainstorming, writing-plans, subagent-driven-development) e um MCP de navegador (Chrome DevTools ou Playwright).

Dica: coloque no `CLAUDE.md` do seu projeto as branches base, ambientes de teste, portas locais e onde fica o quadro de tarefas. A skill lê isso no passo 1.

## Licença

MIT
