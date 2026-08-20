# Claude Code Config

Repositório pessoal com configurações e referências que utilizo no Claude Code.

## Arquivos

### `CLAUDE.md`

Contém minhas **instruções globais para o Claude Code**.

Define comportamentos que devem valer em qualquer projeto, como:

* estilo de resposta;
* autonomia de execução;
* segurança e permissões;
* cuidados com ações destrutivas;
* Git e qualidade de código;
* organização do contexto dos projetos;
* retroalimentação de contexto ao final das sessões.

Contextos específicos de negócio e tecnologia permanecem dentro de cada projeto e são referenciados pelo `CLAUDE.md` local.

### `SKILLS.md`

Lista de **skills importantes/recomendadas** que utilizo ou quero ter como referência.

Por enquanto, funciona apenas como um catálogo de nomes de skills úteis, e não como implementação das próprias skills.

---

## Instalando o `CLAUDE.md` global

O Claude Code utiliza `~/.claude/CLAUDE.md` para instruções pessoais carregadas em todos os projetos.

### Windows

No PowerShell:

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude"
Copy-Item ".\CLAUDE.md" "$HOME\.claude\CLAUDE.md"
```

O arquivo ficará normalmente em:

```text
C:\Users\SEU_USUARIO\.claude\CLAUDE.md
```

### Linux

```bash
mkdir -p ~/.claude
cp CLAUDE.md ~/.claude/CLAUDE.md
```

Resultado:

```text
~/.claude/CLAUDE.md
```

### macOS

```bash
mkdir -p ~/.claude
cp CLAUDE.md ~/.claude/CLAUDE.md
```

Resultado:

```text
~/.claude/CLAUDE.md
```

Depois disso, o conteúdo do `CLAUDE.md` estará disponível globalmente nas sessões do Claude Code.

> O `CLAUDE.md` deste repositório é global. Cada projeto pode continuar tendo seu próprio `CLAUDE.md` com instruções e referências específicas daquele projeto.
