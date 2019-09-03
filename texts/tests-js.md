Diferentemente do que muitos pensam, o desenvolvimento de uma aplicação Web ou Mobile necessita de testes, seja para assegurar a qualidade do produto, o funcionamento, e até mesmo a aparência, durante a evolução do código.
Quando nosso software está bem consolidado em termos de testes, podemos estabelecer estratégias de integração e deploy contínuos (CI/CD). Esses métodos atuam para garantir que nossa aplicação não tenha sofrido efeitos colaterais com as adições, modificações e correções que estarão sendo enviadas à branch principal para deploy. Nesse post, serão introduzidos os conceitos de **Spies** e **Stubs**, e como eles são úteis durante o desenvolvimento de um conjunto de testes de unidade.

# Teste de Unidade

Vamos supor o seguinte cenário: temos uma aplicação que requer o cadastro dos seus usuários com um *username*, que deve ter tamanho de pelo menos 3 caracteres. Para tal, podemos adicionar no código de cadastro uma verificação para o tamanho do username:

```javascript
function cadastrar(username, senha) {
  if (username.length < 3) {
    throw new Error('O username necessita de pelo menos 3 caracteres');
  }
  // Continua o cadastro
};
```

Quando escrevemos testes para a função de cadastro, nossa intenção seria testar diferentes casos, escolhendo **valores limite**, para podermos testar a qualidade da nossa verificação e se estamos deixando passar algum cenário indesejado. Por enquanto, não vamos nos importar tanto com a sintaxe, mas na semântica:

```javascript
describe('testes da função de cadastro', () => {
  it('testa um username válido', () => {
    expect(cadastrar('teste', 'teste')).to.not.throw(); // Nesse caso, espera-se que não seja lançado um erro, visto que o username tem três ou mais caracteres
  });
  it('testa um username invalido', () => {
    expect(cadastrar('te', 'teste')).to.throw('O username necessita de pelo menos 3 caracteres'); // Nesse outro caso, como o username tem menos de 3 caracteres, espera-se que seja lançado um erro com a mensagem descrita
  });
  // testes de senha, e outros fluxos do cadastro
});
```

Nesse caso, estamos testando apenas a função de cadastro, ou seja, um teste unitário que testa apenas uma "unidade básica" do sistema (entenda unidade básica como aquela unidade que não chama outras funções internamente). De agora em diante, a ideia é termos funções mais complicadas que isso, ou seja, funções que precisam chamar outras funções na sua execução, por envolverem lógicas mais complexas.

# Spies

