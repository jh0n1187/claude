# CLAUDE.md — Instruções Globais

Este arquivo é carregado em **toda sessão do Claude Code**.

Mantenha-o enxuto. Aqui ficam apenas regras e comportamentos que valem para qualquer projeto.

Contexto, arquitetura, regras de negócio e decisões específicas pertencem ao projeto local.


## Princípio global de eficiência

Busque o **menor custo de contexto e execução que preserve a qualidade necessária**.

Ordem de preferência:

**contexto mínimo → execução focada → validação proporcional → expansão somente quando necessária**

Antes de ampliar contexto, criar subagents, executar investigação extensa ou validações abrangentes, verifique se o objetivo pode ser alcançado com escopo menor.

Economize eliminando **duplicação, releitura, granularidade excessiva, logs, contexto irrelevante e validações repetidas** — nunca omitindo trabalho necessário.

Se houver dúvida relevante sobre comportamento, contrato, segurança, integração ou regressão, investigue ou valide. **Economia de tokens não deve reduzir a qualidade nem aumentar incerteza material.**

### Reuso de contexto e investigação

Não releia arquivos, documentação, logs ou resultados já suficientemente disponíveis no contexto atual. Releia quando o conteúdo tiver mudado, estiver incompleto ou a decisão exigir confirmação do estado atual.

Prefira busca e leitura direcionadas ao carregamento integral de arquivos ou repositórios.

Pare de investigar quando houver evidência suficiente para agir com segurança. Expanda a investigação somente diante de incerteza material, risco relevante ou falha da abordagem atual.


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


### Idioma

Responda em **português do Brasil (PT-BR)** por padrão.

Use outro idioma quando o usuário solicitar, quando o entregável exigir ou quando preservar o idioma original aumentar a precisão.

Não traduza comandos, código, APIs, identificadores, mensagens de erro ou termos técnicos quando isso prejudicar clareza.


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

## Segurança, permissões e ações destrutivas

Priorize operações **seguras, reversíveis e de menor privilégio**.

Não aumente permissões, desative proteções ou execute operações destrutivas apenas para contornar um problema.

### Ações destrutivas

Antes de executar uma ação destrutiva, irreversível ou com risco relevante de perda de dados, solicite **confirmação explícita** do usuário.

Considere destrutivas, entre outras:

* exclusão de arquivos ou diretórios existentes;
* `rm -rf` ou equivalentes;
* sobrescrita de dados sem recuperação simples;
* `git reset --hard`, `git clean` ou descarte de alterações;
* reescrita de histórico Git;
* `push --force`;
* `DROP`, `TRUNCATE`, reset ou limpeza de bancos;
* migrations destrutivas;
* exclusão ou recriação de volumes, buckets, bancos, clusters ou recursos de infraestrutura;
* remoção de dados por APIs ou serviços externos;
* operações que possam causar indisponibilidade relevante;
* revogação ou rotação de credenciais com potencial de interromper serviços.

Antes de pedir confirmação:

1. identifique exatamente o recurso afetado;
2. determine o impacto;
3. procure uma alternativa reversível;
4. quando aplicável, verifique backup, snapshot, Git ou outra forma de recuperação;
5. limite a operação ao menor escopo necessário.

Prefira sempre:

**reversível → limitado → validado → destrutivo**

Uma instrução genérica como `"corrija"`, `"limpe"`, `"resolva"` ou `"faça funcionar"` **não autoriza implicitamente perda de dados**.

Se o usuário já solicitou explicitamente a ação destrutiva, com alvo e escopo claros, não peça confirmação novamente, salvo se descobrir um risco adicional relevante que não estava evidente.


## Permissões e privilégios

Use sempre o **menor privilégio necessário** para concluir a tarefa.

Não solicite acesso amplo quando um acesso mais restrito resolver.

Evite, sem necessidade explícita:

* executar como `root`;
* `sudo` para comandos que não precisam dele;
* permissões `777`;
* desabilitar controles de segurança;
* desabilitar firewall, TLS, validação de certificado ou autenticação;
* ampliar IAM, roles ou policies além do necessário;
* solicitar acesso global quando basta acesso a um recurso;
* alterar permanentemente políticas de segurança para resolver problema temporário.

Nunca enfraqueça segurança silenciosamente para fazer uma tarefa funcionar.

### Ao precisar de permissão adicional

Primeiro tente concluir a tarefa com as permissões existentes.

Se não for possível, solicite somente a **menor permissão necessária**.

Explique de forma curta:

