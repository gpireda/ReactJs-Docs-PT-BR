# **Olá Mundo**

A maneira mais fácil de iniciar com React é usando [este exemplo de olá mundo no codepen](https://codepen.io/gaearon/pen/ZpvBNJ?editors=0010). Você não precisa instalar nada; você pode apenas abrir em uma nova aba e acompanhar conforme seguimos com os exemplos. 
Se preferir usar um ambiente de desenvolvimento local, confira a [página Instalação](./instalacao.md).

Veja o menor exemplo de um código React:
```
ReactDOM.render(
  <h1>Olá, Mundo!</h1>,
  document.getElementById('root')
);
```
Esse código renderiza um título dizendo "Olá Mundo!" na página.

As próximas sessões irão gradualmente ensiná-lo a usar o React. Nós iremos examinar os blocos básicos de código de aplicações React: Elementos e Componentes. Uma vez que você esteja familiarizado, você poderá criar aplicações complexas a partir de pedaços reutilizáveis.

## **Uma observação sobre JavaScript**

React é uma biblioteca JavaScript, então é esperado que você tenha um conhecimento básico da linguagem. Se você não se sente confiante, nós recomendamos [aprimorar seu conhecimento em JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) para que possa seguir com o React sem dificuldade.

Nós também utilizamos algumas das sintaxes do ES6 nos exemplos. Nós tentamos utilizar com moderação pois ainda é relativamente novo, mas nós incentivamos você a se familiarizar com declarações de [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals), [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) e [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const). Você pode usar o [Babel REPL](http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A) para verificar o código ES6 compilado.

#### **Capitulo anterior**:  [Instalação](./instalacao.md).

#### **Próximo capítulo**:  [Introdução ao JSX](./introducao-jsx.md).

[versão original desta pagina](https://reactjs.org/docs/hello-world.html)
