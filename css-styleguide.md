Css Styleguide
==============
Bem-vindo ao Everestti CSS Styleguide.

> *Em desenvolvimento*

## Índice

1. [Princípios](#princípios)
2. [](#)
2. [](#)
3. [](#)
4. [](#)
5. [](#)
6. [](#)
7. [](#)

## Princípios
- Separar a estrutura do skin (cores, fontes, bordas, etc.)
- Separe os containers do conteúdo


## Nomeando classes
##### // CSS
( `_` ) Hífen: Separa palavras compostas<br>
```
<div class="guarda_roupa"></div>
```

( `-` ) Traço: Elemento / Filho<br>
```
<div class="guarda_roupa-gaveta"></div>
```

( `__` ) Dois hífens: Modificador de Estado<br>
```
<div class="guarda_roupa-gaveta__aberta"></div>
```
##### // JS
Em elementos que forem receber alguma manipulação via javascript, não vincular à mesma classe ultilizada no CSS. Dessa forma, caso você queria refatorar seu CSS não precisará alterar o JS. Para isso utilize o prefixo `js-`, por exemplo:
```
<input type="submit" class="btn js-btn" value="Submit" />
```


## Identação
Usar dois (2) "espaços" de identação, por exemplo:
```
.foo {
  padding: 5px;
}
```
Nos casos de setar os filhos de `.foo`, indentar em cascata, sempre com 2 espaços a mais, por exemplo:
```
.foo {
  padding: 5px;
}

  .foo--bar {
    padding: 5px;
  }

    .foo--baz {
      padding: 5px;
    }
```


## Comentários
### Titulando
Comece cada nova seção principal de um projeto de CSS com um título:
```
/*------------------------------------*\
  #SECTION-TITLE
\*------------------------------------*/

.selector {}
```
O título de cada seção é prefixado com um símbolo hash (#) para nos permitir realizar pesquisas mais direcionadas. Em vez de procurar apenas **SECTION_TITLE**, que pode render muitos resultados de uma pesquisa mais com um escopo de **#SECTION_TITLE** deve retornar somente a seção em questão.

Deixe uma linha em branco entre este título e a próxima linha de código (seja um comentário, alguns Sass, ou algum CSS).
```
/*------------------------------------*\
  #SECTION_TITLE
\*------------------------------------*/

/**
 * Comment
 */

.selector {}
```
### 80 caracteres de largura
```
/**
 * I am a long-form comment. I describe, in detail, the CSS that follows. I am
 * such a long comment that I easily break the 80 character limit, so I am
 * broken across several lines.
 */
```


## Múltiplas linhas
CSS deve ser escrito em várias linhas, exceto em circunstâncias muito específicas. Há um certo número de benefícios para o presente: A menor possibilidade de conflitos de mesclagem, porque existe cada pedaço de funcionalidade em sua própria linha. Exceções a essa regra devem ser bastante evidente, como conjuntos de regras semelhantes que apenas carregam uma declaração de cada, por exemplo:
```
.icon {
  display: inline-block;
  width:  16px;
  height: 16px;
  background-image: url(/img/sprite.svg);
}

.icon__home     { background-position: 0px 0px; }
.icon__person   { background-position: -16px 0px; }
.icon__files    { background-position: 0px -16px; }
.icon__settings { background-position: -16px -16px; }
```
Strings identicas devem ser alinhadas entre si, por exemplo:
```
.foo {
  -webkit-border-radius: 3px;
     -moz-border-radius: 3px;
          border-radius: 3px;
}

.bar {
  position: absolute;
  top:    0;
  right:  0;
  bottom: 0;
  left:   0;
  margin-right: -10px;
  margin-left:  -10px;
  padding-right: 10px;
  padding-left:  10px;
}
```


## Espaços em branco
- Uma (1) linha em branco entre os conjuntos de regras relacionadas.
- Duas (2) linhas vazias entre os conjuntos de regras vagamente relacionadas.
- Cinco (5) linhas vazias entre novas seções.

```
/*------------------------------------*\
  #FOO
\*------------------------------------*/

.foo {}

  .foo--bar {}


.foo__baz {}





/*------------------------------------*\
  #BAR
\*------------------------------------*/

.bar {}

  .bar--baz {}

  .bar--foo {}
```


## Especificidade (classes vs. ids)
Elementos que ocorrem exatamente uma vez dentro de uma página devem usar IDs, caso contrário, use classes. Quando em dúvida, use um nome de classe.<br>
> Somente atribuir um ID para um elemento inline se for necessário referenciá-lo em um javascript ou jQuery.

- **Bons** candidatos para ids: cabeçalho, rodapé, popups, modais.
- **Maus** candidatos para ids: navegação, listas de itens, botões.


## HTML
Dado por HTML e CSS serem intrinsecamente interligados, seria negligência da minha parte não cobrir algumas orientações de sintaxe e formatação para marcação.

Citar sempre atributos, mesmo que iria trabalhar sem. Isto reduz a possibilidade de acidentes, e é de um formato mais familiar para a maioria dos promotores. Por tudo isso iria funcionar (e é válida):
```
<div class=box>
```
…esse formato é preferido:
```
<div class="box">
```
As aspas não são necessárias aqui, porém melhor incluí-las.

Ao escrever vários valores em um atributo de classe, separe-os com dois espaços, assim:
```
<div class="foo  bar">
```
Quando várias classes estão relacionados uns aos outros, considere agrupando-os em colchetes ([...]), assim:
```
<div class="[ box  box__highlight ]  [ bio  bio__long ]">
```
Esta não é uma recomendação firme, e é algo que eu ainda estou me experimentação, mas não carregam uma série de benefícios. Leia mais em [Grouping related classes in your markup](http://csswizardry.com/2014/05/grouping-related-classes-in-your-markup/).

Tal como acontece com os nossos conjuntos de regras, é possível a utilização de espaços em branco significativo em seu HTML. Você pode denotar breaks temáticos em conteúdo, com cinco (5) linhas vazias, por exemplo:
```
<header class="page_head">
    ...
</header>





<main class="page_content">
    ...
</main>





<footer class="page_foot">
    ...
</footer>
```
Trechos separados e independentes, mas vagamente relacionadas de marcação com uma única linha vazia, por exemplo:
```
<ul class="primary_nav">

    <li class="primary-nav__item">
        <a href="/" class="primary_nav--link">Home</a>
    </li>

    <li class="primary_nav--item  primary_nav--trigger">
        <a href="/about" class="primary_nav--link">About</a>

        <ul class="primary_nav--sub_nav">
            <li><a href="/about/products">Products</a></li>
            <li><a href="/about/company">Company</a></li>
        </ul>

    </li>

    <li class="primary_nav--item">
        <a href="/contact" class="primary_nav--link">Contact</a>
    </li>

</ul>
```
Isso permite aos desenvolvedores detectar partes separadas do DOM de relance, e também permite que certos editores de texto como o Vim, por exemplo, manipular blocos delimitados com quebras de linhas da marcação.


## Recomendações gerais

> EVITAR AO MÁXIMO o uso de ID's, dando preferência à utilização de classes para fim de componentização e reutilização.

> EVITAR AO MÁXIMO a definição de regras muito específicas. Com a utilização do SASS, não escrever regras¹  aninhadas com mais de dois níveis.

> Desenvolvimento voltado para o FRONT END tem o mesmo princípio do desenvolvimento voltado para o BACK END.
   Antes de codificar PARE, modele o problema, faça um protótipo de baixo nível da tela (caso ainda não haja) e separe-a
   em componentes, de preferência já nomeando classes de css e planejando toda estrutura. Os componentes mais reusáveis são aqueles nas quais os nomes de classes são independentes do conteúdo.

> Evite o uso de !important. Somente use-o de forma proativa e não reativa.

### ¹Especificidade pode, entre outras coisas:

- limit your ability to extend and manipulate a codebase;
- interrupt and undo CSS’ cascading, inheriting nature;
- cause avoidable verbosity in your project;
- prevent things from working as expected when moved into different environments;
- lead to serious developer frustration.

Simple changes to the way we work include, but are not limited to,

- not using IDs in your CSS;
- not nesting selectors;
- not qualifying classes;
- not chaining selectors.

## REFERÊNCIAS

- https://medium.com/@shankarcabus/css-escalavel-parte-1-41e7e863799e
- https://medium.com/@shankarcabus/css-escalavel-parte-2-acb9f0144c9d
- https://medium.com/@shankarcabus/css-escalavel-parte-3-b970ae49acb7
- https://medium.com/@shankarcabus/css-escalavel-parte-final-ff845a62ec4a
- https://medium.com/@mjtweaver/the-css-that-you-dont-know-about-d5945cea1c94
- http://nomadev.com.br/oocss-revisado-suissa-version/
- http://nicolasgallagher.com/about-html-semantics-front-end-architecture/
- https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06
- https://medium.com/user-experience-design-1/o-divorcio-entre-o-psd-e-o-html-70d78279acd1
- http://bradfrostweb.com/blog/post/atomic-web-design/
- http://cssguidelin.es/
- http://csswizardry.com/2013/05/scope-in-css/
- http://csswizardry.com/2011/09/writing-efficient-css-selectors/
- http://csswizardry.com/2011/09/the-nav-abstraction/
- http://csswizardry.com/2011/09/when-using-ids-can-be-a-pain-in-the-class/
- http://smacss.com/book/applicability
- http://csswizardry.com/2014/03/naming-ui-components-in-oocss/
- http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/
- http://tableless.com.br/guia-de-estilos/#.UcOkIPagkR4
- http://tableless.com.br/o-que-e-design-atomic/
- http://daverupert.com/2013/04/responsive-deliverables/
- http://vimeo.com/channels/beyondtellerrand/67476280
- http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml
- https://github.com/styleguide/css
- http://www.smashingmagazine.com/2008/05/02/improving-code-readability-with-css-styleguides/
- http://css-tricks.com/css-style-guides/
- https://speakerdeck.com/hagenburger/a-vision-for-style-guides-in-2015