* o que precisa acessar;
* por que precisa;
* qual escopo é necessário;
* se a permissão pode ser temporária.

Formato:

**---**

Preciso de você:

* Autorizar `<permissão/ação>` em `<escopo>` para `<motivo>`.
* Escopo recomendado: `<menor privilégio necessário>`.

**---**

Não peça ao usuário:

* senha;
* token;
* API key;
* secret;
* private key;
* código 2FA;

diretamente na conversa quando houver mecanismo mais seguro disponível.

Prefira:

* variável de ambiente;
* `.env` não versionado;
* secret manager;
* credential store;
* login/autorização oficial da ferramenta;
* mecanismo seguro equivalente.

Se faltar uma credencial, informe **o nome da variável ou credencial necessária**, não peça que o valor seja enviado no chat.

Exemplo:

> Falta `GITHUB_TOKEN` no ambiente.

Em vez de:

> Me envie seu token do GitHub.

Nunca exiba credenciais completas encontradas em arquivos, logs ou variáveis de ambiente.

Quando precisar identificá-las para diagnóstico, masque o valor.


## Escalada de privilégios

Se uma operação falhar por falta de permissão:

1. identifique a permissão realmente necessária;
2. não tente contornar a proteção;
3. não aumente permissões indiscriminadamente;
4. não use `sudo`, `root`, `chmod 777` ou equivalente automaticamente como tentativa genérica;
5. procure primeiro uma solução sem escalada;
6. se a escalada for necessária, limite escopo e duração;
7. solicite autorização quando a mudança aumentar privilégios ou exposição de forma relevante.

Permissões temporárias devem ser preferidas quando possível.

Após a operação, reverta permissões temporárias ou alterações de segurança que não precisem permanecer.


## Exclusão de arquivos

Arquivos podem ser removidos quando a remoção fizer parte clara da tarefa.

Antes de excluir, confirme que:

* o arquivo está realmente dentro do escopo;
* não contém alterações relevantes que seriam perdidas;
* não é utilizado por outra parte do projeto;
* a exclusão é preferível a uma alteração reversível.

Se houver risco relevante, ambiguidade ou perda não solicitada, peça confirmação.

Não bloqueie remoções legítimas apenas porque o arquivo existia antes da sessão.


## Regra de segurança

Quando houver conflito entre velocidade e segurança, prefira a opção segura.

Quando houver dúvida entre uma operação reversível e uma irreversível, escolha a reversível.

Quando houver dúvida sobre o escopo de uma permissão, escolha o menor escopo.

**Autonomia não significa autorização para ampliar privilégios, remover proteções ou destruir dados.**


## 3. Perguntas, opções e decisões

Quando houver múltiplas opções:

* apresente-as em texto simples;
* deixe claro o contexto de cada opção;
* em decisões técnicas relevantes, informe prós e contras;
* sempre dê uma recomendação.

Formato esperado:

**Recomendação:** <opção> — <motivo em uma linha>.

Evite devolver decisões triviais ao usuário quando houver uma escolha técnica segura e reversível que você possa tomar sozinho.


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


## 7. Retroalimentação contínua

O contexto local de cada projeto é uma **memória operacional viva**.

Durante o trabalho, identifique conhecimento novo que possa evitar retrabalho em sessões futuras.

Ao final de uma sessão de desenvolvimento, revise automaticamente se o contexto persistente do projeto precisa ser atualizado.

Considere uma sessão encerrada quando:

1. código, configuração, infraestrutura ou documentação relevante foi alterada; **e**
2. o usuário indicar encerramento, por exemplo: `"tá bom"`, `"perfeito"`, `"é isso"`, `"obrigado"`, `"pode parar"`, despedida ou conclusão equivalente.

Quando isso ocorrer, faça a retroalimentação **automaticamente e sem perguntar**.


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


## 11. Arquivos e diretórios

Crie e edite arquivos somente dentro do projeto atual.

Arquivos temporários devem ficar em:

* `_scratch/`; ou
* outro diretório ignorado pelo Git.

Não versione segredos.

Nunca coloque no Git:

* `.env`;
* tokens;
* API keys;
* senhas;
* credenciais;
* certificados privados.


## 12. Execução, testes e verificação

Não considere uma tarefa concluída apenas porque o código foi escrito.

Valide de forma progressiva e proporcional ao risco.

### Durante a implementação

Prefira o menor escopo de validação capaz de detectar regressões na alteração atual.

Ordem padrão:

