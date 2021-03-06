
# **Instalação**

React é flexível e pode ser usado em diversos projetos. Você pode criar novos aplicativos com ele, mas você também pode introduzi-lo gradualmente em uma base de código existente sem fazer uma reescrita.

Aqui estão algumas maneiras de começar:

## **Testando o React**  
  
Se você está apenas interessado em testar e brincar com React, você pode usar o Codepen. Tente começar deste código de exemplo de [hello world](https://codepen.io/gaearon/pen/rrpgNB?editors=0010). Você não precisa instalar nada, você pode apenas modificar o código e ver se funcionou.

Se você preferir usar seu próprio editor de código, você pode fazer o [download deste html](https://raw.githubusercontent.com/reactjs/reactjs.org/master/static/html/single-file-example.html), editá-lo, e abri-lo a partir do sistema de arquivos local em seu navegador. Essa maneira tem uma performance mais lenta, então não use em produção.

Se você quiser utilizá-lo para uma aplicação completa, existem duas maneiras populares para iniciar com React: usando Create React App, ou adicionando-o a uma aplicação existente.

## **Criando uma nova aplicação**  
[Create react app](https://github.com/facebookincubator/create-react-app) é a melhor maneira para iniciar a construção de uma nova single page application. Ele configura automaticamente para você o ambiente de desenvolvimento para que você possa utilizar as últimas features do JavaScript, proporcionando uma boa experiência de desenvolvimento e otimizando seu app para produção. Você irá precisar ter o Node >= 6 na sua máquina.

```
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

Create React App não manipula lógica backend ou de banco de dados; ele apenas cria um modelo front end, assim, você pode utilizá-lo com qualquer backend que desejar. 
Ele utiliza ferramentas de compilação como Babel e Webpack por baixo dos panos, mas funciona sem que seja necessária fazer nenhuma configuração.

Quando estiver pronto para realizar o deploy, executar npm run build criará uma compilação otimizada do seu aplicativo na pasta build. Você pode aprender mais sobre o Create React App a partir do [seu README](https://github.com/facebookincubator/create-react-app#create-react-app-) e do [Guia do Usuário](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#table-of-contents)

## **Adicionando o React a uma aplicacao existente**

Você não precisa reescrever seu app para começar a usar o React.

Nós recomendamos adicionar o React a uma pequena parte da sua aplicação, como um widget individual, para que você veja se ele funciona bem para o seu caso de uso.
 
Enquanto o React [pode ser usado](https://reactjs.org/docs/react-without-es6.html) sem um pipeline de compilação, recomendamos configurá-lo para que você possa ser mais produtivo. Um pipeline de construção moderna normalmente consiste em:

Um **Controlador de pacotes**, como [yarn](https://yarnpkg.com/pt-BR/) ou [npm](https://www.npmjs.com/). Ele permite que você aproveite um vasto ecossistema de pacotes de terceiros e os instale ou os atualize facilmente.

Um **bundler**, como o [webpack](https://webpack.js.org/) ou [Browserify](http://browserify.org/). Ele permite escrever código modular e agrupá-lo em pequenos pacotes para otimizar o tempo de carregamento.

Um **compilador** como o [Babel](http://babeljs.io/). Ele permite que você escreva código JavaScript moderno que ainda funciona em navegadores mais antigos.

## **Instalando o React**
**nota:**

* Uma vez instalado, recomendamos que configure um [processo de compilação para produção](https://reactjs.org/docs/optimizing-performance.html#use-the-production-build) para garantir que você esteja usando a versão mais rápida do React em produção.


Nós recomendamos usar o [Yarn](https://yarnpkg.com/pt-BR/) ou [Npm](https://www.npmjs.com/) para gerenciamento de dependências front-end. Se você for iniciante com pacotes de gerenciamento, a [documentação do Yarn](https://yarnpkg.com/en/docs/getting-started) é um ótimo lugar para aprender mais.

Para instalar o React com Yarn, execute:

```
yarn init
yarn add react react-dom
```

Para instalar o React com npm, execute

```
npm init
npm install --save react react-dom
```

Ambos os pacotes, yarn e npm, fazem download do [registro npm](http://npmjs.com/).

## **Habilitando ES6 e JSX**

Nós recomendamos usar o react com o [Babel](http://babeljs.io/) para que você possa utilizar o ES6 e JSX no seu código JavaScript. ES6 é um conjunto de características do JavaScript que torna o desenvolvimento mais fácil, e JSX é uma extensão da linguagem JavaScript que trabalha muito bem com React.

O [setup de instruções do Babel](https://babeljs.io/docs/setup/) explica como configurar o Babel em diferentes ambientes de desenvolvimento. Certifique-se de instalar o [babel-preset-react](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-) e o [babel-preset-env](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-) e ativá-los no seu [.babelrc configuration](http://babeljs.io/docs/usage/babelrc/) e você estará pronto para iniciar.

## **Olá Mundo com ES6 e JSX**

Nós recomendamos utilizar um empacotador como o [webpack](https://webpack.js.org/) ou [browserify](http://browserify.org/) para que você possa escrever códigos modulares e empacotá-los juntos em pequenos pacotes para otimizar o tempo de carregamento.

O menor exemplo React fica assim:

```
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

Este código renderiza em um elemento do DOM com o id root. Então você precisará da ```<div id=”root”></div>``` em algum lugar do seu arquivo HTML.

Da mesma forma, você pode processar um componente React dentro de um elemento DOM em algum lugar dentro do seu aplicativo existente escrito com qualquer outra biblioteca JavaScript.

[Aprenda mais sobre integração de código existente com o React](https://reactjs.org/docs/integrating-with-other-libraries.html#integrating-with-other-view-libraries)

## **Versões de Desenvolvimento e Produção**

Por padrão, React inclui muitos avisos úteis. Esses avisos são muito úteis no desenvolvimento.

**No entanto, tais avisos fazem com que a versão de desenvolvimento do React seja maior e mais lenta, assim, você deveria utilizar a versão de produção quando for realizar o deploy do seu app.**
 
[Aprenda a saber se seu site está utilizando a versão correta do React](https://reactjs.org/docs/optimizing-performance.html#use-the-production-build) e como configurar o processo de construção de produção de forma mais eficiente:

* [Creating a Production Build with Create React App](https://reactjs.org/docs/optimizing-performance.html#create-react-app)
* [Creating a Production Build with Single-File Builds](https://reactjs.org/docs/optimizing-performance.html#single-file-builds)
* [Creating a Production Build with Brunch](https://reactjs.org/docs/optimizing-performance.html#brunch)
* [Creating a Production Build with Browserify](https://reactjs.org/docs/optimizing-performance.html#browserify)
* [Creating a Production Build with Rollup](https://reactjs.org/docs/optimizing-performance.html#rollup)
* [Creating a Production Build with webpack](https://reactjs.org/docs/optimizing-performance.html#webpack)

## **Usando CDN**
Se você não quiser utilizar o npm para gerenciar os pacotes clientes, os pacotes react e react-dom também fornecem distribuições single-file, que são hospedadas nas CDN:

```
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
```

As versões acima apenas são destinadas ao desenvolvimento e não são adequadas para a produção. As versões de produção minimizadas e otimizadas do React estão disponíveis em:

```
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

Para carregar uma versão específica do react e react-dom, substitua 16 pelo número da versão.
Se você usar o Bower, o React está disponível através do pacote react.

## **Porque o atributo crossorigin?**
Se você utilizar o React da CDN recomendamos manter o atributo [crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) definido:

```  <script crossorigin src="..."></script> ```

Também recomendamos verificar se o CDN que você está usando define o Access-Control-Allow-Origin: * cabeçalho HTTP:

Isso permite uma melhor experiência de [gerenciamento de erros](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html) no React 16 e posterior.

#### **Próximo capítulo**:  [Olá Mundo](./ola-mundo.md).

[versão original desta pagina](https://reactjs.org/docs/installation.html)
