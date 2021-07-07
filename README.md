# Site Museu da Computação UFRJ 

Esse projeto é o MVP do site do museu ele foi desenvolvido com [jekyll](https://jekyllrb.com/) e tem a intenção de ser nosso primeiro contado com a comunidade. Adicionando assim blog posts e pequenas exposições virtuais. 

Nós usamos

+ Ruby 2.7.0
+ Jekyll 4.2.0

## Tabela de Conteúdo
=================

  * [Install](#install)
  * [Uso](#uso)
  * [Diretrizes Git](#git-guideline)

## Install

+ Clone the repo and cd into docs folder

``` bash
$ bundle install
```

## Uso

```bash 
$ bundle exec jackyll serve
```

The application will become available at the URL:

```
http://localhost:3000/
```


## Git Guideline
Crie suas branches e commits em inglês seguindo as diretrizes a seguir: 

#### Branches
- Feature:  `feat/branch-name`
- Hotfix: `hotfix/branch-name`
- POC: `poc/branch-name`

#### Commits prefix
- Chore: `chore(context): message`
- Feat: `feat(context): message`
- Fix: `fix(context): message`
- Refactor: `refactor(context): message`
- Tests: `tests(context): message`
- Docs: `docs(context): message`

#### Abrindo PR's 

Quando abrir um PR por favor siga nosso template. 
