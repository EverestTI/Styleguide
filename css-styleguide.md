
Css Styleguide
============

Bem-vindo ao Everestti CSS Styleguide.

# Índice

1. [Princípios](#recomendacoes-gerais)
2. [Patterns](#iife)
2. [Título de seções](#modules)
3. [Espaços em branco:](#controllers)
4. [Services e factories](#services-e-factories)
5. [Diretivas](#diretivas)
6. [Filtros](#filtros)
7. [Comentários](#comentarios)
8. [Testes](#testes)

## Coding style
### Espaços:
### Formatação:
### Diversos:
### Exemplos:

## Princípios
- Separar a estrutura do skin (cores, fontes, bordas, etc.)
- Separe os containers do conteúdo

## -- Padrões de escrita --
### • Identação
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

### • 80 caracteres de largura
```
/**
 * I am a long-form comment. I describe, in detail, the CSS that follows. I am
 * such a long comment that I easily break the 80 character limit, so I am
 * broken across several lines.
 */
```

### • Titulando
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
#### • Espaços em branco
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

### • Multi-line CSS
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

## • Meaningful use of whitespace

## • Convenções de nomenclatura de classes
##### - CSS
- [ _ ] Hífen: Separa palavras compostas
    - `<div class="guarda_roupa"></div>`
- [ - ] Traço: Elemento / Filho
    - `<div class="guarda_roupa-gaveta"></div>`
- [ __ ] Dois hífens: Modificador de Estado
    - `<div class="guarda_roupa-gaveta__aberta"></div>`


##### - JS
Em elementos que forem receber alguma manipulação via javascript, não vincular à mesma classe ultilizada no CSS. Dessa forma, caso você queria refatorar seu CSS não precisará alterar o JS. Para isso utilize o prefixo `js-`, por exemplo:
```
<input type="submit" class="btn js-btn" value="Submit" />
```
## • Especificidade (classes vs. ids)
Elementos que ocorrem exatamente uma vez dentro de uma página devem usar IDs, caso contrário, use classes. Quando em dúvida, use um nome de classe.<br>
> Somente atribuir um ID para um elemento inline se for necessário referenciá-lo em um javascript ou jQuery.

- **Bons** candidatos para ids: cabeçalho, rodapé, popups, modais.
- **Maus** candidatos para ids: navegação, listas de itens, botões.

<br>
___
<br><br><br>


> Written with [StackEdit](https://stackedit.io/).