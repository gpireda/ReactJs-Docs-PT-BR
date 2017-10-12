
# **instalação**

React é flexível e pode ser usado em diversos projetos. Você pode criar novos aplicativos com ele, mas você também pode introduzi-lo gradualmente em uma base de código existente sem fazer uma reescrita.

Aqui estão algumas maneiras de começar:

#### **Testando o React**  
  
Se você está apenas interessado em testar e brincar com React, você pode usar o Codepen. Tente começar deste código de exemplo de [hello world](https://codepen.io/gaearon/pen/rrpgNB?editors=0010). Você não precisa instalar nada, você pode apenas modificar o código e ver se funcionou.

Se você preferir usar seu próprio editor de código, você pode fazer o download deste html, editá-lo, e abri-lo a partir do sistema de arquivos local em seu navegador. Essa maneira tem uma performance mais lenta, entao nao use em produção.

Se você quiser usar a aplicação completa, existem duas maneiras populares para iniciar com React: usando Create React App, ou adicionando-o a uma aplicação existente.

#### **Criando uma nova aplicação**  
Create react app a melhor maneira para iniciar a construção de uma nova single page application. Ele configura automaticamente para você o ambiente de desenvolvimento para que possa utilizar as últimas features do javascript, proporcionando uma boa experiência de para desenvolver e otimizar seu app para produção. Você irá precisar ter o node >= 6 na sua máquina.

```
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

Create React App nao manipula lógica backend ou de banco de dados, ele apenas cria um modelo front end, logo você pode usar isso com qualquer backend que desejar. 
Ele utiliza ferramentas de compilação como Babel e Webpack por baixo dos panos, mas funciona sem que seja necessária fazer nenhuma configuração.

quando estiver pronto para realizar o deploy, executar npm run build criara uma compilação otimizada do seu aplicativo na pasta build. Você pode aprender mais sobre o create react app a partir do [README](https://github.com/facebookincubator/create-react-app#create-react-app-) e do [Guia do usuário](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#table-of-contents)
