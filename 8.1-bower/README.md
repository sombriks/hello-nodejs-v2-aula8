# Introdução ao bower

- 10 minutos

## Definição e aplicação

- antigamente o npm era apenas para módulos node.js
- programadores *front-end* desejavam o mesmo benefício
- nasce o [bower](https://bower.io/)
- hoje o npm também gerencia a parte front-end, *mas*
  - "bower vai morrer! dura mais nem um ano!" (circa 2013)
  - "meu projeto tem dois package.json, o que faço agora?" (circa 2014)
  - "bower é bom para resolver os assets do projeto: css, fontes, etc" (2015)
- o bower está marcado para morrer, mas ele é parente do Bruce Willis: [Duro de matar](https://g.co/kgs/cFjSz8)

## Exemplo

- crie um pequeno projeto chamado hellobower

```bash
mkdir hellobower
cd hellobower
bower init
```

- note que usamos **bower init** em vez de **npm init**
- após algumas perguntas você terá um arquivo chamado ~~package.json~~ **[bower.json](https://bower.io/docs/creating-packages/#bowerjson)**
- instale uma dependência:

```bash
bower install angularjs --save
```

- será criado o diretório ~~node_modules~~ **bower_components** e lá você encontrará o angularjs
- crie um **index.html** e adicione a dependência que baixamos:

```html
<!DOCTYPE html>
<!-- index.html -->
<html>
  <head>
    <title>Hello Bower</title>
    <script type="text/javascript" src="bower_components/angular/angular.js"></script>
  </head>
  <body>
    <div ng-app="hellobower">
      <div ng-controller="Foo as ctl">
        <h1>Angular from {{ctl.dependecymanager}}!</h1>
      </div>
    </div>
    <script type="text/javascript">
      angular.module("hellobower",[]);
      angular.module("hellobower").controller("Foo", function(){
        this.dependecymanager = "Bower";
      });
    </script>
  </body>
</html>
```

- em analogia ao que acontece com a pasta **node_modules**, a **bower_components** é gerada automaticamente
- ainda na analogia, é *recomendação de boa etiqueta e modos à mesa* colocar a pasta no **.gitignore**
- o bower não coloca magicamente a tag do script no documento html
- em contrapartida, se um pacote bower depende de outro, o bower baixa pra você

## Exercício

- use o bower para baixar o **angular-router**, visto na aula 7
- modifique o exemplo acima para ser uma SPA com duas telas: **tela1.html** e **tela2.html**

```html
<!-- tela1.html -->
<h1>Tela 1</h1>
<a href="#/tela2">Ir para a tela 2</a>
```

```html
<!-- tela2.html -->
<h1>Tela 2</h1>
<a href="#/tela1">Ir para a tela 1</a>
```

- a rota padrão deverá ser a tela 1
- caso encontre dificuldade, [gabarito](../8.4-projeto-exemplo/README.md) no final da aula

[Voltar](../README.md)
