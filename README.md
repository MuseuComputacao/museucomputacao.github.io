# Site Museu da Computação UFRJ

Repositório destinado ao site público do Museu da Computação da UFRJ, planejado para publicar textos, posts e pequenas exposições virtuais.

## Estado deste checkout

O branch `main` deste checkout contém apenas este README e a licença. Não há arquivos Jekyll, Gemfile, layouts ou páginas de conteúdo disponíveis localmente.

Antes de alterar as páginas “sobre” ou “tour virtual”:

1. confirme no GitHub qual branch contém o site publicado;
2. valide se o conteúdo está em outro repositório ou em um histórico diferente;
3. registre o branch e o commit usados como base;
4. só então crie a alteração de conteúdo e valide a publicação.

## Desenvolvimento, quando o código do site estiver disponível

O fluxo esperado para um checkout Jekyll é:

```bash
bundle install
bundle exec jekyll serve
```

O servidor local normalmente fica disponível em `http://localhost:4000/`. Confirme a configuração do projeto antes de executar comandos de publicação.

## Convenções de Git

Branches e mensagens de commit devem usar inglês:

- Branches: `feat/branch-name`, `fix/branch-name`, `docs/branch-name`
- Commits: `feat(context): message`, `fix(context): message`, `docs(context): message`, `tests(context): message`

Não registrar credenciais, tokens ou dados de publicação neste README.
