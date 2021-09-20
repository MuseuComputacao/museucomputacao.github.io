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

O jekyll é um gerador de sites estáticos. Mas o que isso significa? Uma página estática é um site da Internet que exibe o mesmo conteúdo para todos os usuários, em vez de fornecer conteúdos personalizados para cada usuário. Sites estáticos têm como vantagens a velocidade de carregamento e o baixo custo de desenvolvimento, e, por isso, houve um crescimento no mercado de geradores de sites estáticos. Os geradores de sites estáticos nos permitem trabalhar como se estivéssemos criando páginas dinâmicas, ou seja, podemos criar pedaços de HTML que ficarão em arquivos separados. A diferença é que, ao invés de gerar uma página quando o usuário requisitar, os geradores já geram todos os arquivos HTML. Assim, quando alguém requisitar uma página, ela já estará pronta para ser entregue. Alguns geradores de sites estáticos famosos são o [Wix](https://pt.wix.com/), [WordPress](https://wordpress.com/pt-br/), [Nuxt.js](https://nuxtjs.org), etc. Mas dentre tantos, um que se destaca por por ser open-source e dar a maior liberdade pro usuário constuir e personalizar seu site é o [Jekyll](https://jekyllrb.com). E nesse post vou ensinar como customizar o tema padrão do Jekyll para que você possa utilizar o jekyll ao seu máximo.

Tabela de Conteudo
=================

  * [Jekyll](#jekyll)
  * [EstruturaJekyll](#estrutura-jekyll)
  * [Assets](#assets)
  * [_Sass](#sass)
  * [_Include](#include)
  * [_Layouts](#layouts)

## Jekyll 

O Jekyll é uma [biblioteca](https://rubygems.org/gems/jekyll) em ruby que permite a criação de websites e blogs. Sites em Jekyll são programados usando as tecnologias Markdown, [Liquid](https://shopify.github.io/liquid/)(Linguagem de template escrita em Ruby), Html, Css e JavaScript, que, ao serem compilados, são transformados em sites estáticos. Uma das grandes vantagens do Jekyll é o suporte do [GitHub Pages](https://pages.github.com) em que pode-se hospedar o site criado gratuitamente.

##  Estrutura Jekyll

Quando se cria um novo site com o Jekyll (através do comando `jekyll new <PATH>`, onde o `<PATH>` é o caminho em que será criado o projeto), o Jekyll cria um projeto que usa uma [Gem](https://pt.wikipedia.org/wiki/RubyGems) de tema chamada Minima. Os temas do Jekyll definem os layouts, includes e stylesheets. Entretando você pode sobrescrever qualquer tema padrão com o seu próprio conteúdo.

Um novo projeto no Jekyll tem esta estrutura: 

{% highlight bash linenos %}
.
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _posts
│   └── 2016-12-04-welcome-to-jekyll.markdown
├── about.markdown
└── index.markdown
{% endhighlight %}

O primeiro passo para se customizar o tema do site é incluir os diretórios `_include`, `_layouts`, `_sass`, `assets` na root do projeto para customizar o site. Mesmo que os diretórios tenham sido criados, como não há arquivo nenhum dentro deles, o jekyll continuará a renderizar o conteúdo do Minima. Para criar o nosso tema, teremos então que adicionar nossas mudanças nos diretórios correspondentes. Após criar estes diretórios, a estrutura do projeto ficará assim: 

{% highlight bash linenos %}
.
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _data
├── _includes
├── _layouts
├── _posts
│   └── 2016-12-04-welcome-to-jekyll.markdown
├── _sass
├── about.markdown
└── index.markdown
{% endhighlight %}

## assets

O diretório assets serve para guardar qualquer arquivo que seja útil para o site, como imagens, arquivos svg, pdfs, dentre muitos outros. Este diretório também armazena o arquivo css que o jekyll irá usar para buildar o site.
Após criarmos o diretório `assets`, iremos criar um outro diretório dentro dele, chamado `css`, será aqui que irá ficar o arquivo sass principal que será convertido para css.
Assim, devemos criar o arquivo `main.sass`. Esse arquivo vai somente importar o `main.sass` do diretório `_sass`. Poderíamos, por exemplo, criar todos os estilos usados no site dentro deste arquivo sass, mas, para deixar-mos mais organizado, iremos apenas importar o `main.sass` do diretório `_sass` neste arquivo. E então, o arquivo main dentro de assets/css fica da seguinte forma:

{% highlight liquid linenos%}
{% raw %}
---
---

@import "main"
{% endraw %}
{% endhighlight %}

## _sass

Sass (em inglês, "syntactically awesome stylesheets", ou "folhas de estilo sintaticamente incríveis") é uma linguagem de folhas de estilo. Sass é uma linguagem de script que é interpretada ou compilada em Cascading Style Sheets (CSS). O jekyll tem suporte nativo ao Sass, e, para usarmos, basta criar um arquivo com o nome `main.sass` dentro do diretório `_sass`. Por motivos de organização, iremos criar um arquivo .sass para cada componente que iremos fazer, assim como também podemos criar para cada view que quisermos, e, para cada novo arquivo sass que criarmos, devemos importa-lo no `main.sass` para que ele possa ser enviado para o `main.sass` do diretório `assets` e consequentemente, renderizado no site. Além do `main.sass`, ainda criaremos o arquivo `_variables.sass` que irá armazenar as variáveis que iremos usar para padronizar o projeto, e para usar estas variáveis, basta dar um `import` dentro do arquivo sass que desejar usar e usar a sintaxe `$nome-da-variavel` quando quiser usar algum valor dentro de `_variables.sass`. No nosso projeto, usaremos as seguintes variaveis:

{% highlight sass linenos %}
{% raw %}
$bg-color: #333
$text-color: #f2f2f2
$hover-color: #ddd
$contrast-color: #04AA6D
{% endraw %}
{% endhighlight %}

## _include

O include é uma forma de colocar o mesmo elemento várias vezes em uma mesma página ou em páginas diferentes sem ter que repetir o código fonte, deixando assim o html mais limpo e legível. Como os includes são uma cópia do código fonte original, basta modificar o código fonte que as mudanças serão refletidas em todas as instâncias daquele elemento. Portanto, criaremos dentro do diretório _includes(que acabamos de criar), o arquivo `head.html` com as informações básicas do site:

{% highlight html linenos %}
{% raw %}
<head>
    <link rel="stylesheet" type="text/css" href="{{ site.baseusrl }}/assets/css/main.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
{% endraw %}
{% endhighlight %}

E também iremos criar o arquivo `header.html` que irá conter o html da nossa Navbar simples e responsiva.

{% highlight html linenos %}
{% raw %}
<div class="topnav" id="myTopnav">
  <a href="#home" class="active">Home</a>   
  <a href="#about">About</a>
  <a href="javascript:void(0);" class="icon" onclick="myFunction()">
    <i class="fa fa-bars"></i>
  </a>
</div>

<script>
  function myFunction() {
    var x = document.getElementById("myTopnav");
  if (x.className === "topnav") {
    x.className += " responsive";
  } else {
    x.className = "topnav";
  }
}
</script>
{% endraw %}
{% endhighlight %}

Para finalizar o header, basta criar o estilo dele, para isso, dentro do diretório `_sass`, criaremos um diretório chamado `components`. O diretório components irá conter todos os estilos dos includes do nosso site. Portanto, criaremos um arquivo chamado `header.sass`. Esse arquivo irá conter o seguinte:

{% highlight sass linenos%}
{% raw %}
@import './_variables'

.topnav
  overflow: hidden
  background-color: $bg-color
  a
    float: left
    display: block
    color: $text-color
    text-align: center
    padding: 14px 16px
    text-decoration: none
    font-size: 17px
    &.active
      background-color: $contrast-color
      color: white
    &:hover
      background-color: $hover-color
      color: black
  .icon
    display: none

@media screen and (max-width: 600px)
  .topnav 
    a
      &:not(:first-child) 
        display: none
      &.icon
        float: right
        display: block
    &.responsive
      position: relative
      .icon
        position: absolute
        right: 0
        top: 0
      a
        float: none
        display: block
        text-align: left
{% endraw %}
{% endhighlight %}

Agora, iremos criar o footer usando a mesma lógica do header. Basta criar um arquivo `footer.html` no diretório `_includes`.

{% highlight html linenos %}
{% raw %}
<footer>
    <div><p>&copy Copyright 2021.</p></div>
</footer>
{% endraw %}
{% endhighlight %}

{% highlight sass linenos%}
{% raw %}
@import "../_variables"

footer
    position: absolute
    background-color: $bg-color
    color: $text-color
    width: 100%
    height: 20px
    border-top: 2px solid $contrast-color
    text-align: center
    bottom: 0
    padding: 10px 0
{% endraw %}
{% endhighlight %}

Para incluir um elemento a alguma página, basta usar o comando {%raw%}`{% include nome-do-include.html %}`{%endraw%}.

{% highlight liquid linenos%}
{% raw %}
{% include header.html %}
{% endraw %}
{% endhighlight %}

## _layouts

Layouts são modelos que envolvem seu conteúdo. Eles permitem que você crie um esqueleto de página que você possa usar várias vezes só mudando algumas informações, como o conteúdo que vai ser exibido, o titulo da página, dentre outras. 

Para se criar um layout, a primeira etapa é colocar o código-fonte do modelo em um arquivo `.html` que desejar, neste caso, usaremos o default.html.

{% highlight html linenos%}
{% raw %}
---
---
{% include head.html %}

{% include header.html %}

<head>
    <title>{{ page.title }}</title>
</head>

<main>
    <section>
        {{ content }}
    </section>
</main>

{% include footer.html %}
{% endraw %}
{% endhighlight %}

o `content` é uma variável especial, o valor é o conteúdo renderizado do post ou da página que está sendo encapsulado. A variável `page.title`, por sua vez, é obtida através das metatags da página que está a ser desenvolvida. Usaremos então o arquivo `index.markdown` para criar nossa página principal.

{% highlight liquid linenos%}
{% raw %}
---
layout: home
title: Home
---

<h1>Esse é um exemplo do que podemos fazer com o jekyll.</h1>
{% endraw %}
{% endhighlight %}

E, com isso, pudemos criar um site estático feito em jekyll com tema customizável. Você pode encontrar o repositório com esse projeto [aqui]() para poder baixar e usar como quiser.