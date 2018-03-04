# Listas e Chaves

Primeiro, revisemos como você transforma listas em JavaScript.

Dado o código abaixo, utilizamos a função [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) para tomar um array de números e dobrar seus valores. Atribuímos o novo array retornado por `map()` à variável dobrada e logamos:

```
  const numbers = [1, 2, 3, 4, 5];
  const doubled = numbers.map((number) => number * 2);
  console.log(doubled);
```

Este código loga `[2, 4, 6, 8, 10]` no console.

Em React, transformar arrays em listas de elementos é praticamente idêntico.

## Renderizando Múltiplos Componentes

Você pode construir coleções de elementos e [incluí-los em JSX](./introducao-jsx.md#incorporando-expressões-no-jsx) utilizando chaves `{}`.

Abaixo, realizamos um loop através do array `numbers` utilizando a função `map()`. Retornamos um elemento `<li>` para cada item. Finalmente, atribuímos o array resultante de elemento para `listItems`:

```
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
```

Incluímos todo o array `listItems` dentro de um elemento `<ul>`, e [o renderizamos no DOM](./renderizando-elementos.md#renderizando-um-elemento-no-dom):

```
  ReactDOM.render(
    <ul>{listItems}</ul>,
    document.getElementById('root)
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/GjPyQr?editors=0011)

Este código mostra uma lista, com marcadores, de números entre 1 e 5.

### Componente Básico de Lista

Geralmente você renderizaria listas dentro de um [componente](./componentes-props).

Podemos refatorar o exemplo anterior para um componente que aceita um array de `números` e retorna uma lista não ordenada de elementos.

```
  function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) =>
      <li>{number}</li>
    );
    return (
      <ul>{listItems}</ul>
    );
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
  );
```

Quando este código for executado, você receberá um aviso que uma `key` deveria ser fornecida para os ítens da lista. Uma "chave" é um atributo string especial que você precisa incluir quando cria listas de elementos. Discutiremos porque este atributo é importante na próxima seção.

Vamos atribuir uma `key` para nossa lista dentro de `numbers.map()` e resolver o problema.

```
  function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) =>
      <li key={number.toString()}>
        {number}
      </li>
    );
    return (
      <ul>{listItems}</ul>
    );
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)

## Chaves

Chaves auxiliam o React a identificar quais ítens mudaram, são adicionados ou são removidos. Chaves devem ser dadas aos elementos dentro do array para dar aos elementos uma identidade estável:

```
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
```

A melhor forma de escolher uma chave é utilizar uma string que identifique unicamente um item da lista dentre seus irmãos. Na maioria das vezes você utilizaria IDs de seus dados como chaves:

```
  const todoItems = todos.map((todo) =>
    <li key={todo.id}>
      {todo.text}
    </li>
  );
```

Quando você não tem IDs estáveis para ítens renderizados, você pode utilizar o índice do ítem como um último recurso:

```
  const todoItems = todos.map((todo, index) =>
    // Faça isso apenas se os ítens não têm IDs estáveis
    <li key={index}>
      {todo.text}
    </li>
  );
```

Não recomendamos utilizar índices para chaves se a ordem da lista pode sofrer alteração. Isto pode impactar negativamente a performance e pode causar problemas com o estado do componente. Confira o artigo de Robin Pokorny para uma [profunda explicação dos impactos negativos de se utilizar um índice como uma chave](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318). Se você escolher não atribuir uma chave explícita para ítens de listas, o React utilizará seus índices por padrão.

Aqui há uma [explicação profunda sobre porque chaves são necessárias](#) se você estiver interessado em aprender mais.

### Extraindo Componentes com Chaves

Chaves somente fazem sentido no contexto do array.

Por exemplo, se você [extrai](./componentes-props.md#extraindo-componentes) um componente `ListItem`, você deveria manter a chave nos elementos `<ListItem />` no array ao invés dos elementos `<li>` no próprio `<ListItem>`.

#### Exemplo: Utilização Incorreta de Chaves
```
  function ListItem(props) {
    const value = props.value;
    return (
      // Errado! Não há necessidade de especificar a chave aqui:
      <li key={value.toString()}>
        {value}
      </li>
    );
  }

  function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) =>
      // Errado! A chave deveria ter sido especificada aqui:
      <ListItem value={number} />
    );
    return (
      <ul>
        {listItems}
      </ul>
    );
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
  );
```

#### Exemplo: Utilização Correta de Chaves
```
  function ListItem(props) {
    // Correto! Não há necessidade de especificar a chave aqui:
    return <li>{props.value}</li>;
  }

  function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) =>
      // Correto! A chave deveria ser especificada dentro do array.
      <ListItem key={number.toString()}
                value={number} />

    );
    return (
      <ul>
        {listItems}
      </ul>
    );
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
  );

```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/ZXeOGM?editors=0010)

Uma boa regra geral é que elementos dentro da chamada a `map()` precisam de chaves.

### Chaves Devem ser Únicas Apenas Entre Irmãos

Chaves utilizadas dentro de arrays deveriam ser únicas dentre seus irmãos. Entretanto, não precisam ser únicas globalmente. Podemos utilizar as mesmas chaves quando produzimos dois arrays distintos:

```
  function Blog(props) {
    const sidebar = (
      <ul>
        {props.posts.map((post) =>
          <li key={post.id}>
            {post.title}
          </li>
        )}
      </ul>
    );
    const content = props.posts.map((post) =>
      <div key={post.id}>
        <h3>{post.title}</h3>
        <p>{post.content}</p>
      </div>
    );
    return (
      <div>
        {sidebar}
        <hr />
        {content}
      </div>
    );
  }

  const posts = [
    {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
    {id: 2, title: 'Installation', content: 'You can install React from npm.'}
  ];
  ReactDOM.render(
    <Blog posts={posts} />,
    document.getElementById('root')
  );
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)

Chaves servem como dicas para o React, mas não são passadas para seus componentes. Se você precisar do mesmo valor em seu componente, passe-o explicitamente como uma prop com um nome diferente:

```
  const content = posts.map((post) =>
    <Post
      key={post.id}
      id={post.id}
      title={post.title} />
  );
```

### Incorporando map() em JSX

Nos exemplos acima declaramos uma variável separada `listItems` e a incluímos no JSX:

```
  function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) =>
      <ListItem key={number.toString()}
                value={number} />

    );
    return (
      <ul>
        {listItems}
      </ul>
    );
  }
```

JSX nos permite [incorporar quaisquer expressões](./introducao-jsx.md#incorporando-expressões-no-jsx) em chaves. Assim podemos utilizar o resultado de `map()` diretamente:

```
  function NumberList(props) {
    const numbers = props.numbers;
    return (
      <ul>
        {numbers.map((number) =>
          <ListItem key={number.toString()}
                    value={number} />

        )}
      </ul>
    );
  }
```

[Experimente no CodePen.](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)

Às vezes isso resulta em código mais limpo, mas este estilo também pode ser abusado. Assim como em JavaScript, cabe a você decidir se vale a pena extrair uma variável por legibilidade. Tenha em mente que se o corpo de `map()` é muito aninhado, pode ser uma boa hora para [extrair um componente](./componentes-props.md#extraindo-componentes).

#### **Capitulo anterior**:  [Renderização Condicional](./renderizacao-condicional.md).

#### **Próximo capítulo**:  [Formulários](./formularios.md).

[versão original desta pagina](https://reactjs.org/docs/lists-and-keys.html)
