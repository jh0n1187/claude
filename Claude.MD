# CLAUDE.md — Instruções Globais

Este arquivo é carregado em **toda sessão do Claude Code**.

Mantenha-o enxuto. Aqui ficam apenas regras e comportamentos que valem para qualquer projeto.

Contexto, arquitetura, regras de negócio e decisões específicas pertencem ao projeto local.

---

## 1. Estilo de resposta

Seja direto.

Entregue o resultado sem recapitular o pedido e sem explicações desnecessárias.

Não transforme perguntas simples em análises extensas.

Quando houver decisão, confirmação ou ação manual necessária do usuário, coloque somente no final:

**---**

Preciso de você:

* <pergunta ou decisão — inclua sua recomendação em uma linha>

**---**

Se não precisar de intervenção do usuário, não crie esse bloco.

---

## 2. Autonomia de execução

Execute você mesmo tudo que puder executar.

Isso inclui:

* comandos de terminal;
* instalação de dependências;
* scripts;
* `curl`;
* chamadas de API;
* migrations;
* testes;
* lint;
* build;
* deploy;
* inspeção de logs;
* validações técnicas.

**Não peça para o usuário executar comandos no terminal.**

Só solicite ação manual quando realmente não puder executá-la por ferramenta, por exemplo:

* login em painel externo;
* autenticação 2FA;
* CAPTCHA;
* autorização humana;
* clique obrigatório em UI externa.

Se faltar uma credencial, informe exatamente qual variável ou segredo está faltando.

Nunca invente tokens, credenciais ou valores de configuração.

---

## 3. Perguntas, opções e decisões

Quando houver múltiplas opções:

* apresente-as em texto simples;
* deixe claro o contexto de cada opção;
* em decisões técnicas relevantes, informe prós e contras;
* sempre dê uma recomendação.

Formato esperado:

**Recomendação:** <opção> — <motivo em uma linha>.

Evite devolver decisões triviais ao usuário quando houver uma escolha técnica segura e reversível que você possa tomar sozinho.

---

## 4. Organização do conhecimento

Use divulgação progressiva de contexto.

### Global

O `CLAUDE.md` global contém somente regras que valem para todos os projetos.

Não coloque nele:

* contexto de produto;
* regras de negócio;
* arquitetura de um projeto;
* stack específica;
* decisões técnicas locais;
* histórico de projeto.

### Projeto

Cada projeto deve manter seu próprio contexto local.

Estrutura recomendada:

```text
project/
├── CLAUDE.md
├── business-context.md
├── technical-context.md
└── .claude/
    └── guides/
```

O `CLAUDE.md` local funciona como **ponto de entrada e índice do projeto**.

Ele deve referenciar explicitamente os arquivos locais de contexto.

Exemplo:

```markdown
## Contexto

Consulte conforme necessário:

- Negócio e produto: `business-context.md`
- Arquitetura e implementação: `technical-context.md`

Carregue apenas o contexto relevante para a tarefa atual.
```

Não duplique nos arquivos globais informações existentes no contexto local.

---

## 5. Separação de contexto local

Todo projeto deve separar conhecimento persistente em pelo menos:

### Contexto de negócio

`business-context.md`

Responsável pelo conhecimento de domínio e produto, como:

* objetivo do produto;
* problema que resolve;
* conceitos de domínio;
* usuários;
* jornadas;
* funcionalidades;
* regras de negócio;
* restrições funcionais;
* decisões de produto;
* comportamentos esperados.

### Contexto técnico

`technical-context.md`

Responsável pelo conhecimento de implementação, como:

* arquitetura;
* stack;
* estrutura do projeto;
* integrações;
* APIs;
* bancos;
* infraestrutura;
* autenticação;
* padrões;
* dependências;
* configurações;
* convenções;
* decisões arquiteturais;
* restrições técnicas;
* comportamentos não óbvios;
* problemas recorrentes e soluções.

Esses arquivos pertencem **exclusivamente ao projeto local**.

O global apenas estabelece a convenção.

---

## 6. CLAUDE.md local

Todo projeto deve possuir um `CLAUDE.md` local.

Ele deve ser curto e servir como:

* ponto de entrada;
* índice do contexto;
* conjunto de regras operacionais específicas;
* referência para guides e skills relevantes.

Não transforme o `CLAUDE.md` local em documentação extensa.

Quando uma informação for de negócio, coloque no contexto de negócio.

Quando for de implementação ou arquitetura, coloque no contexto técnico.

Quando for uma documentação especializada e extensa, prefira `.claude/guides/`, `references/` ou skills.

---

## 7. Retroalimentação contínua

O contexto local de cada projeto é uma **memória operacional viva**.

Durante o trabalho, identifique conhecimento novo que possa evitar retrabalho em sessões futuras.

Ao final de uma sessão de desenvolvimento, revise automaticamente se o contexto persistente do projeto precisa ser atualizado.

Considere uma sessão encerrada quando:

1. código, configuração, infraestrutura ou documentação relevante foi alterada; **e**
2. o usuário indicar encerramento, por exemplo: `"tá bom"`, `"perfeito"`, `"é isso"`, `"obrigado"`, `"pode parar"`, despedida ou conclusão equivalente.

Quando isso ocorrer, faça a retroalimentação **automaticamente e sem perguntar**.

---

## 8. Destino da retroalimentação

Classifique o conhecimento antes de persistir.

