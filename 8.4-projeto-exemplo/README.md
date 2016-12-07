# Projeto de finalização de minicurso

- hoje devemos fazer
  - o repositório do projeto (tem que inventar um nome)
  - a interpretação da especificação
  - a criação do migrate inicial
  - a criação das rotas de serviço

## O problema

- não é mistério que houveram problemas com a reserva da sala nas **duas** edições do curso
- *casa de ferreiro, espeto de pau*
- crie um projeto de reserva de espaços

## Requisitos funcionais da solução

- devo ser capaz de:
  - listar e cadastrar espaços
  - listar e cadastrar pessoas
  - listar e cadastrar reservas
- nesta etapa não preciso:
  - sistema de autenticação
  - verificação de conflito de reservas
- pessoa tem no mínimo nome
- espaço tem pelo menos descrição
- reserva deve indicar a pessoa, o espaço, e a marcação de data e hora de início e de término da reserva

## Requisitos não-funcionais da solução

- crie um repositório no github só pra esse projeto
  - o nome do projeto deverá ser sistema-de-reservas-hello-js
    - não é um fork, é um projeto seu
- você deve utilizar tecnologias frontend e backend vistas durante o curso
- o gerenciamento do banco de dados deve usar knex migrations
- o controle do backend deve utilizar o express
- o controle do frontend deve utilizar:
  - angularjs
  - angular-route
- você deve decidir se:
  - terá um projeto único contendo backend e frontend dentro da pasta public
  - dois projetos cada um contendo a parte que lhe cabe

- That's all

## Gabarito da SPA com bower

- no gabarito vamos criar o projeto do zero, pra facilitar a compreensão da montagem

```bash
mkdir hellobowerspa
cd hellobowerspa
bower init
bower install angular-route
touch index.html
touch main.js
touch tela1.html
touch tela2.html
touch tela1.js
touch tela2.js
```

- o index.html passa a ter um ponto de montagem e a referenciar os scripts por tags separadas

```html
<!DOCTYPE html>
<!-- index.html -->
<html>
  <head>
    <title>Hello Bower</title>
    <script type="text/javascript" src="bower_components/angular/angular.js"></script>
    <script type="text/javascript" src="bower_components/angular/angular-route.js"></script>
  </head>
  <body>
    <div ng-app="hellobower">
      <div ng-view></div>
    </div>
    <script type="text/javascript" src="main.js"></script>
    <script type="text/javascript" src="tela1.js"></script>
    <script type="text/javascript" src="tela2.js"></script>
  </body>
</html>
```

- no main.js temos a definição do módulo

```javascript
angular.module("hellobower",["ngRoute"]);
```

- no tela1.js

```javascript
// tela1.js
function Tela1Controller(){
  this.comp1=" do SPA no controle 1";
}
angular.module("hellobower").config( ($routeProvider) => {
  $routeProvider.when("/tela1", {
    controller:Tela1Controller,
    templateUrl:"tela1.html",
    controllerAs:"ctl"
  });
});
```

- no tela2.js

```javascript
// tela2.js
function Tela2Controller(){
  this.comp2=" do SPA no controle 2";
}
angular.module("hellobower").config( ($routeProvider) => {
  $routeProvider.when("/tela1", {
    controller:Tela2Controller,
    templateUrl:"tela1.html",
    controllerAs:"ctl"
  });
});
```

- tela1.html

```html
<!-- tela1.html -->
<h1>Tela 1</h1>
<a href="#/tela2">Ir para a tela 2 {{ctl.comp1}}</a>
```

- tela2.html

```html
<!-- tela2.html -->
<h1>Tela 2</h1>
<a href="#/tela1">Ir para a tela 1 {{ctl.comp2}}</a>
```

[Voltar](../README.md)
