# Componentes e Props

Componentes permitem que você divida a UI em pedaços independentes, reutilizáveis, e pense em cada pedaço isoladamente.

Conceitualmente, componentes são como funções JavaScript. Eles aceitam inputs arbitrários (chamados "props") e retornam elementos React descrevendo o que deve aparecer na tela.

## Componentes Funcionais e Componentes em Classes

A forma mais simples de se definir um componente é escrever uma função em JavaScript:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

Esta função é um componente React válido porque aceita um único objeto "props" (que significa propriedades) como argumento com dado e retorna um elemento React. Chamamos tais componentes de "funcionais" porque eles são literalmente funções JavaScript.

Você também pode usar uma [classe do ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) para definir um componente:

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Os dois componentes acima são equivalentes do ponto de vista do React.

Classes têm algumas características adicionais que discutiremos nas [próximas seções](./estado-ciclo-vida.md). Até lá, utilizaremos componentes funcionais por sua concisão.

## **Renderizando um Componente**

Anteriormente, encontramos apenas elementos React que representam tags do DOM:

```
const element = <div />;
```

No entanto, elementos também podem representar componentes definidos pelo usuário:

```
const element = <Welcome name="Sara" />;
```

Quando o React encontra um elemento que representa um componente definido pelo usuário, ele passa os atributos JSX para este componente como um único objeto. A este objeto chamamos "props".

Por exemplo, este código renderiza "Hello, Sara" na página:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[Experimente no CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/rendering-a-component).

Vamos revisar o que acontece neste exemplo:

1. Chamamos ReactDOM.render() com o elemento <Welcome name="Sara" />.
2. React invoca o componente Welcome com {name: 'Sara'} como props.
3. Nosso componente Welcome retorna um elemento `<h1>Hello, Sara</h1>` como resultado.
4. React DOM eficientemente atualiza o DOM para corresponder a `<h1>Hello, Sara</h1>`.

**Ressalva:**
**Sempre inicie nomes de componentes com letra maiúscula.**
**Por exemplo, `<div />` representa uma tag do DOM, mas `<Welcome />` representa um componente e exige que Welcome esteja no escopo.**

## **Compondo Componentes**

Componentes podem referenciar outros componentes na sua saída. Isso nos permite utilizar a mesma abstração de componentes para qualquer nível de detalhe. Um botão, um formulário, um diálogo, uma tela: em aplicações React, todos estes são comumente expressados como componentes.

Por exemplo, podemos criar um componente App que renderiza Welcome várias vezes:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[Experimente no CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/composing-components).

Tipicamente, novas aplicações React possuem um único componente App no seu topo. No entanto, se você integrar React a uma aplicação existente, você pode começar de baixo para cima com um pequeno componente como Button e grdualmente trabalhar até o topo da hierarquia de visualização.

## **Extraindo Componentes**

Não tenha medo de separar componentes em componentes menores.

Por exemplo, considere este componente Comment:

```
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Experimente no CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components).

Ele aceita author (um objeto), text (uma string) e date (uma data) como props, e descreve um comentário em um website de mídia social.

Este componente pode ser complicado de se alterar devido a todo o aninhamento, e também é difícil utilizar partes individuais do mesmo. Vamos extrair alguns componentes dele.

Primeiro, extrairemos Avatar:

```
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

O Avatar não precisa saber que está sendo renderizado dentro de um Comment. É por isso que nomeamos sua prop de forma mais genérica: user, ao invés de author.

Recomendamos a nomeação de props do ponto de vista do próprio componente, ao invés do contexto no qual ele está sendo utilizado.

Agora podemos simplicar Comment um pouco:

```
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text>
        {props.text}
      </div>
      <div className="Comment-date>
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Agora vamos extrair um componente UserInfo que renderiza um Avatar próximo ao nome do usuário:

```
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

Isso nos permite simplificar Comment ainda mais: 

```
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Experimente no CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components-continued).

Extrair componentes pode parecer um trabalho excessivo à primeira vista, mas possuir uma paleta de componentes reutilizáveis compensa em aplicações maiores. Uma boa regra é que se uma parte de sua UI é utilizada várias vezes (Button, Panel, Avatar), ou é complexa o suficiente por si só (App, FeedStory, Comment), então é uma boa candidata a ser um componente reutilizável.

## **Props são Somente Leitura**

Se você declarar um componente, seja como [uma função ou uma classe](#componentes-funcionais-e-componentes-em-classes), ele nunca deve modificar suas props. Considere esta função sum:

```
function sum(a, b) {
  return a + b;
}
```

Tais funções são chamadas ["puras"](https://en.wikipedia.org/wiki/Pure_function) porque não tentam modificar seus inputs, e sempre retornam o mesmo resultado para os mesmos inputs.

Em contraste, esta função é impura porque ela modifica seus próprios inputs:

```
function withdraw(account, amount) {
  account.total -= amount;
}
```

React é bastante flexível, mas tem uma única regra rigorosa:

**Todos os componentes React devem agir como funções puras com respeito às suas props.**

É claro, aplicações UIs são dinâmicas e mudam no decorrer do tempo. Na [próxima seção](./estado-ciclo-vida.md) introduziremos um novo conceito de "estado". O estado permite que componentes React modifiquem seus outpus no decorrer do tempo em resposta a ações de usuários, respostas de rede, e tudo o mais, sem violar esta regra.

#### **Capitulo anterior**:  [Renderizando Elementos](./renderizando-elementos.md).

#### **Próximo capítulo**:  [Estado e Ciclo de Vida](./estado-ciclo-vida.md).

[versão original desta pagina](https://reactjs.org/docs/components-and-props.html)
