---
layout: post
title:  "Lidando com Dados no Jekyll"
date:   2021-09-07 10:00:00 -0300
categories: jekyll
description: Já pensou em como lidar com dados no jekyll? Nesse post vamos discutir alguns formatos de dados e seu fluxo de uso.
tags: jekyll
author: Fábio R. Nóbrega
tags-icon: far fa-gem
---

**“Data is the new oil”** (Dados são o novo Petróleo) como exemplifica o matemático e cientista de dados [Clive Humby](https://en.wikipedia.org/wiki/Clive_Humby), lidar com dados ganha, cada vez mais, um valor inimaginável. Portanto uma sequência de zeros e uns ganha um significado que pode mudar vidas e setores financeiros inteiros. O uso desses dados tem que ser tomados com muita responsabilidade, entretanto o que vamos discutir nesse post é algo bem mais inicial. **Vamos entender como funciona alguns modelos de transmissão de informações no formato texto, como o [YML]({{site.url}}/{{ page.url}}/#yml), [JSON]({{site.url}}/{{ page.url}}/#json) e [CSV]({{site.url}}/{{ page.url}}/#csv), e aplica-los na prática ao framework jekyll**. 

Para começarmos é importante entender o fluxo convencional de distribuição de dados dentro da web. É comum entrarmos em e-commerces e vermos diversos textos e listagens de produtos de forma de cards ou blocos de informação. Na maioria dessas plataformas existe uma API (Application Programming Interface ou em português "Interface de programação de aplicações"), por traz que se conecta a um banco de dados e gerencia essa informação apresentada ao usuário final. Ou seja, cada produto que você visualiza em um site possui uma representação em um banco de dados que pode ser uma linha em uma tabela (caso o banco seja no formato relacional [SQL](https://pt.wikipedia.org/wiki/Banco_de_dados_relacional)). Em seguida esse dado é enviado para a interface ([ Frontend ](https://pt.wikipedia.org/wiki/Front-end_e_back-end)) renderiza-lo podendo ser um modelo de informação como um JSON um YML ou até um CSV. Na maioria das vezes como é o caso de uma API com [Ruby on Rails](https://guides.rubyonrails.org) pode assim retornar uma lista de JSON. Neste sentido, portanto, vamos iterar por essa lista e renderizar separadamente cada informação da mesma usando uma estrutura de repetição como por exemplo um laço for do [liquid](https://shopify.github.io/liquid/).

Contudo, **no jekyll não precisamos criar uma mega estrutura de API e Banco de Dados. Esse framework se utiliza de um gerenciamento de dados estáticos atraves de arquivos fixos.** Aqui, portanto, toda essa lógica convencional da web é simplificada para algo mais enxuto uma vez que uma aplicação em Jekyll não pressupõe uma interação de [big data](https://pt.wikipedia.org/wiki/Big_data) ( mesmo não sendo um impeditivo usá-lo para esse fim). Nesse sentido temos em sua árvore de arquivos uma pasta chamada `_data`. Nela podemos adicionar arquivos de transmissão de informação de diversos tipos, podendo assim acessa-los nos nossos `_layouts` e `_includes`.

### JSON 

JSON é um acrônimo de JavaScript Object Notation ou em portugues "Notação de objeto em JavaScript". E, portanto, é um formato compacto, de padrão aberto independente, de troca de dados simples e rápida (parsing) entre sistemas, como especificou [Douglas Crockford](https://pt.wikipedia.org/wiki/Douglas_Crockford) em 2000.


Sua aplicação é bem simples precisamos criar um arquivo com  um identificador do tipo de dados dentro da pasta `_data`. Neste caso criamos o `myData.json` sendo o `.json` a extensão do arquivo do tipo JSON. 

{% highlight bash linenos %}

  |-- docs
  |----- _data
  |----- myData.json

{% endhighlight %}

Agora devemos criar uma lista de JSON dentro desse aquivo. Para isso precismos entender a sintaxe JSON. Dentro do arquivo precisamos declarar um objeto global que será identificado atraves do uso de chaves `{}`. As propriedades desse objeto serão identificadas com aspas mais o nome da propriedade seguido por dois pontos `"myList":`. Feito isso atribuimos um valor para essa propriedade, neste caso estamos criando um array de objetos e para isso identificamos um array ou lista com o sinal de colchetes `[]` dentro da lista seguimos a mesma lógica anterior para declarar objetos como demonstra o exemplo. 

É importante também entender que os valores atribuidos a propriedades possuem seus tipos sendo: uma String identificada com aspas `"Seu Texto"`, booleanos com o texto `true` para verdeiro e `false` para falso. Number com numeros sem o uso das aspas `100`.

> OBS: Lembre sempre de depois de declarar uma propriedade adicionar uma vírgula caso haja uma propriedade subsequente.

```
{
  "myList": [
    {
      "name": "Item 1",
      "image": "item1.jpeg",
      "size": 100,
      "active": true,
      "url": "http://item1.com"
    },
    {
      "name": "Item 2",
      "image": "item2.jpeg",
      "size": 100,
      "active": false,
      "url": "http://item2.com"
    },
        {
      "name": "Item 3",
      "image": "item3.jpeg",
      "size": 100,
      "active": true,
      "url": "http://item3.com"
    }
  ]
}
```


Agora que temos nosso arquivo JSON setado com os dados que precisamos renderizar precisamos apenas adicionar a lógica de laço em algum layout do nosso projeto jekyll. Como mostra o exemplo. 

{% highlight html linenos%}
  {% raw %}
<div class="items">
  {% for item in site.data.myData.myList %}  
    {% if item.active %}
      <div class="items__card">
        <img 
          src="{{ site.url }}/assets/{{ item.image }}" 
          alt="{{ item.name }}"
        >
        <div class="items__card--name">
          {{ item.name }}
        </div>
        <a 
          class="items__card--url" 
          href="{{ item.url }}"  
        >                
          {{ item.url }}
        </a>
      </div>
    {% endif %}
  {% endfor %}
</div>
  {% endraw %}
{% endhighlight %}

Neste exemplo estamos fazendo um laço `FOR` para iterar pela lista `myList` acessando atraves do caminho padrão do jekyll `site.data` onde a variável `site` faz referência ao seu projeto raiz e `data` à pasta `_data`, já o `myData` é o nome do arquivo json que representa o objeto global declarado anteriormente. Assim o `myList` é a propriedade array que criamos. A estrutura de decisão encadeada `IF` serve para validarmos a propriedade booleana do nosso JSON. Portanto nesse exemplo termos renderizado apenas o item 1 e 3. 


### YML

YML tambem conhecido como YAML é um acronimo para "YAML Ain't Markup Language" ou em portugues  "YAML não é linguagem de marcação". De forma controversa é uma linguagem de serialização de dados legíveis por seres humanos, isto é, uma tentativa de criar uma formatação de dados de alto nivel, baseado no [XML](https://pt.wikipedia.org/wiki/XML). De forma resumida, o YML cria uma relação de dados bem semelhante ao JSON diminuido a quantidade de caracteres escritos. 

Veja-mos essa construção na prática. Em sua aplicação precisamos criar um arquivo com  um identificador do tipo de dados dentro da pasta `_data`. Neste caso criamos o `myData.yml` ou `myData.yaml` sendo o `.yml` ou `.yaml` a extensão do arquivo do tipo YML. Para fins didáticos vamos seguir essa explicação se utilizando da extensão `.yml`, uma vez que o que realmente importa é o conteudo do arquivo. Essa multipla possibilidade de criar um arquivo YML se dá através da historia onde os sistemas operacionais com windows não aceitavam extensões de dados com mais de três caracteres. 

{% highlight bash linenos %}

  |-- docs
  |----- _data
  |----- myData.yml

{% endhighlight %}


Agora vamos criar uma lista de dados dentro desse aquivo YML. Dentro desse arquivo vamos criar uma lista de dados chamada `myList`mara isso basta criar uma propriedade seguida por dois pontos `:`. Em seguida o que define seu conteudo será a indentação do seu arquivo. Iniciando cada nova propriedade com um hífen `-`e declarando as propriedade seguidas com dois pontos basta seguir seus tipo , srting como texto livre, number como número e booleano como true ou false. Assim como demonstra o exemplo a baixo. 


{% highlight yml linenos %}
  myList:
    - name: Item 1
      image: 1item1.jpeg
      size: 100
      active: true
      url: http://item1.com

    - name: Item 2
      image: item2.jpeg
      size: 100
      active: false
      url: http://item2.com

    - name: Item 3
      image: item3.jpeg
      size: 100
      active: true
      url: http://item3.com
{% endhighlight %}

Agora assim como no JSON podemos fazer um laço de interação `FOR` seguido por um estrutura de decisão encadeada `IF` e assim teremos uma lista de card renderizados. 

{% highlight html linenos%}
  {% raw %}
    <div class="items">
      {% for item in site.data.myData.myList %}  
        {% if item.active %}
          <div class="items__card">
            <img 
              src="{{ site.url }}/assets/{{ item.image }}" 
              alt="{{ item.name }}"
            >
            <div class="items__card--name">
              {{ item.name }}
            </div>
            <a 
              class="items__card--url" 
              href="{{ item.url }}"  
            >                
              {{ item.url }}
            </a>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  {% endraw %}
{% endhighlight %}

Neste exemplo estamos fazendo um laço `FOR` para iterar pela lista `myList` acessando atraves do caminho padrão do jekyll `site.data` onde a variável `site` faz referência ao seu projeto raiz e `data` à pasta `_data`, já o `myData` é o nome do arquivo yml que representa o objeto global declarado anteriormente. Assim o `myList` é a propriedade array que criamos. A estrutura de decisão encadeada `IF` serve para validarmos a propriedade booleana do nosso JSON. Portanto nesse exemplo termos renderizado apenas o item 1 e 3. 


### CSV

https://jekyllrb.com/tutorials/csv-to-table/