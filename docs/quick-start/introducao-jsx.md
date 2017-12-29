# **Introdução ao JSX**

Considere essa declaração de variável:

``` 
const element = <h1>Hello, world!</h1>;
```

Esta divertida sintaxe não é nem uma string e nem um HTML.

Ela é chamada de JSX, é uma extensão sintática do JavaScript. Nós recomendamos utilizá-la com React para dizer como a UI deve ser. JSX pode te dar a impressão de uma linguagem de marcação, mas ela vem como todo o poder do JavaScript.

JSX produz "elementos" React. Exploraremos as suas renderizações no DOM na próxima seção. Abaixo, você pode encontrar os princípios básicos do JSX necessários para você começar.

## **Incorporando expressões no JSX**

Você pode incorporar qualquer [expressão javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) no JSX apenas envolvendo o código entre chaves.

por exemplo, **2+2**, **user.firstName** e **formatName(user)** são todas expressões válidas:

```
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
``` 

[Veja no Codepen](https://codepen.io/gaearon/pen/PGEjdG?editors=0010)

Nós dividimos o JSX em diversas linhas para uma melhor leitura.  Embora não seja necessário, ao fazê-lo, também recomendamos encaixá-lo entre parênteses para evitar as armadilhas da [inserção automática de ponto e vírgula](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi).

## **JSX também é uma expressão**

Depois da compilação, códigos JSX se tornam chamadas normais de funções JavaScript e são avaliados em objetos JavaScript.

Isso quer dizer que você pode usar JSX dentro de declarações **if** e **for** loops, atribuí-lo a variáveis, aceitá-lo como argumentos e retorná-lo de funções:

```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

## **Especificando atributos com JSX**

Você pode utilizar aspas para especificar strings literais como atributos:

```
const element = <div tabIndex="0"></div>;
```

Você também pode utilizar chaves para embutir uma expressão JavaScript em um atributo:

``` 
const element = <img src={user.avatarUrl}></img>;
```

Não coloque aspas em torno das chaves ao embutir uma expressão de JavaScript em um atributo. Você deve usar aspas (para valores de string) ou chaves (para expressões), mas não ambas no mesmo atributo.

**Atenção:**
**Como o JSX está mais próximo do JavaScript do que do HTML, o React DOM usa a convenção de nomenclatura de propriedades em camelCase no lugar de nomes de atributos HTML.**
**Por exemplo, a classe se torna className no JSX e tabindex torna-se tabIndex.**

## **Especificando Filhos com JSX**

Se uma tag estiver vazia, você pode fechá-la imediatamente com />, assim como no HTML:

```
const element = <img src={user.avatarUrl} />;
```

Tags JSX podem conter filhos:

```
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## **JSX previne Ataques de Injeção**

É seguro embutir input de usuário no JSX:

```
const title = response.potentiallyMaliciousInput;
// Isto é seguro
const element = <h1>{title}</h1>;
```

Por padrão, o React DOM [escapa](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) quaisquer valores embutidos no JSX antes de renderizá-los. Assim, ele garante que você nunca conseguirá injetar qualquer coisa que não esteja explicitamente escrita na sua aplicação. Tudo é convertido para uma string antes de ser renderizado. Isso ajuda a prevenir ataques [XSS(cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting).

## **JSX representa Objetos**

O Babel compila o JSX até chamadas React.createElement().

Esses dois exemplos são idênticos.

```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

React.createElement() executa algumas verificações que te ajudam a escrever código livre de bugs, mas essencialmente cria um objeto como este:

```
// Nota: esta estrutura está simplificada
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

Esses objetos são chamados "elementos React". Você pode pensar neles como descrições do que você quer ver na tela. O React lê esses objetos e os utiliza para construir o DOM e mantê-lo atualizado.

Exploraremos a renderização de elementos React na próxima seção.

**Dica:**
**Recomendamos a utilização da [definição de linguagem do "Babel"](http://babeljs.io/docs/editors) em seu editor de escolha para que ambos os códigos ES6 e JSX sejam devidamente realçados.**

#### **Capitulo anterior**:  [Olá Mundo](./ola-mundo.md).

#### **Próximo capítulo**:  [Renderizando Elementos](./renderizando-elementos.md).

[versão original desta pagina](https://reactjs.org/docs/introducing-jsx.html)
