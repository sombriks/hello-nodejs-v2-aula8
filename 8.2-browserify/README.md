# Introdução ao [Browserify](http://browserify.org/)

- 15 minutos

## Definição

- Nascido em 2014
- Ferramenta de *build* do projeto
  - Faz algo parecido com o que o [webpack](https://webpack.github.io/) faz, mas sem as burocracias
    - entender o browserify ajudará a entender qualquer outro build system, inclusive o webpack
  - Suporta *plugins* e *transforms*
  - Faz apenas uma coisa e faz esta uma coisa muito bem
- Simples proposta de programar para a web como se estivéssemos programando para o node.js
- ***Compila*** seus scripts e módulos para um arquivo de saída

## Aplicação

- Organização mais civilizada dos códigos do sistema
- Download único dos scripts do sistema
- Permite, *hipoteticamente*, uma mesma base de código compartilhada entre o back-end e o front-end
  - Na **prática** as pessoas já cansaram de sofrer fazem dois projetos, um pro front e outro pro back

## Exemplo

- faça um novo projeto chamado hellobrowserify

```bash
mkdir hellobrowserify
cd hellobrowserify
npm init -y
```

- instale o angular agora
- crie o index.html vazio

```bash
npm install angular --save
touch index.html
```

- nosso index.html será um pouco diferente

```html
<!DOCTYPE html>
<!-- index.html -->
<html>
  <head>
    <title>Hello Browserify</title>
  </head>
  <body>
    <div ng-controller="Foo as ctl">
      <h1>Angular from {{ctl.dependecymanager}}!</h1>
    </div>
    <script type="text/javascript" src="build.js"></script>
  </body>
</html>
```

- não temos e nem faremos referência às bibliotecas que baixarmos
- o **build.js** não deve ser criado *ainda*
- crie uma pasta chamada src e dentro dela um script chamando de main.js

```bash
mkdir src
touch src/main.js
```

```javascript
// src/main.js
const angular = require("angular");

angular.module("hellobrowserify",[]);
angular.module("hellobrowserify").controller("Foo", function(){
  this.dependecymanager = "Browserify";
});
angular.bootstrap(document,["hellobrowserify"]);

```

- o código apresentado procura inocentemente fazer **require**, como se estivéssemos rodando no node
- para tornar isso possível, vamos ao browserify:

```bash
browserify src/main.js -o build.js
```

- nosso index.html tem agora o build.js que prometemos.
- note o passo adicional de "*compilação*" introduzido
- o grande bônus é mexer o mínimo possível no html e poder criar módulos javascript gerenciados no javascript
- Uma [SPA](https://en.wikipedia.org/wiki/Single-page_application) com browserify já apresentaria vantagens, veja abaixo

```bash
npm install angular-route --save
touch tela1.html
touch tela2.html
touch src/tela1.js
touch src/tela2.js
```

- uma vez instalado o router e criados os arquivos adicionais, modifique o index.hml

```html
<!DOCTYPE html>
<!-- index.html -->
<html>
  <head>
    <title>Hello Browserify</title>
  </head>
  <body>
    <div ng-view>
      <!-- dynamic content goes here -->
    </div>
    <script type="text/javascript" src="build.js"></script>
  </body>
</html>
```

- os fragmentos de tela

```html
<!-- tela1.html -->
<h1>Tela 1</h1>
<a href="#/tela2">Ir para a tela 2 {{ctl.dados1}}</a>
```

```html
<!-- tela2.html -->
<h1>Tela 1</h1>
<a href="#/tela1">Ir para a tela 1 {{ctl.dados2}}</a>
```

- modifique o src/main.js

```javascript
// src/main.js
const angular = require("angular");

angular.module("hellobrowserify", [
  require("angular-router")
]);

// roteamento
angular.module("hellobrowserify").config(($routeProvider) => {
  $routeProvider.when("/tela1", require("./tela1"));
  $routeProvider.when("/tela2", require("./tela2"));
  $routeProvider.otherwise("/tela1");
});

angular.bootstrap(document,["hellobrowserify"]);

```

- por fim, os scripts das telas

```javascript
// src/tela1.js
function Tela1Controller(){
  this.dados1 = "na SPA de browserify";
}

module.exports = {
  controller:Tela1Controller,
  templateUrl:"tela1.html",
  controllerAs:"ctl"
};
```

```javascript
// src/tela2.js
function Tela2Controller(){
  this.dados2 = "na SPA de browserify";
}

module.exports = {
  controller:Tela2Controller,
  templateUrl:"tela2.html",
  controllerAs:"ctl"
};
```

## Exercício

- pergunta: podemos usar bower e npm no mesmo projeto? O que você acha?
- comite os projetos de exemplo feitos até agora
- crie um repositório no git para cada projeto

[Voltar](../README.md)
