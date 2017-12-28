# Estado e Ciclo de Vida

Considere o exemplo do relógio de [uma das seções anteriores](./renderizando-elementos.md#atualizando-o-elemento-renderizado).

Até agora apenas aprendemos uma maneira de atualizar a UI.

Nós chamamos ReactDOM.render() para modificar a saída renderizada:

```
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[Experimente no CodePen](http://codepen.io/gaearon/pen/gwoJZk?editors=0010).

Nesta seção, aprenderemos como fazer o componente Clock verdadeiramente reutilizável e encapsulado. Ele configurará seu próprio temporizador e atualizará a si mesmo a cada segundo.

Podemos começar pelo encapsulamento de como o relógio se parece:

```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[Experimente no CodePen](http://codepen.io/gaearon/pen/dpdoYR?editors=0010).

Entretanto, faltou um requisito crucial: o fato de que o Clock configura um temporizador e atualiza a UI a cada segundo deveria ser um detalhe de implementação do Clock.

Idealmente, queremos escrever isso uma vez e fazer com que o Clock atualize a si mesmo:

```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Para implementar isto, precisamos adicionar "estado" ao componente Clock.

Estado é similar às props, mas é privado e totalmente controlado pelo componente.

Nós [mencionamos anteriormente]() que componentes definidos como classes possuem algumas características adicionais. Estado local é exatamente isso: uma característica disponível apenas para classes.

## Convertendo uma Função para uma Classe

Você pode converter um componente funcional como o Clock para uma classe em cinco passos:

1. Crie uma classe ES6, com o mesmo nome, que extenda de React.Component.
2. Adicione um único método vazio a ela, chamado render().
3. Mova o corpo da função para dentro do método render().
4. Substitua props por this.props no corpo de render().
5. Exclua a declaração remanescente da função.

```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
[Experimente no CodePen](http://codepen.io/gaearon/pen/zKRGpo?editors=0010).

Clock agora está definido como uma classe, ao invés de uma função.

Isso nos permite utilizar características adicionais, tais como estado local e ganchos de ciclo de vida.

## Adicionando Estado Local a uma Classe

Nós agora moveremos a date de props para o estado em três passos:

1. Substitua this.props.date por this.state.date no método render():

```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2. Adicione um [construtor da classe](#) que atribui o this.state inicial:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Perceba como agora passamos props para o construtor base:

```
constructor(props) {
  super(props);
  this.state = {date: new Date()};
}
```

Componentes em classes sempre devem invocar o construtor base com props.

3. Remova a prop date do elemento <Clock />:

```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Acrescentaremos posteriormente o código do temporizador de volta para o próprio componente.

O resultado parece-se com:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[Experimente no CodePen](http://codepen.io/gaearon/pen/KgQpJd?editors=0010).

A seguir, faremos com que Clock inicie seu próprio temporizador e atualize a si mesmo a cada segundo.

## Adicionando Métodos de Ciclo de Vida a uma Classe

Em aplicações com muitos componentes, é muito importante a liberação de recursos tomados pelos componentes quando estes são destruídos.

Nós deseamos [inicializar um temporizador](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) sempre que Clock for renderizado no DOM pela primeira vez. Isso é chamado de "montagem" no React.

Queremos, também, [limpar esse temporizador](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) sempre que o DOM produzido pelo Clock for removido. Isso é chamado de "desmontagem" no React.

Podemos declarar métodos especiais na classe do componente para executar alguns códigos quando um componente é montado e desmontado:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Esses métodos são chamados de "ganchos de ciclo de vida".

O gancho componentDidMount() é executado após a saída do componente ter sido renderizada no DOM. Este é um bom lugar para se inicializar um temporizador:

```
componentDidMount() {
  this.timerID = setInterval(
    () => this.tick(),
    1000
  );
}
```

Perceba como armazenamos o timer ID exatamente em this.

Enquanto this.props é inicializado pelo próprio React e this.state tem um significado especial, você é livre para adicionar campos extras à classe manualmente caso precise armazenar algo que não seja utilizado para a saída visual.

Se você não utilizar algo no render(), não deveria estar no estado.

Vamos destruir o temporizador no gancho de ciclo de vida componentWillUnmount():

```
componentWillUnmount() {
  clearInterval(this.timerID);
}
```

Finalmente, implementaremos um método chamado tick() que o componente Clock executará a cada segundo.

Este método utilizará this.setState() para programar atualizações do estado local do componente:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[Experimente no CodePen](http://codepen.io/gaearon/pen/amqdNA?editors=0010)

Agora o relógio "tica" a cada segundo.

Recapitulemos rapidamente o que está acontecendo e a ordem na qual os métodos são chamados:

1. Quando <Clock /> é passado para ReactDOM.render(), React invoca o construtor do componente Clock. Como Clock precisa mostrar a hora atual, ele inicializa this.state com um objeto incluindo a hora atual. Atualizaremos este estado posteriormente.

2. React então invoca o método render() do componente Clock. É assim que o React sabe o que deve ser mostrado na tela. React então atualiza o DOM para equivaler à saída do render do Clock.

3. Quando a saída de Clock é inserida no DOM, React invoca o gancho de ciclo de vida componentDidMount(). Dentro dele, o componente Clock pede ao navegador para inicializar um temporizador para chamar o método tick() do componente a cada segundo.

4. A cada segundo o navegador invoca o método tick(). Dentro dele, o componente Clock programa uma atualização da UI através da chamada de setState() com um objeto contendo a hora atual. Graças à chamada a setState(), React sabe que o estado sofreu alteração, e invoca o método render() novamente para saber o que deveria estar na tela. Desta vez, this.state.date no método render() será diferente, e assim a saída de render() incluirá a hora atualizada. React atualiza o DOM concordantemente.

5. Se o componente Clock for removido do DOM, React invoca o gancho de ciclo de vida componentWillUnmount() para que o temporizador seja interrompido.

## Utilizando Estado Corretamente

Existem três coisas que você deve saber sobre setState().

### Não Modifique o Estado Diretamente

Por exemplo, isso não irá re-renderizar um componente:

```
// Errado
this.state.comment = 'Hello';
```

Ao contrário, utilize setState():

```
// Correto
this.setState({comment: 'Hello'});
```

O único lugar onde você pode fazer atribuições a this.state é no construtor.

### Atualizações de Estado Podem ser Assíncronas

O React pode reunir múltiplas chamadas de setState() em uma única atualização por performance.

Devido ao fato de this.props e this.state poderem ser atualizados assincronamente, você não deve confiar em seus valores para calcular o próximo estado.

Por exemplo, este código pode falhar para atualizar o counter:

```
// Errado
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

Para consertá-lo, utilize uma segunda forma de setState() que aceita uma função ao invés de um objeto. Essa função receberá o estado anterior como primeiro argumento, e as props assim que a atualização for aplicada como segundo argumento:

```
// Correto
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

Utilizamos uma [função de seta](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) acima, mas funções regulares também funcionam:

```
// Correto
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### Atualizações de Estado são Fundidas

Quando você chama setState(), React funde o objeto que você prover dentro do estado atual.

Por exemplo, seu estado talvez contenha várias variáveis independentes:

```
constructor(props) {
  super(props);
  this.state = {
    posts: [],
    comments: []
  };
}

```

Então você pode atualizá-las independentemente com chamadas setState() separadas:

```
componentDidMount() {
  fetchPosts().then(response => {
    this.setState({
      posts: response.posts
    });
  });

  fetchComments().then(response => {
    this.setState({
      comments: response.comments
    });
  });
}
```

A fusão é superficial, então this.setState({comments}) deixa this.state.posts intacto, mas substitui this.state.comments completamente.

## Os Dados Fluem para Baixo

Nem componentes pais nem filhos podem saber se um certo componente possui estado ou não, e eles não devem se importar se este componente é definido como funcional ou por classe.

É por isso que o estado frequentemente é chamado local ou encapsulado. Ele não é acessível a nenhum componente que não seja aquele que o possui e o configura.

Um componente pode escolher passar seu estado como props para seus componentes filhos:

```
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

Isso também funciona para componentes definidos pelo usuário:

```
<FormattedDate date={this.state.date} />
```

O componente FormattedDate receberia date em suas props e não saberia se date veio do estado de Clock, das props de Clock, ou se foi digitado à mão:

```
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[Experimente no CodePen](http://codepen.io/gaearon/pen/zKRqNB?editors=0010).

Isso comumente é chamado de fluxo "cima-baixo" ou "unidirecional" de dados. Qualquer estado sempre é possuído por algum componente específico, e qualquer dado ou UI derivada desse estado pode afetar apenas componentes "abaixo" deles na árvore.

Se você imaginar uma árvore de componentes como uma cachoeira de props, cada estado de cada componente é como uma fonte adicional de água que se une às props em um ponto arbitrário, mas também flui para baixo.

Para mostrar que todos os componentes são realmente isolados, podemos criar um componente App que renderiza três <Clock>s:

```
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
[Experimente no CodePen](http://codepen.io/gaearon/pen/vXdGmd?editors=0010).

Cada Clock configura seu próprio temporizador e o atualiza independentemente.

Em aplicações React, se um componente possui estado ou não é considerado um detalhe de implementação que talvez mude com o tempo. Você pode utilizar componentes sem estado dentro de componentes com estado, e vice-versa.

#### **Capitulo anterior**:  [Componentes e Props](./componentes-props.md).

#### **Próximo capítulo**:  [Manipulando Eventos](./manipulando-eventos.md).

[versão original desta pagina](https://reactjs.org/docs/state-and-lifecycle.html)
