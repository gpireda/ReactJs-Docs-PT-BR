# **Introdução ao JSX**

Considera essa declaração de variável:

``` 
const element = <h1>Hello, world!</h1>;
```

Esta divertida sintaxe não é nem uma string e nem um HTML.

Ela é chamada de JSX, é uma sintaxe específica do Javascript. Nós recomendamos utilizá-la com React para dizer como a UI deve ser. JSX pode te dar a impressão de uma linguagem de marcação, mas ela vem como todo o poder do javascript.

JSX produz "elementos" React. Exploraremos a sua renderização no DOM na próxima seção. Abaixo, você pode encontrar os princípios básicos do JSX necessários para você começar.

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

Nós dividimos o JSX em diversas linhas para uma melhor leitura.  Embora não seja necessário, ao fazê-lo, também recomendamos encaixá-lo entre parênteses para evitar as armadilhas da inserção [automática de ponto e vírgula](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi).