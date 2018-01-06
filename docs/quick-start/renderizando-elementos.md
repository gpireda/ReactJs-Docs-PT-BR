# **Renderizando Elementos**

Elementos são a menor parte de aplicações React.

Um elemento descreve o que você quer ver na tela:

```
const element = <h1>Hello, world</h1>;
```

Ao contrário de elementos DOM de navegadores, elementos React são objetos puros, e são econômicos de se criar. React DOM se preocupa em atualizar o DOM para corresponder aos elementos React.

**Nota:**
**Pode ser feita confusão entre elementos e um conceito mais amplamente conhecido de "componentes". Introduziremos componentes na [próxima seção](./componentes-props.md). Elementos são a base de construção de componentes, e nós o encorajamos a ler esta seção antes de prosseguir.**

## **Renderizando um Elemento no DOM**

Digamos que haja um `<div>` em algum lugar do seu arquivo HTML:

```
<div id="root"></div>
```

Chamamos este node do DOM de "root" (raiz) porque tudo dentro dele será gerenciado pelo React DOM.

Aplicações construídas apenas com React geralmente possuem um único node root. Se você estiver integrando React em uma aplicação existente, talvez possua tantos nodes root isolados quanto quiser.

Para renderizar um elemento React em um node DOM root, passe ambos para ReactDOM.render():

```
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root)
);
```

[Experimente no CodePen](https://codepen.io/gaearon/pen/rrpgNB?editors=1010)

O código acima mostra "Hello, world" na tela.

## Atualizando o Elemento Renderizado

Elementos React são [imutáveis](https://en.wikipedia.org/wiki/Immutable_object). Uma vez criado um elemento, você não pode modificar seus filhos ou atributos. Um elemento é como um único quadro em um filme: ele representa a UI em um certo ponto no tempo.

Com nosso conhecimento até agora, a única forma de se atualizar a UI é através da criação de um novo elemento, e sua passagem para ReactDOM.render().

Considere o exemplo do relógio tique-taque:

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

[Experimente no CodePen](https://codepen.io/gaearon/pen/gwoJZk?editors=0010)

Ele invoca ReactDOM.render() a cada segundo através de um setInterval() de callback.

**Nota:**
**Na prática, a maioria das aplicações React chamam ReactDOM.render() apenas uma vez. Nas próximas seções aprenderemos como tal código é encapsulado em [componentes com estado](./estado-ciclo-vida.md).**
**Recomendamos que você não pule tópicos, porque eles são construídos um por cima do outro.**

## **React Apenas Atualiza o que é Necessário**

O React DOM compara o elemento e seus filhos com o anterior, e somente aplica as atualizações necessárias no DOM para trazê-lo a estado desejado.

No exemplo anterior, apesar de termos criado um elemento descrevendo toda a árvore UI a cada tique do relógio, apenas os nodes de texto cujos conteúdos foram alterados são atualizados pelo React DOM.

Em nossa experiência, pensar em como a UI deve parecer em um dado momento, ao invés de como modificá-la no decorrer do tempo, elimina toda uma classe de bugs.

#### **Capitulo anterior**:  [Introdução ao JSX](./introducao-jsx.md).

#### **Próximo capítulo**:  [Componentes e Props](./componentes-props.md).

[versão original desta pagina](https://reactjs.org/docs/rendering-elements.html)
