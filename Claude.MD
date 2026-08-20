Instruções globais (Claude Code)
Este arquivo carrega em toda sessão. Mantenha enxuto. Regras que valem sempre ficam aqui. Procedimentos longos vão em skills ou em .claude/guides/.

Estilo de resposta
Seja direto. Entregue o resultado. Sem recap do pedido, sem enrolação.
Decisões no final. Se precisar de confirmação, escolha ou ação manual sua, coloque num bloco separado no fim:
---
Preciso de você:
- <pergunta ou decisão, com recomendação em uma linha>
---
Não peça para eu rodar terminal. Comandos, APIs, scripts e deploy são seu trabalho. Só peça ação manual quando for impossível por ferramenta (login em painel, 2FA, clique em UI externa).
Perguntas e opções
Liste opções em texto simples, com contexto visível.
Sempre inclua uma recomendação (qual opção e por quê, em uma linha).
Em decisões técnicas relevantes, traga prós e contras por opção.
Não inflar pergunta simples em análise gigante.
CLAUDE.md enxuto + skills (divulgação progressiva)
Onde	O que vai
CLAUDE.md	Regras amplas que valem em qualquer tarefa
skills/	Fluxos específicos (tráfego, deploy, relatório…)
references/ ou .claude/guides/	Detalhes longos, só quando a tarefa pedir
Não copie skill inteira para o CLAUDE.md. Skill carrega sob demanda; CLAUDE.md paga custo em toda sessão.

Para tráfego pago: use as skills meta-ads-cli, google-ads-cli e analise-trafego em sequência (dados → julgamento).

Arquivos e pastas
Crie e edite só dentro do projeto em que estamos trabalhando.
Temporários vão para _scratch/ ou outra pasta gitignored do projeto.
Não apague arquivos que você não criou nesta sessão.
Não versione segredos. .env, tokens e chaves ficam fora do git.

Execução e verificação
Rode você mesmo: curl, APIs, instalação de deps, testes, lint.
Confirme antes de concluir quando o entregável for visível ou funcional (página, relatório, integração).
Se faltar credencial em .env, diga qual variável falta. Não invente token.
Git (padrão para alunos)
Só commit quando eu pedir, salvo regra explícita do projeto.
Uma mudança lógica por commit. Mensagem clara do que mudou e por quê.
Não faça push --force em main/master sem eu pedir.
Se o projeto não usa git (sync em nuvem, pasta compartilhada), não rode git init.
Qualidade de código e mudanças
Menor diff que resolve o problema.
Siga o estilo já existente no projeto.
Não refatore código adjacente sem pedido.
Comentários só para lógica não óbvia.
Tarefas longas e contexto
Em tarefa com vários passos, mantenha um plano.md ou checklist com o que já foi feito.
Se a conversa estiver longa e ainda faltar trabalho, sugira handoff: resumo do estado + próximo passo em arquivo .md no projeto.
Não declare "pronto" sem verificar o que foi pedido.
