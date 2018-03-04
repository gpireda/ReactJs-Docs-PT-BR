# Renderização Condicional

Em React, você pode criar componentes distintos que encapsulam o comportamento que você precisa. Então, você pode renderizar apenas alguns deles, dependendo do estado da sua aplicação.

Renderização condicional em React funciona da mesma forma que condições funcionam em JavaScript. Utilize operadores JavaScript como [if](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) ou o [operador condicional](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) para criar elementos representando o estado atual, e deixe o React atualizar a UI em conformidade.

Considere estes dois componentes:

```
  function UserGreeting(props) {
    return <h1>Welcome back!</h1>;
  }

  function GuestGreeting(props) {
    return <h1>Please sign up.</h1>;
  }
```

Criaremos um componente `Greeting` que mostra um desses dois componentes dependendo se o usuário está logado:

```
  function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;
    if (isLoggedIn) {
      return <UserGreeting />;
    }
    return <GuestGreeting />;
  }

  ReactDOM.render(
    // Try changing to isLoggedIn={true}:
    <Greeting isLoggedIn={false} />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)

Este exemplo renderiza diferentes saudações dependendo do valor da prop `isLoggedIn`.

## Elementos em Variáveis

Você pode utilizar variáveis para armazenar elementos. Isso pode ajudá-lo a renderizar condicionalmente uma parte do componente enquanto o restante da saída não é alterado.

Considere estes dois novos componentes representando botões de Logout e Login:

```
  function LoginButton(props) {
    return (
      <button onClick={props.onClick}>
        Login
      </button>
    );
  }

  function LogoutButton(props) {
    return (
      <button onClick={props.onClick}>
        Logout
      </button>
    );
  }
```

No exemplo abaixo, criaremos um [componente com estado](estado-ciclo-vida.md#adicionando-estado-local-a-uma-classe) chamado `LoginControl`.

Ele renderizará `<LoginButton />` ou `<LogoutButton />` dependendo do estado atual. Também renderizará um `<Greeting />` do exemplo anterior.

```
  class LoginControl extends React.Component {
    constructor(props) {
      super(props);
      this.handleLoginClick = this.handleLoginClick.bind(this);
      this.handleLogoutClick = this.handleLogoutClick.bind(this);
      this.state = {isLoggedIn: false};
    }

    handleLoginClick() {
      this.setState({isLoggedIn: true});
    }

    handleLogoutClick() {
      this.setState({isLoggedIn: false});
    }

    render() {
      const isLoggedIn = this.state.isLoggedIn;

      let button = null;
      if (isLoggedIn) {
        button = <LogoutButton onClick={this.handleLogoutClick} />;
      } else {
        button = <LoginButton onClick={this.handleLoginClick} />;
      }

      return (
        <div>
          <Greeting isLoggedIn={isLoggedIn} />
          {button}
        </div>
      );
    }
  }

  ReactDOM.render(
    <LoginControl />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

Apesar de se declarar uma variável e utilizar um `if` ser um modo aceitável de se renderizar condicionalmente um componente, às vezes você pode querer utilizar uma sintaxe mais curta. Há algumas formas de se realizar condições em linha no JSX, como explicado abaixo.

## If em Linha com Operador Lógico &&

Você pode [incorporar quaisquer expressões em JSX](./introducao-jsx.md#incorporando-expressões-no-jsx) envolvendo-as com colchetes. Isso inclui o operador lógico JavaScript &&. Isso pode ser útil para se incluir condicionalmente um elemento:

```
  function Mailbox(props) {
    const unreadMessages = props.unreadMessages;
    return (
      <div>
        <h1>Hello!</h1>
        {unreadMessages.length > 0 &&
          <h2>
            You have {unreadMessages.length} unread messages.
          </h2>
        }
      </div>
    );
  }

  const messages = ['React', 'Re: React', 'Re:Re: React'];
  ReactDOM.render(
    <Mailbox unreadMessages={messages} />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

Isso funciona porque, em JavaScript, `true && expressão` sempre é avaliado em `expressão`, e `false && expressão` sempre é avaliado em `false`.

Portanto, se a condição for `true`, o elemento logo após o `&&` aparecerá na saída. Se for `false`, React o ignorará e continuará a execução a partir da próxima linha.

## If-Else em Linha com Operador Condicional

Outro método para renderizar elementos condicionalmente é utilizar o operador condicional JavaScript [condition ? true : false](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

No exemplo abaixo, utilizamos o operador condicional para renderizar condicionalmente um pequeno bloco de texto.

```
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
      <div>
        The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
      </div>
    );
  }
```

Ele também pode ser utilizado para expressões maiores, apesar de tornar menos óbvio o que está acontecendo:

```
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
      <div>
        {isLoggedIn ? (
          <LogoutButton onClick={this.handleLogoutClick} />
        ) : (
          <LoginButton onClick={this.handleLoginClick} />
        )}
      </div>
    );
  }
```

Assim como em JavaScript, cabe a você decidir um estilo apropriado baseado no que você e sua equipe consideram mais legível. Lembre-se também que sempre que condições se tornam muito complexas, talvez seja uma boa hora para [extrair um componente](./componentes-props.md#extraindo-componentes).

## Evitando a Renderização de Componentes

Em raros casos você pode querer que um componente seja escondido, apesar de ter sido renderizado por outro componente. Para fazer isso, retorne `null` ao invés da sua saída normal.

No exemplo abaixo, o `<WarningBanner />` é renderizado dependendo do valor da prop chamada `warn`. Se o valor da prop for `false`, o componente não é renderizado:

```
  function WarningBanner(props) {
    if (!props.warn) {
      return null;
    }

    return (
      <div className="warning">
        Warning!
      </div>
    );
  }

  class Page extends React.Component {
    constructor(props) {
      super(props);
      this.state = {showWarning: true};
      this.handleToggleClick = this.handleToggleClick.bind(this);
    }

    handleToggleClick() {
      this.setState(prevState => ({
        showWarning: !prevState.showWarning
      }));
    }

    render() {
      return (
        <div>
          <WarningBanner warn={this.state.showWarning} />
          <button onClick={this.handleToggleClick}>
            {this.state.showWarning ? 'Hide' : 'Show'}
          </button>
        </div>
      );
    }
  }

  ReactDOM.render(
    <Page />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

Retornar `null` do método `render` de um componente não afeta o disparo dos métodos de ciclo de vida do componente. Por exemplo, `componentWillUpdate` e `componentDidUpdate` ainda serão chamados.

#### **Capitulo anterior**:  [Manipulando Eventos](./manipulando-eventos.md).

#### **Próximo capítulo**:  [Listas e Chaves](./listas-e-chaves.md).

[versão original desta pagina](https://reactjs.org/docs/conditional-rendering.html)