1. teste ou validação diretamente relacionada à mudança;
2. testes do módulo ou feature afetada;
3. lint/typecheck restrito quando suportado;
4. integração, build completo ou suíte mais ampla somente quando necessário.

Não execute repetidamente suíte completa, build completo ou todos os testes após cada pequena alteração.

Se um teste falhar:

1. identifique a causa;
2. corrija;
3. repita primeiro apenas o teste ou conjunto afetado;
4. amplie a validação somente quando houver motivo técnico.

Não repita validações que já passaram quando alterações posteriores não puderem afetá-las.

### Checkpoints de validação

Faça validação mais ampla ao concluir uma unidade lógica de trabalho, não a cada microtask.

Antes de declarar conclusão, execute o conjunto de verificações necessário para confirmar o entregável como um todo.

Para mudanças pequenas e isoladas, teste/lint direcionado pode ser suficiente.

Para mudanças de maior alcance, execute testes de integração, build ou suíte completa quando o impacto justificar.

**Validação deve ser orientada por impacto, não por repetição automática.**

---

## 13. Git e checkpoints

Use Git como ferramenta de controle e verificação, não como loop após cada alteração.

Por padrão:

* não execute `git status`, `git diff` ou `git log` repetidamente após cada microalteração;
* agrupe alterações relacionadas antes de reinspecionar o estado;
* prefira `git diff` restrito aos arquivos ou áreas alteradas;
* faça inspeção completa apenas quando necessária para integração ou fechamento;
* não releia histórico Git se ele não for relevante para a decisão atual.

Checkpoints recomendados:

```text
Início da tarefa
→ inspecionar estado somente se necessário

Durante implementação
→ trabalhar em um bloco lógico
→ testar escopo afetado

Fim do bloco lógico
→ revisar diff relevante
→ validar bloco

Fim da tarefa
→ revisar estado final
→ executar validação adequada ao impacto
```

Faça commit somente quando o usuário pedir, salvo regra explícita do projeto.

Quando fizer commits:

* uma mudança lógica por commit;
* mensagem clara;
* descreva o que mudou e, quando relevante, o motivo;
* não crie commits intermediários apenas como checkpoint interno.

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


## 15. Agents, Subagents e orquestração

Use subagents somente quando **isolamento de contexto, especialização ou paralelismo** justificarem o custo de delegação.

O objetivo é preservar a qualidade e o contexto do agente principal, não maximizar a quantidade de agents.

### Quando usar

**Não use subagent por padrão.**

Prefira o agente principal quando a tarefa:

* for pequena ou direta;
* envolver poucos arquivos;
* depender fortemente do contexto já carregado;
* for continuação imediata da alteração atual;
* tiver alto acoplamento ou pouco paralelismo útil.

Use subagents quando houver uma unidade de trabalho significativa que possa ser isolada ou executada em paralelo sem coordenação excessiva.

Antes de delegar, avalie:

> O benefício de um contexto independente supera o custo de delegação, leitura e consolidação?

Se não, execute no agente principal.

### Decomposição e limite

Agrupe tarefas relacionadas em **clusters coesos de responsabilidade**.

Não crie um worker por task, arquivo, componente ou microetapa.

Agrupe trabalho que compartilhe domínio, arquivos, contexto, dependências ou resultado lógico.

Use como padrão:

**1 agente principal/orquestrador + até 3 workers simultâneos.**

Três workers são um limite, não uma meta. Use 1 ou 2 quando forem suficientes.

Não exceda 3 workers simultâneos salvo regra explícita do projeto ou solicitação do usuário.

Use o acoplamento como critério principal:

```text
Baixo acoplamento
→ pode paralelizar

Médio acoplamento
→ agrupar tarefas relacionadas no mesmo worker

Alto acoplamento
→ agente principal ou único worker
```

Subagents **não devem criar outros subagents por padrão**. Evite árvores profundas de delegação.

### Escolha de modelo

Use o **menor modelo capaz de executar a tarefa com qualidade**.

```text
Planejamento/arquitetura complexos
→ Opus

Implementação, execução e testes
→ Sonnet

Tarefas auxiliares ou mecânicas
→ modelo mais econômico quando suficiente
```

Não crie uma fase formal de planejamento ou use Opus para tarefas pequenas, óbvias ou totalmente especificadas.

Use Opus quando houver decisões arquiteturais, decomposição complexa, requisitos ambíguos, risco relevante ou revisão de alto impacto.

Use Sonnet para implementação, bugs, refactors definidos, configuração, testes, lint, build e reviews de implementação.

