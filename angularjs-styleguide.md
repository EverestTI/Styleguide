#Angular JS Styleguide (em construção)
_Angular JS Styleguide utilizado pela equipe da [Everest TI](http://www.everestti.com.br), ._ 

<br>

Uma abordagem para padronização do desenvolvimento de aplicações no Front end utilizando Angular JS.
Esse guia foi baseado nas abordagens utilizadas por @toddmotto e @john_papa, bem como em vivências do dia a dia.

##Índice 

1. [Recomendações gerais](#recomendacoes-gerais)
2. [IIFE](#iife)
2. [Modules](#modules)
3. [Controllers](#controllers)
4. [Services e factories](#services-e-factories)
5. [Diretivas](#diretivas)
6. [Filtros](#filtros)
7. [Comentários](#comentarios)
8. [Testes](#testes)


##Recomendações gerais

<br>

* Nomenclatura
    
    - Arquivos em ```CamelCase```. Ex: ```UserController.js```
    - Funções que definem Controllers, Factories, Diretivas em ```CamelCase```.               Ex: ```UserController```
    - Demais funções e variáveis em ```camelCase```. Ex: ```firstName```


   

##IIFE

<br>

Empacote componentes AngularJS em _Immediately Invoked Function Expression_ [(IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/).


Por que?:

1. Evita que a declaração de variáveis e funções existam em contexto global por mais tempo que o esperado. <br><br>
2. Ajuda a evitar [colisão de variáveis](http://www.herongyang.com/JavaScript/Function-Variable-Collision-Example.html). <br><br>


__Observação__: A sua utilização é ainda mais importante para quando fazemos a minificação e concatenação dos arquivos para serem servidos em produção, provendo um escopo de varáveis para cada arquivo.


```
/* Evitar */

angular
    .module('app')
    .factory('someFactory', someFactory);
    
    // a função someFactory e adicionada como uma variável global
    function someFactory() { }

```

```
/* 
    Recomendado 
    
    Sem contexto global. O escopo agora está em contexto local.
*/

(function() {

    angular
        .module('app')
        .factory('someFactory', someFactory);
        
        //
        function someFactory() { }

})();

```
[Voltar ao índice](#indice)

##Modules

<br>

Declare os módulos sem usar variáveis, usando a sintaxe de ```getter``` e ```setter```

_Por que?_: Proporciona um código mais legível e evita a [colisão de variáveis](http://www.herongyang.com/JavaScript/Function-Variable-Collision-Example.html)

__Getter vs Setter__ <br>

* Use ```angular.module('app',[]);``` para setar um módulo.
* Use ```angular.module('app');``` para obter um módulo.
  
<br>

```
/* Evitar */

var app = angular.module('app');
app.controller('SomeController' , SomeController);

function SomeController() { }

```

```

/* Recomendado */

angular
    .module('app')
    .controller('SomeController' , SomeController);

function SomeController() { }

```
<br>

__Funções nomeadas vs funções anônimas__: Use funções nomeadas ao invés de funções anônimas como _callback_

_Por que?_: Isso proporciona um código mais legível, mais fácil de _debugar_ e reduz a quantidade _callbacks_ aninhados.

```
/* Evitar */
angular
    .module('app')
    .controller('SomeController', function() { });
    .factory('SomeFactory', function() { });
```

```
/* Recomendado */

// SomeController.js
angular
    .module('app')
    .controller('SomeController', SomeController);

function SomeController() { }

// SomeFactory.js
angular
    .module('app')
    .factory('SomeFactory', SomeFactory);

function SomeFactory() { }
```

[Voltar ao índice](#indice)

##Controllers

<br>

__Sintáxe ControllerAs na View__ :  Use a sintáxe de [ControllerAs](http://toddmotto.com/digging-into-angulars-controller-as-syntax/) ao invés da sintáxe clássica com ```$scope```

_Por que?:_ Promove a utilização do _binding_ na _view_ através de um _alias_, tornando mais contextual e fácil de ler.

```
<!-- Evitar -->
<div ng-controller="UserController">
  {{ name }}
</div>

<!-- Recomendado -->
<div ng-controller="UserController as user">
  {{ user.name }}
</div>
```

<br>

__Sintáxe ControllerAs no controller__ : O _controllerAs_ permite a utilização do ```this``` nos _controllers_ no lugar da utilização clássica do ```$scope```.

```
// Evitar
function UserController ($scope) {
  $scope.name = {};
  $scope.doSomething = function () {

  };
}

// Recomendado
function UserController () {
  this.name = {};
  this.doSomething = function () {

  };
}
```
<br> 

Só utilize o ```$scope``` quando necessário. Por exmplo, se houver a necessidade de utilizar algo que não seja ```data bindindg``` (```$watch```, ```$on```, ```$broadcast```). Mesmo assim, é importante evitar ao máximo essa prática. Sempre que possível deve ser delegado a uma ```factory``` ou ```service```.

_Por que?_: Explicar!

<br>

__ControllerAs com a variável 'vm' (_ViewModel_)__: Use uma variável de captura para o ```this```.

_Por que?_: 

1.   O [```this```](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) é uma palavra-chave contextual e quando usada dentro de uma função, dentro de um _controller_ o seu contexto pode mudar. Capturar o contexto do ```this``` dentro de uma variável evita que isso ocorra. <br><br> 
2.  O ```$scope``` funciona como uma "cola" entre o ```model``` e a ```view```([_ViewModel_](http://en.wikipedia.org/wiki/Model_View_ViewModel)), portando, a atribuição do ```this``` na variável ```vm```, além de ser mais curta, torna o código mais legível, tornando tudo que é ```"bindable"``` contextual. <br><br> 
3.  Na maioria das vezes é possível evitar uma injeção de dependência (```$scope```) desnecessária. <br><br> 
4.  O uso demasiado do ```$scope``` deixa o código "sujo", atrapalhando a legibilidade. <br><br> 


```
/* Evitar */
function Customer() {
    this.name = {};
    this.sendMessage = function() { };
}
```

```
/* Recomendado */
function Customer() {
    var vm = this;
    vm.name = {};
    vm.sendMessage = function() { };
}
```

<br>

__Não coloque lógica de negócio (regras de negócio) no _controller_ __: Delege a lógica de negócio para ```services``` ou ```factories```/

<br>

_Por que?_: 

1. A lógica pode ser usada por mais de um _controller_ <br><br>
2. A lógica pode ser facilmente isolada em um teste unitário enquanto a chamada no _controller_ pode ser facilmente _mockada_. <br><br>
3. Remove dependências e oculta detalhes de implementação do controller. <br><br>


```
/* Evitar */
function User($http, $q) {
    var vm = this;
    vm.checkCpf = checkCpf;
    

    function checkCpf() {         
        return $http.get('api/cpfCheckService')
        .success(function(data){  })
        .error(function(error) {  });
    };
}
```

```
/* Recomendado */
function User(cpfCheckService) {
    var vm = this;
    vm.checkCpf = checkCpf;
    
    function checkCpf() { 
       return creditService.check();
    };
}
```
[Voltar ao índice](#indice)


##Services

Utilize ```services``` para funções utilitárias compartilhadas.

```
  function ArrayUtilService () {
    this.groupBy = function (array, f) {

    };
  }
  angular
    .module('app')
    .service('ArrayUtilService', ArrayUtilService);
```


_Por que?__: Explicar! (TODO)


[Voltar ao índice](#indice)

#Factories

Utilize ```factories``` para a lógica de negócios da aplicação.

__"API pública" no topo__: Exponha todas as suas funções públicas no topo, usando uma técnica derivada do Revealing Module Pattern.

_Por que?_: Colocar as funções acessíveis publicamente no topo torna mais fácil a identificação podem e devem ser consumidos e testados.

```
/* Evitar  */
function SomeFactory() {
  function save() { 
    /* */
  };
  function delete() { 
    /* */
  };

  return {
      save: save,
      delete: delete
  };
}
```
```
/* Recomendado */
function SomeFactory() {
    var someFactory = {
        save: save,        
        delete: delete
    };
    return someFactory;


    function save() { 
    };

    function delete() { 
    };
}
```

[Voltar ao índice](#indice)

##Diretivas

Em breve...

[Voltar ao índice](#indice)

##Filters

Em breve...

[Voltar ao índice](#indice)

##Comentários

Use a sintáxe do [jsDoc](http://jsdoc.org) para comentar o código. 

_Por que?_: Dessa forma podemos gerar uma documentação para nosso código, ao invés de escrevê-la do zero.

```
/**
 * @name SomeService
 * @desc Main application Controller
 */
function SomeService (SomeService) {

  /**
   * @name doSomething
   * @desc Does something awesome
   * @param {Number} x - First number to do something with
   * @param {Number} y - Second number to do something with
   * @returns {Number}
   */
  this.doSomething = function (x, y) {
    return x * y;
  };

}
angular
  .module('app')
  .service('SomeService', SomeService);  
```

[Voltar ao índice](#indice)

##Testes

[Voltar ao índice](#indice)

##Licença de uso

(The MIT License)

Copyright (c) 2014 Everest TI

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.