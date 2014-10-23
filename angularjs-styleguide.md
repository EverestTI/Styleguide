#Angular JS Styleguide (em constru��o)
_Angular JS Styleguide utilizado pela equipe da [Everest TI](http://www.everestti.com.br), ._ 

<br>

Uma abordagem para padroniza��o do desenvolvimento de aplica��es no Front end utilizando Angular JS.
Esse guia foi baseado nas abordagens utilizadas por @toddmotto e @john_papa, bem como em viv�ncias do dia a dia.

##�ndice 

1. [Recomenda��es gerais](#recomendacoes-gerais)
2. [IIFE](#iife)
2. [Modules](#modules)
3. [Controllers](#controllers)
4. [Services e factories](#services-e-factories)
5. [Diretivas](#diretivas)
6. [Filtros](#filtros)
7. [Coment�rios](#comentarios)
8. [Testes](#testes)


##Recomenda��es gerais

<br>

* Nomenclatura
    
    - Arquivos em ```CamelCase```. Ex: ```UserController.js```
    - Fun��es que definem Controllers, Factories, Diretivas em ```CamelCase```.               Ex: ```UserController```
    - Demais fun��es e vari�veis em ```camelCase```. Ex: ```firstName```


   

##IIFE

<br>

Empacote componentes AngularJS em _Immediately Invoked Function Expression_ [(IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/).


Por que?:

1. Evita que a declara��o de vari�veis e fun��es existam em contexto global por mais tempo que o esperado. <br><br>
2. Ajuda a evitar [colis�o de vari�veis](http://www.herongyang.com/JavaScript/Function-Variable-Collision-Example.html). <br><br>


__Observa��o__: A sua utiliza��o � ainda mais importante para quando fazemos a minifica��o e concatena��o dos arquivos para serem servidos em produ��o, provendo um escopo de var�veis para cada arquivo.


```
/* Evitar */

angular
    .module('app')
    .factory('someFactory', someFactory);
    
    // a fun��o someFactory e adicionada como uma vari�vel global
    function someFactory() { }

```

```
/* 
    Recomendado 
    
    Sem contexto global. O escopo agora est� em contexto local.
*/

(function() {

    angular
        .module('app')
        .factory('someFactory', someFactory);
        
        //
        function someFactory() { }

})();

```
[Voltar ao �ndice](#indice)

##Modules

<br>

Declare os m�dulos sem usar vari�veis, usando a sintaxe de ```getter``` e ```setter```

_Por que?_: Proporciona um c�digo mais leg�vel e evita a [colis�o de vari�veis](http://www.herongyang.com/JavaScript/Function-Variable-Collision-Example.html)

__Getter vs Setter__ <br>

* Use ```angular.module('app',[]);``` para setar um m�dulo.
* Use ```angular.module('app');``` para obter um m�dulo.
  
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

__Fun��es nomeadas vs fun��es an�nimas__: Use fun��es nomeadas ao inv�s de fun��es an�nimas como _callback_

_Por que?_: Isso proporciona um c�digo mais leg�vel, mais f�cil de _debugar_ e reduz a quantidade _callbacks_ aninhados.

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

[Voltar ao �ndice](#indice)

##Controllers

<br>

__Sint�xe ControllerAs na View__ :  Use a sint�xe de [ControllerAs](http://toddmotto.com/digging-into-angulars-controller-as-syntax/) ao inv�s da sint�xe cl�ssica com ```$scope```

_Por que?:_ Promove a utiliza��o do _binding_ na _view_ atrav�s de um _alias_, tornando mais contextual e f�cil de ler.

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

__Sint�xe ControllerAs no controller__ : O _controllerAs_ permite a utiliza��o do ```this``` nos _controllers_ no lugar da utiliza��o cl�ssica do ```$scope```.

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

S� utilize o ```$scope``` quando necess�rio. Por exmplo, se houver a necessidade de utilizar algo que n�o seja ```data bindindg``` (```$watch```, ```$on```, ```$broadcast```). Mesmo assim, � importante evitar ao m�ximo essa pr�tica. Sempre que poss�vel deve ser delegado a uma ```factory``` ou ```service```.

_Por que?_: Explicar!

<br>

__ControllerAs com a vari�vel 'vm' (_ViewModel_)__: Use uma vari�vel de captura para o ```this```.

_Por que?_: 

1.   O [```this```](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) � uma palavra-chave contextual e quando usada dentro de uma fun��o, dentro de um _controller_ o seu contexto pode mudar. Capturar o contexto do ```this``` dentro de uma vari�vel evita que isso ocorra. <br><br> 
2.  O ```$scope``` funciona como uma "cola" entre o ```model``` e a ```view```([_ViewModel_](http://en.wikipedia.org/wiki/Model_View_ViewModel)), portando, a atribui��o do ```this``` na vari�vel ```vm```, al�m de ser mais curta, torna o c�digo mais leg�vel, tornando tudo que � ```"bindable"``` contextual. <br><br> 
3.  Na maioria das vezes � poss�vel evitar uma inje��o de depend�ncia (```$scope```) desnecess�ria. <br><br> 
4.  O uso demasiado do ```$scope``` deixa o c�digo "sujo", atrapalhando a legibilidade. <br><br> 


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

__N�o coloque l�gica de neg�cio (regras de neg�cio) no _controller_ __: Delege a l�gica de neg�cio para ```services``` ou ```factories```/

<br>

_Por que?_: 

1. A l�gica pode ser usada por mais de um _controller_ <br><br>
2. A l�gica pode ser facilmente isolada em um teste unit�rio enquanto a chamada no _controller_ pode ser facilmente _mockada_. <br><br>
3. Remove depend�ncias e oculta detalhes de implementa��o do controller. <br><br>


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
[Voltar ao �ndice](#indice)


##Services

Utilize ```services``` para fun��es utilit�rias compartilhadas.

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


[Voltar ao �ndice](#indice)

#Factories

Utilize ```factories``` para a l�gica de neg�cios da aplica��o.

__"API p�blica" no topo__: Exponha todas as suas fun��es p�blicas no topo, usando uma t�cnica derivada do Revealing Module Pattern.

_Por que?_: Colocar as fun��es acess�veis publicamente no topo torna mais f�cil a identifica��o podem e devem ser consumidos e testados.

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

[Voltar ao �ndice](#indice)

##Diretivas

Em breve...

[Voltar ao �ndice](#indice)

##Filters

Em breve...

[Voltar ao �ndice](#indice)

##Coment�rios

Use a sint�xe do [jsDoc](http://jsdoc.org) para comentar o c�digo. 

_Por que?_: Dessa forma podemos gerar uma documenta��o para nosso c�digo, ao inv�s de escrev�-la do zero.

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

[Voltar ao �ndice](#indice)

##Testes

[Voltar ao �ndice](#indice)

##Licen�a de uso

(The MIT License)

Copyright (c) 2014 Everest TI

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.