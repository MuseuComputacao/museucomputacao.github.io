---
layout: post
title:  Customizando um tema no Jekyll
date:   2021-09-02 10:00:00 -0300
categories: jekyll
description: Já pensou em como customizar seu site no jekyll? Nesse post vamos discutir como modificar o tema padrão do jekyll
tags: jekyll
author: Thiago B. Mattos
tags-icon: far fa-gem
---

O jekyll é um gerador de sites estáticos. O objetivo deste post é ajudar a construir um site usando jekyll com tema customizado pelo usuário./// usamos pra criar o site e blog do museu da computação da UFRJ e vou ensinar como customizar o tema padrão do Jekyll. Esse post é um exemplo de um tema customizado.
falar sobre os geradores de sites estáticos para introduzir o jekyll./ mostrar o cenário atual
jekyll é o que mais dá liberdade de criação para desenvolver um site

Tabela de Conteudo
=================

  * [Jekyll](#jekyll)
  * [EstruturaJekyll](#estrutura-jekyll)
  * [_Include](#include)
  * [_Layouts](#layouts)
  * [_Sass](#sass)
  * [Assets](#assets)
  * [Modificando o tema](#modificando-o-tema)

## Jekyll 

O Jekyll é uma [biblioteca](https://rubygems.org/gems/jekyll) em ruby que permite a criação de websites e blogs. Sites em Jekyll são programados usando as tecnologias Markdown, [Liquid](https://shopify.github.io/liquid/)(Linguagem de template escrita em Ruby), Html, Css e JavaScript, que, ao serem compilados, são transformados em sites estáticos. Uma das grandes vantagens do Jekyll é o suporte do [GitHub Pages](https://pages.github.com) em que pode-se hospedar o site criado gratuitamente.

##  Estrutura Jekyll

Quando se cria um novo site com o Jekyll (através do comando `jekyll new <PATH>`, tal que o `<PATH>` é o diretório onde o projeto será criado. Caso não exista um diretório correspondente, ele será criado), o Jekyll instala um site que usa uma [Gem](https://pt.wikipedia.org/wiki/RubyGems) de tema chamada Minima. Os temas do Jekyll definem os layouts, includes e stylesheets. Entretando você pode sobrescrever qualquer tema padrão com o seu próprio conteúdo.

Um novo projeto no Jekyll tem esta estrutura: 

{% highlight bash linenos %}.
.
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _posts
│   └── 2016-12-04-welcome-to-jekyll.markdown
├── about.markdown
└── index.markdown
{% endhighlight %}

Após rodar o projeto com o comando `bundle exec jekyll serve` serão criados os diretórios

{% highlight bash linenos %}
├── _site
└── .jekyll-cache
{% endhighlight %}

O primeiro passo para se customizar o tema do site é incluir os diretórios `_include`, `_layouts`, `_sass`, `assets` na root do projeto para customizar o site. Assim que os diretórios forem criados, o site irá quebrar já que estaremos sobrescrevendo os arquivos do Minima.

## _include

O include é uma forma de colocar o mesmo elemento várias vezes em uma mesma página ou em páginas diferentes sem ter que repetir o código fonte, deixando assim o html mais limpo e legível. Como os includes são uma cópia do código fonte original, basta modificar o código fonte que as mudanças serão refletidas em todas as instâncias daquele elemento. Um include é instanciado da seguinte forma:

{% highlight liquid linenos%}
{% raw %}
{% include footer.html %}
{% endraw %}
{% endhighlight %}

Pode-se também passar parâmetros na chamada de um include. Por exemplo, suponha que você tem um arquivo com o nome `note.html` no seu diretório _includes que tenha essa formatação:

{% highlight html linenos%}
{% raw %}
<div markdown="span" class="alert alert-info" role="alert">
<i class="fa fa-info-circle"></i> <b>Note:</b>
{{ include.content }}
</div>
{% endraw %}
{% endhighlight %}

O {% raw %} `{{ include.content }}` {% endraw %} é um parâmetro que é preenchido quando você instancia o `note.html` e passa um valor na chamada para esse parâmetro, como por exemplo:

{% highlight liquid linenos%}
{% raw %}
{% include note.html content="This is my sample note." %}
{% endraw %}
{% endhighlight %}

O valor do content(que nesse caso é "This is my sample note.") será inserido no lugar do parâmetro {{ include.content }}

## _layouts

Layouts são modelos que envolvem seu conteúdo. Eles permitem que você crie um esqueleto de página que você possa usar várias vezes só mudando algumas informações, como o conteúdo que vai ser exibido, o titulo da página, dentre outras. 

Para se criar um layout, a primeira etapa é colocar o código-fonte do modelo em um arquivo `.html` que desejar, neste caso, usaremos o default.html. o `content` é uma variável especial, o valor é o conteúdo renderizado do post ou da página que está sendo encapsulado.

{% highlight html linenos%}
{% raw %}
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
    <nav>
      <a href="/">Home</a>
      <a href="/blog/">Blog</a>
    </nav>
    <h1>{{ page.title }}</h1>
    <section>
      {{ content }}
    </section>
    <footer>
      &copy; to me
    </footer>
  </body>
</html>
{% endraw %}
{% endhighlight %}

A variável `page.title`, por sua vez, é obtida através das metatags da página que está a ser desenvolvida.

{% highlight liquid linenos%}
{% raw %}
---
title: My First Page
layout: default
---

This is the content of my page
{% endraw %}
{% endhighlight %}

O conteúdo final da página com o nome "My First Page", após a substituição do `content` e do `page.title` no código fonte será: 

{% highlight html linenos%}
{% raw %}
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>My First Page</title>
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
    <nav>
      <a href="/">Home</a>
      <a href="/blog/">Blog</a>
    </nav>
    <h1>My First Page</h1>
    <section>
      This is the content of my page
    </section>
    <footer>
      &copy; to me
    </footer>
  </body>
</html>
{% endraw %}
{% endhighlight %}

## _sass

Sass (em inglês, "syntactically awesome stylesheets", ou "folhas de estilo sintaticamente incríveis") é uma linguagem de folhas de estilo. Sass é uma linguagem de script que é interpretada ou compilada em Cascading Style Sheets (CSS).
O jekyll tem suporte nativo ao Sass, e, para usarmos, basta criar um arquivo com o nome `main.sass` dentro do diretório `_sass`.

## assets

É neste diretório que ficam todos os arquivos de imagem que serão utilizados pelos layouts e includes. Além das imagens, fica o `main.sass` que vai importas o arquivo `main` do diretório `_sass`.

## Modificando o tema

Agora que entendemos para que serve cada um dos diretórios presentes em um tema, precisamos agora criar o nosso próprio tema com essa mesma estrutura. Portanto, iremos criar um diretório na root do projeto para cada um dos conteúdos presentes no mínima( _include, _layouts, _sass, assets). Ao fazer isso, estaremos sobrescrevendo os diretórios do tema Minima com os nossos próprios diretórios. Note que todos os includes, layouts, assets e folhas de estilo do Minima serão perdidos, já que o projeto vai procurar os arquivos diretamente nos diretórios que acabamos de criar.

Criaremos então os includes `navbar.html` e `footer.html` dentro de `_includes`: 
.
.
.
.

Notas
=============

Em temas baseados em gems, alguns diretórios do site (como os diretórios assets, _layouts, _includes, e _sass) são armazenados na gem do tema, escondido da visualização inicial. O jekyll vai sempre procurar primeiro nos arquivos do site antes de olhar nos arquivos da gem, portanto para criarmos o nosso próprio tema, devemos sobrescrever as pastas do tema atual(minima, o tema padrão do jekyll).
Para fazer isto, basta que criemos os diretórios: assets, _layouts, _includes, _sass. Ao criar, a visualização do site irá quebrar, como de esperado, já que o jekyll não está mais reconhecendo as pastas padrão do tema.