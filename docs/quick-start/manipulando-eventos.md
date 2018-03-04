# Manipulando Eventos

A manipulação de eventos com elementos React é muito similar à manipulação de eventos em elementos do DOM. Há apenas algumas diferenças sintáticas:

* Eventos em React são nomeados utilizando-se camelCase, ao invés de letras minúsculas.
* Com JSX você passa uma função como o manipulador de eventos, ao invés de uma string.

Por exemplo, o HTML:

```
  <button onclick="activateLasers()">
    Activate Lasers
  </button>
```

é levemente diferente em React:

```
  <button onclick={activateLasers}>
    Activate Lasers
  </button>
```

Outra diferença é que você não pode retornar `false` para evitar comportamentos padrão em React. Você deve chamar explicitamente `preventDefault`. Por exemplo, com HTML puro, para evitar o comportamento padrão que links possuem de abrir uma nova página, você pode escrever:

```
  <a href="#" onclick="console.log('The link was clicked.'); return false">
    Click me
  </a>
```

Em React, esse código poderia ser:

```
  function ActionLink() {
    function handleClick(e) {
      e.preventDefault();
      console.log('The link was clicked.');
    }

    return (
      <a href="#" onClick={handleClick}>
        Click me
      </a>
    );
  }
```

Aqui, `e` é um evento sintético. React define esses eventos sintéticos de acordo com as [especificações do W3C](https://www.w3.org/TR/DOM-Level-3-Events/), então você não precsa se preocupar com compatibilidade entre navegadores. Veja o guia de referência de [Eventos Sintéticos](#) para aprender mais.

Utilizando-se React você geralmente não precisa chamar `addEventListener` para adicionar ouvintes a um elemento do DOM após a sua criação. Ao invés disso, apenas forneça um ouvinte quando o elemento for inicialmente renderizado.

Quando você define um componente utilizando uma [classe do ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), um padrão comum é fazer um manipulador de eventos sendo um método na classe. Por exemplo, este componente `Toggle` renderiza um botão que permite ao usuário alternar entre os estados "ON" e "OFF":

```
  class Toggle extends React.Component {
    constructor(props) {
      super(props);
      this.state = {isToggleOn: true};

      // Este confinamento é necessário para fazer com que `this` funcione no callback
      this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
      this.setState(prevState => ({
        isToggleOn: !prevState.isToggleOn
      }));
    }

    render() {
      return (
        <button onClick={this.handleClick}>
          {this.state.isToggleOn ? 'ON' : 'OFF'}
        </button>
      );
    }
  }

  ReactDOM.render(
    <Toggle />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)

Você deve tomar cuidado com o significado de `this` em callbacks JSX. Em JavaScript, métodos de classes não são [confinados](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) por padrão. Se você esquecer de confinar `this.handleClick` e passá-lo para `onClick`, `this` será `undefined` quando a função for efetivamente chamada.

Este não é um comportamento específico do React; é parte de [como funcionam as funções em JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Geralmente, se você se referir a um método sem `()` após ele, assim como em `onClick={this.handleClick}`, você deveria confinar esse método.

Se a chamada a `bind` o incomoda, há duas formas que você pode contornar o problema. Se você estiver utilizando a [sintaxe de campos de classe pública](https://babeljs.io/docs/plugins/transform-class-properties/), você pode utilizar os campos da classe para confinar corretamente os callbacks:

```
  class LoggingButton extends React.Component {
    // Esta sintaxe garante que `this` esteja confinado dentro de handleClick.
    // Aviso: esta sintaxe é *experimental*.
    handleClick = () => {
      console.log('this is:', this);
    }

    render() {
      return (
        <button onClick={this.handleClick}>
          Click me
        </button>
      );
    }
  }
```

Esta sintaxe é habilitada por padrão em [Create-React-App](https://github.com/facebookincubator/create-react-app)

Se você não estiver fazendo uso da sintaxe de campos de classe, pode utilizar uma [função de seta](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) no callback:

```
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // Esta sintaxe garante que `this` esteja confinado dentro de handleClick.
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

O problema com esta sintaxe é que um callback diferente é criado a cada vez que o `LoggingButton` for renderizado. Na maior parte dos casos, é isso o que se deseja. No entanto, se este callback for passado como uma prop para componentes mais baixos, esses componentes talvez façam uma re-renderização extra. Geralmente recomendamos o confinamento no construtor ou através da utilização da sintaxe de campos de classe para evitar este tipo de problema de performance.

## Passando Argumentos para Manipuladores de Eventos

É comum que se queira, dentro de um loop, passar um parâmetro extra para um manipulador de eventos. Por exemplo, se id é o ID da linha, os dois códigos abaixo funcionariam:

```
  <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
  <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

As duas linhas acima são equivalentes, e utilizam [funções de seta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) e [Function.prototype.bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind), respectivamente.

Em ambos os casos, o argumento `e` representando o evento React será passado como um segundo argumento após o ID. Com uma função de seta, temos de passá-lo explicitamente, mas com `bind` quaisquer argumentos extras são automaticamente passados adiante.

#### **Capitulo anterior**:  [Estado e Ciclo de Vida](./estado-ciclo-vida.md).

#### **Próximo capítulo**:  [Renderização Condicional](./renderizacao-condicional.md).

[versão original desta pagina](https://reactjs.org/docs/handling-events.html)
