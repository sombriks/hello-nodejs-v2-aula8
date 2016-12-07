# Prototipação rápida com o Budo

- 5 minutos

## Definição

- O browserify é lindo e cheiroso, mas a etapa de compilação enche o saco
- com o [budo](https://github.com/mattdesl/budo) isso tudo fica automático

## Aplicação

- prototipação rápida
- é que nem o nodemon/forever, só que pro projeto front-end

## Exemplo

- no [projeto anterior](../8.2-browserify/README.md) execute o seguinte comando

```bash
budo src/main.js:build.js --live --open
```

- a saída deverá ser algo do tipo

```bash
[sombriks@sephiroth hellobrowserify]$ budo src/main.js:build.js --live --open
[0003] info  Server running at http://192.168.0.109:9966/ (connect)
[0003] info  LiveReload running on 35729
[0004] 1171ms     3.2MB (browserify)
[0004] 10ms        365B GET    200 /
[0004] 2ms        3.2MB GET    200 /build.js
[0004] 2ms          90B GET    200 /tela1.html
[0006] 3ms          90B GET    200 /tela2.html
```

- além de abrir o navegador pra você, temos o bônus do reload
- modifique o **tela1.js** para o seguinte

```javascript
// src/tela1.js
function Tela1Controller(){
  this.dados1 = "na SPA de browserify com hot reload";
}

module.exports = {
  controller:Tela1Controller,
  templateUrl:"tela1.html",
  controllerAs:"ctl"
};
```

- se a máquina for veloz, você pode não ver a página recarregando automaticamente
- confira o console pra ver que os arquivos foram recarregados

### Bônus: npm scripts

- seu **package.json** deve estar mais ou menos assim

```json
{
  "name": "hellobrowserify",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "angular": "^1.5.9",
    "angular-route": "^1.5.9"
  }
}
```

- a seção scripts pode receber comando úteis
- o jargão dos desenvolvedores node apregoa os scripts **build** e **dev**
- eles não são mandatórios, tanto que nem estão aí
- mas eles são de uso comum e você verá outros projetos usando eles

```json
{
  "name": "hellobrowserify",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "budo src/main.js:build.js --live --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "angular": "^1.5.9",
    "angular-route": "^1.5.9"
  }
}
```

- agora você pode executar o budo de modo indireto

```bash
npm run dev
```

- não é ***sensacional?***

### Bônus 2: cabeçalhos CORS

- suponha que seu projeto backend seja totalmente separado do seu projeto frontend
  - *foobarapp-client* e *foobarapp-service*
- um serviço angular no app cliente poderia ter a seguinte forma:

```javascript
// sampleservice.js
function Servicecontatos($http){
  this.listcontatos (params) => $http.get("contatos/list",{
    params:params
  });
}
module.exports = Servicecontatos;
```

- se você tiver um backend e estiver servindo o app cliente via **express.static("public")**, all good!
  - exceto pelo setup com um package.json na raíz e outro package.json dentro da pasta **public**
- se você tiver dois projetos, o serviço acima não funcionará, pois o backend estará rodando em outra porta
  - mudou a porta, conta como um servidor inteiramente novo
- para solucionar isso, primeiro mudando a chamada no serviço

```javascript
// sampleservice.js
function Servicecontatos($http){
  this.listcontatos (params) => $http.get("http://127.0.0.1:3000/contatos/list",{
    params:params
  });
}
module.exports = Servicecontatos;
```

- em seguida, precisamos habilitar o [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) no serviço

```javascript
// trecho do seu index.js no backend

// ...

app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

// ...
```

- e assim você terá dois projetos rodando sem maiores traumas.

## Exercício

- crie um script **build** no **package.json** que chame a "compilação" do **src/main.js** pelo **browserify**
- comite estas alterações também

[Voltar](../README.md)