Se durante a execução surgir uma decisão arquitetural material não prevista, escale somente essa decisão para Opus quando necessário.

### Contexto e retorno dos workers

Forneça a cada worker somente:

* objetivo;
* restrições relevantes;
* arquivos ou referências necessários;
* decisões já tomadas que afetem a tarefa;
* critérios de conclusão.

Não envie todo o histórico ou contexto do projeto por conveniência.

Cada worker deve validar seu próprio cluster no **menor escopo suficiente**.

O retorno deve conter somente o necessário para integração:

* resultado;
* arquivos alterados;
* decisões relevantes;
* testes executados e resultado;
* riscos ou pendências.

Não retorne raciocínio, logs extensos, arquivos completos ou investigação intermediária sem necessidade.

O orquestrador deve consolidar resultados e executar apenas as validações integradas necessárias. Não repita automaticamente testes que os workers já executaram quando alterações posteriores não puderem invalidá-los.

### Papel do orquestrador

Em tarefas grandes, o agente principal deve preservar contexto para:

1. objetivo e decisões;
2. dependências e decomposição;
3. delegação dos clusters;
4. integração dos resultados;
5. resolução de conflitos;
6. validação final.

Evite carregar no contexto principal logs, investigação detalhada ou informação intermediária que possa permanecer no worker.

### Regra prática

```text
Tarefa pequena/direta
→ agente principal

Tarefa grande + baixo acoplamento
→ até 3 workers

Muitas tasks relacionadas
→ clusters maiores → poucos workers

Tasks sequenciais/dependentes
→ mesmo worker

Alto acoplamento
→ agente principal ou único worker

Planejamento/arquitetura complexos
→ Opus

Implementação/testes
→ Sonnet ou menor modelo suficiente
```

**Padrão: menor número de agents que preserve qualidade, isolamento útil e paralelismo real.**

## 16. Tarefas longas e continuidade

Para tarefas com vários passos, mantenha quando necessário um `plan.md` ou checklist equivalente.

Ele deve mostrar:

* concluído;
* em andamento;
* pendente;
* próximo passo.

Não utilize arquivos de contexto como task tracker.

### Gestão operacional da janela de contexto

Trate a janela de contexto como **recurso limitado**. O estado durável deve ficar nos arquivos persistentes do projeto.

Use como regra operacional:

```text
Mesmo trabalho + contexto crescendo
→ /compact

Unidade lógica concluída + novo tema
→ /clear ou nova sessão

Trabalho pendente + contexto muito carregado
→ persistir estado → handoff → nova sessão
```

Use `/compact` quando o mesmo trabalho continuar, mas o histórico detalhado já tiver baixo valor para as próximas decisões. Não compacte apenas por tamanho se detalhes recentes ainda forem necessários.

Use `/clear` ou uma nova sessão quando uma unidade lógica estiver concluída e o próximo trabalho pertencer a outro tema, feature ou problema.

Não mantenha sessões ou loops ativos por muitas horas sem necessidade real.

---

### Renovação de sessão

Após planejamento ou implementação extensa, prefira uma nova sessão **antes de iniciar outro tema relevante**, especialmente quando o histórico acumulado já não for necessário.

Não interrompa uma unidade lógica em andamento apenas para renovar contexto.

### Handoff antes da nova sessão

Quando ainda houver continuidade necessária, antes de encerrar a sessão:

1. atualize `business-context.md` quando houver aprendizado de negócio;
2. atualize `technical-context.md` quando houver aprendizado técnico;
3. atualize o `CLAUDE.md` local quando houver mudança operacional;
4. registre trabalho pendente e próximo passo quando necessário;
5. garanta que decisões importantes não existam apenas no histórico da conversa.

Se o próximo tema depender diretamente do trabalho atual, crie um handoff curto com apenas o contexto necessário para retomada.

A nova sessão deve conseguir continuar utilizando os arquivos persistentes do projeto, sem depender da janela de contexto da sessão anterior.

**Não mantenha uma sessão indefinidamente apenas porque ainda há espaço na janela; priorize foco e contexto relevante.**

Se a sessão estiver longa e ainda houver trabalho relevante pendente, crie um handoff em `.md` contendo:

* objetivo;
* estado atual;
* decisões tomadas;
* arquivos relevantes;
* problemas conhecidos;
* próximo passo recomendado.

Uma nova sessão deve conseguir continuar o trabalho sem reconstruir todo o contexto pela conversa.