```text
Regra/decisão de negócio
        ↓
business-context.md

Arquitetura/implementação/lição técnica
        ↓
technical-context.md

Regra sobre como trabalhar no projeto
        ↓
CLAUDE.md local

Documentação especializada/extensa
        ↓
.claude/guides/, references/ ou skills/
```

Atualize sempre os arquivos **do projeto atual**.

Nunca envie contexto específico de um projeto para o `CLAUDE.md` global.

---

## 9. Captura de desafios recorrentes

Ao encerrar uma sessão, identifique desafios encontrados.

Um desafio deve ser persistido somente quando:

1. causou perda real de tempo; **e**
2. existe possibilidade plausível de acontecer novamente.

Exemplos:

* bug não óbvio;
* mensagem de erro enganosa;
* particularidade de configuração;
* comportamento inesperado de biblioteca;
* incompatibilidade de versão;
* problema de ambiente;
* workflow quebrado;
* solução que exigiu investigação;
* armadilha de API, SDK ou ferramenta.

Não registre:

* typo;
* erro trivial;
* ajuste óbvio;
* problema circunstancial;
* tentativa intermediária sem valor futuro.

Quando houver um desafio recorrente, registre automaticamente, sem perguntar.

Normalmente essas lições pertencem ao `technical-context.md` local.

Prefira:

**sintoma → causa → solução/prevenção**

Se não houver aprendizado reutilizável, não registre nada.

---

## 10. Higiene da memória

Antes de persistir conhecimento:

1. verifique se já está documentado;
2. atualize em vez de duplicar;
3. substitua informação obsoleta;
4. não registre detalhes temporários;
5. não registre tentativas descartadas;
6. preserve apenas conhecimento útil para sessões futuras.

Registre o **estado atual válido**, não o histórico da sessão.

Prefira:

> Autenticação utiliza Keycloak através do BFF.

Em vez de:

> Nesta sessão alteramos a autenticação para utilizar Keycloak.

O objetivo não é acumular informação indefinidamente.

O objetivo é fazer com que **cada nova sessão comece melhor informada que a anterior**.

---

## 11. Arquivos e diretórios

Crie e edite arquivos somente dentro do projeto atual.

Arquivos temporários devem ficar em:

* `_scratch/`; ou
* outro diretório ignorado pelo Git.

Não apague arquivos que você não criou nesta sessão, salvo quando a tarefa exigir explicitamente essa remoção.

Não versione segredos.

Nunca coloque no Git:

* `.env`;
* tokens;
* API keys;
* senhas;
* credenciais;
* certificados privados.

---

## 12. Execução e verificação

Não considere uma tarefa concluída apenas porque o código foi escrito.

Sempre que aplicável:

1. implemente;
2. execute;
3. teste;
4. valide;
5. corrija;
6. valide novamente.

Para entregáveis visíveis ou funcionais, confirme o funcionamento antes de declarar conclusão.

**Nunca declare "pronto" sem verificar o que foi pedido.**

---

## 13. Git

Por padrão, faça commit somente quando o usuário pedir, salvo regra explícita do projeto.

Quando fizer commits:

* uma mudança lógica por commit;
* mensagem clara;
* descreva o que mudou;
* quando relevante, deixe claro o motivo.

Nunca execute `push --force` em `main` ou `master` sem solicitação explícita.

Se o projeto não utiliza Git, não execute `git init` automaticamente.

---

## 14. Qualidade de código

Prefira o menor diff que resolva corretamente o problema.

Sempre:

* siga o estilo existente;
* preserve padrões arquiteturais;
* evite alterações fora do escopo;
* não refatore código adjacente sem necessidade;
* não introduza abstrações prematuras;
* não altere contratos existentes sem necessidade.

Comentários devem explicar **por que** algo não óbvio existe.

Evite comentários que apenas repetem o código.

---

## 15. Tarefas longas e continuidade

Para tarefas com vários passos, mantenha quando necessário um `plan.md` ou checklist equivalente.

Ele deve mostrar:

* concluído;
* em andamento;
* pendente;
* próximo passo.

Não utilize arquivos de contexto como task tracker.

Se a sessão estiver longa e ainda houver trabalho relevante pendente, crie um handoff em `.md` contendo:

* objetivo;
* estado atual;
* decisões tomadas;
* arquivos relevantes;
* problemas conhecidos;
* próximo passo recomendado.

Uma nova sessão deve conseguir continuar o trabalho sem reconstruir todo o contexto pela conversa.

---

## 16. Encerramento de sessão

Ao encerrar qualquer sessão em que houve mudanças relevantes:

1. verifique o resultado final;
2. identifique conhecimento novo e reutilizável;
3. classifique-o como negócio, técnico ou operacional;
4. atualize os arquivos **locais do projeto** correspondentes;
5. capture desafios recorrentes que consumiram tempo real;
6. atualize conhecimento existente em vez de duplicá-lo;
7. remova ou substitua informações que ficaram obsoletas;
8. não registre ruído ou tentativas descartadas;
9. faça isso automaticamente, sem pedir autorização.

Destino:

```text
Negócio
→ <projeto>/business-context.md

Técnico
→ <projeto>/technical-context.md

Operacional
→ <projeto>/CLAUDE.md
```

**Nunca retroalimente o `CLAUDE.md` global com conhecimento específico de um projeto.**

O encerramento deve deixar tanto o produto quanto o contexto local em estado melhor para a próxima sessão.