![Spies](https://pbs.twimg.com/profile_images/951185073402994688/pKyQmqYh.jpg)

Imagine agora que, uma vez cadastrado, também seja possível alterar esse *username*. Temos, então, duas situações possíveis em que desejamos verificar se o que o usuário inseriu é válido. Para isso, podemos refatorar nosso código atual de maneira a reutilizar as linhas que verificam se o *username* está no padrão correto:

```javascript
function verificaUsername(username) {
  if (username.length < 3) {
    throw new Error('O username necessita de pelo menos 3 caracteres');
  }
};

function cadastrar(username, senha) {
  verificaUsername(username);
  // Continua o cadastro
};
```

Com o código refatorado, é também preciso refatorar os testes, para que se adequem ao contexto real do código:

```javascript
describe('testes da função de cadastro', () => {
  it('testa um username válido', () => {
    const spy = sinon.spy(verificaUsername);
    expect(cadastrar('teste', 'teste')).to.not.throw();
    expect(spy).to.have.been.called;
  });
  it('testa um username invalido', () => {
    const spy = sinon.spy(verificaUsername);
    expect(cadastrar('te', 'teste')).to.throw('O username necessita de pelo menos 3 caracteres');
    expect(spy).to.have.been.called;
  });
  // testes de senha, e outros fluxos do cadastro
});
```

Agora que já vimos como os spies são declarados e verificados, é mais fácil entender seu significado: um spy serve para verificar se uma função foi chamada ou não durante a execução de outra função. No nosso exemplo, pedimos para que o jest "espie" o método `verificaUsername` e, após a chamada para a execução de `cadastrar`, verificamos se `verificaUsername` foi chamada. 

Entretanto, existe uma particularidade importante a se notar no nosso código: quando testamos um username inválido, a exceção ainda é lançada. Isso nos faz notar que nosso spy não modifica nada no código em execução, apenas verifica se as chamadas internas a uma função são realmente chamadas.

# Stubs

Mudando um pouco a perspectiva dentro do sistema que estamos construindo, podemos pensar num sistema mais complexo e que funciona numa certa sequência de operações e, para executar a operação seguinte, a anterior precisa ter sido executada corretamente. Por exemplo:

```javascript
function operacaoComplexa() {
  return operacaoMenor().then((resposta) => {
    if (resposta.param) {
      // ...
    } else {
      // ...
    }
    return x;
  }).catch((erro) => {
    throw new Error(erro);
  });
}
```

A função acima não parece ter uma lógica nem um motivo bem definidos, como é o caso da função de cadastro. Entretanto, não é esse o ponto em que precisamos focar: podemos ver que o retorno da `operacaoMenor` é importante para entendermos o que será retornado nessa função, seja em caso de sucesso ou em caso de erro. Consideremos então que, por exemplo, essa função menor faz uma requisição a um serviço externo, uma API por exemplo.

Na execução do nosso código, o código dessa função executará normalmente, fazendo a requisição necessária. Durante os testes, entretanto, não se deve fazer uma chamada à API, já que a API pode alterar dados reais da aplicação, deixar o banco de dados inconsistente e causar muitos outros problemas. Precisamos então de uma forma para testar a operação complexa sem realmente executar o código de `operacaoMenor`, e para isso servem os **stubs**.

![Stubs](https://i0.wp.com/yukaichou.com/wp-content/uploads/2014/10/Gamification-vs-Manipulation-image.jpg?resize=600%2C375&ssl=1)

Então, o que exatamente um Stub faz? Durante a execução dos nossos testes, um stub substitui uma função existente no código por uma função representativa, que nos permite controlar o retorno, para que o restante do código possa executar normalmente e possamos percorrer todos os cenários possíveis da execução do programa durante os testes. Vejamos como seria a aplicação de um stub no código dessa função:

```javascript
describe('testa operacaoComplexa', () => {
  it('testa cenario 1 do then', async () => {
    const stub = sinon.stub(operacaoMenor)
      .resolves({ param: true });
    const retornoComplexo = await operacaoComplexa();
    expect(retornoComplexo).to.eql(/* retorno no caso 1 */);
    expect(stub).to.have.been.called;
  });
  it('testa cenario 2 do then', async () => {
    const stub = sinon.stub(operacaoMenor)
      .resolves({ param: false });
    const retornoComplexo = await operacaoComplexa();
    expect(retornoComplexo).to.eql(/* retorno no caso 2 */);
    expect(stub).to.have.been.called;
  });
  it('testa cenario catch', () => {
    const stub = sinon.stub(operacaoMenor)
      .rejects('mensagem de erro');
    operacaoComplexa()
      .then(() => {
        throw new Error('Operação não deveria ter dado certo');
      }).catch((erro) => {
        expect(erro).to.eql('mensagem de erro');
      });
    expect(stub).to.have.been.called;
  });
});
```

O teste acima verifica os três cenários que colocamos no código da nossa função. O teste parece ser grande, mas cobre apenas os três fluxos básicos na execução da `operacaoComplexa`. Explicando em alto nível a sintaxe:
* no caso 1, estamos dizendo que a `operacaoMenor` deve ser um stub que resolve, no retorno da Promise, um objeto `{ param: true }`;
* no caso 2, estamos dizendo que a `operacaoMenor` deve ser um stub que resolve, no retorno da Promise, um objeto `{ param: false }`;
* no caso 3, de erro, estamos dizendo que a `operacaoMenor` deve ser um stub que rejeita, no retorno da Promise, sendo `'mensagem de erro'` a string retornada no erro.

Nesse caso específico, nossa função complexa tinha uma chamada assíncrona (uma Promise) e, por isso, utilizamos `resolves` e `rejects` no nosso stub; caso fosse uma função síncrona, poderíamos ter utilizado `returns` normalmente.

## Plus!

Existem diversas bibliotecas que podem ser utilizadas para testes em JavaScript. Algumas das mais famosas são [Mocha](https://mochajs.org/#getting-started), [Sinon](https://sinonjs.org) e [Chai](https://chaijs.com), que geralmente são utilizados em conjunto. Atualmente, uma das bibliotecas que está sendo bastante visada é o [Jest](https://jestjs.io). Se você está pensando em como começar a aplicar o que aprendeu aqui, te sugiro fazer alguns testes simples num dos sites que você hospeda no GitHub Pages — um portfolio, um pequeno projeto de disciplina, quem sabe? Qualquer um desses vai te dar um bom contato inicial :).

## Chegamos ao fim desse post... :(

Mas não se preocupe, há muito mais conteúdo do OpenDevUFCG para ler aqui no dev.to, e em breve ainda mais posts saindo do forno.

Agradeço bastante pela leitura, e se quiser entrar em contato comigo, só mandar um [Tweet](https://twitter.com/juliobguedes)! Se quiser ler mais textos meus, confere meu [Medium](https://medium.com/@Juliobguedes/) que em breve mais posts vão sair.
