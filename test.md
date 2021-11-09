## Automação de Testes em Angular

O foco desta documentação é passar rapidamente por alguns conceitos de testes automatizados focados em front-end Angular, bem como mostrar alguns conceitos específicos do ambiente de testes que o Angular provê.

Antes de tudo, vamos focar na modalidade de testes automatizados que é responsabilidade do desenvolvedor,  os testes unitários.
![Pirâmide de Testes](https://miro.medium.com/max/1000/0*UMzL89XZJ63vRCcc.png)

## 🧑‍🔬 Testes Unitários

O que são testes unitários?

	Teste Unitário é um nível de teste de software em que unidades individuais de um sistema são testados. O principal objetivo é validar que cada unidade do software funcione conforme o projetado.

O Angular por padrão nos provê um ambiente de testes já altamente configurado mas passível de ser modificado. Para testes unitários duas ferramentas que farão parte desta tarefa são o **Jasmine** e o **Karma**.

## 🌸 **Jasmine**

Framework para testar códigos JS (*Assertion Library*). Como segue o BDD (*Behavior-Driven Development*), permite escrever testes de forma legível a humanos. Mesmo não técnicos do time conseguem entender qual comportamento está sendo testado;

Vamos ver um exemplo básico de uma função e seu respectivo "Suíte de Teste"

```js
function sayHello(name) {
  return `Hi, ${name}`
}

// Spec
describe("sayHello Suite", function() {
  let name;

  it("Should say Hello Person", function() {
    name = 'Peter Parker';
    
    expect(digaOi(name)).toBeEqual('Hi, Peter Parker');
  });
});
```

Baseado no código acima temos:
 - **describe(sgring, fn)** Cria uma suíte de testes. Pode ser aninhado para compor diversos contextos. Seu primeiro parâmetro é o nome da suite e o segundo é a função;
 - **it(string, fn)**  — Define um único teste. Um teste deve conter uma ou mais expectativas que testem o estado do código;
- **expect(any)**  — Cria uma expectativa. No exemplo, a expectativa é que o retorno da função seja igual a  **‘Hi, Peter Parker’**;
- Para mais, leia esta documentação do Jasmine [aqui](https://jasmine.github.io/tutorials/your_first_suite);

Ao criar um arquivo (componente/serviço/pipe/diretiva) pelo Angular CLI, automaticamente um arquivo *.spec* é criado contendo já a maioria do *boilerplate* necessário para iniciar os testes.

## 🏇 Karma

Karma é o *test runner* fornecido pelo Angular CLI. É o responsável por rodar os testes em um navegador (default Chrome) e verificar as alterações nos arquivos. O Angular CLI também o traz já bem configurador.

## 🛏️ Angular TestBed 

É como se fosse um módulo Angular de testes. É o responsável por ajudar na preparação dos testes que dependem do Angular bem como manipulação do DOM, detectar mudanças e injetar dependência. Confira o exemplo:
```js
// Hook Que será executado antes de cada bloco (it) de testes
beforeEach(async () => {
  TestBed.configureTestingModule({
    // Importa as dependências daquele componente
    imports: [
      HttpClientTestingModule,
      RouterTestingModule.withRoutes([]);
    ],
    // Declaração do Componente
    declarations: [MyComponent],
    // Injeção dos serviços com um mock
    providers: [
      { provide: MyService, useValue: mockService as MyService},
    ]
  })
  await TestBed.compileComponents();
  fixture = TestBed.createComponent(MyComponent); // Retorna um objeto ComponentFixture
});
```
**ComponentFixture?**
	
	The purpose of a test fixture is to ensure that there is a well known and fixed environment in which tests are run so that results are repeatable. Some people call this the test context.
[Stackoverflow discussion link](https://stackoverflow.com/questions/12071344/what-are-fixtures-in-programming?answertab=active#tab-top)

Através do *ComponentFixture* podemos ter acesso a diversos métodos que nos ajudaram a testar um componente. Algumas propriedades comuns de se usar através do *ComponentFixture*:
- **fixture.componentInstance**: Retorna uma instância do componente criado;
 - **fixture.debugElement**:  O Angular depende de uma abstração de DebugElement para funcionar corretamente em browser distintos. Ao invés de criar uma árvore de elementos HTML, o angular criará uma árvore de componentes DebugElement, assim, essa abstração nos fornece diversas propriedades que são úteis na realização dos testes.
 
## 🚀 Executando os Testes

Para executar, basta executar o comando:
```
$ ng test
```
Uma interface será aberta no navegador mostrando todos os testes (se passaram ou não). É ali também que você se norteará para corrigir os testes.

## 📃 Code Coverage

Para mensurar a cobertura de testes, o Angular CLI utiliza por padrão o [instanbul](https://istanbul.js.org/). O **instanbul** verifica se um comando, branch, função ou linha foi chamada e produz um relatório de teste abrangente. Para apurar o *coverage* dos testes, execute o seguinte comando:
```
$ ng test --code-coverage
```
Assim que ele terminar de executar os testes, um diretório nominado como **coverage** irá aparecer na raíz do diretório do projeto. O relatório é um grupo de arquivos HTML que você pode abrir no browser. Inicie abrindo o arquivo *coverage/index.html*. Uma tela semelhante a esta deverá ser aberta:

![](https://testing-angular.com/assets/img/code-coverage-flickr-search.png)

Partindo desta tela você pode navegar para arquivos específicos e ter um detalhamento de quais partes do código estão sendo testadas e quantas vezes foram chamadas. Confira a imagem exemplo abaixo: 

![](https://testing-angular.com/assets/img/code-coverage-photo-item.png)

Vale lembrar que este tipo de relatório é apenas uma métrica quantitativa e não qualitativa dos testes. Também é válido ressaltar que cobrir 100% da *code base* com testes beira o impossível na prática.

## Conclusões
A ideia é que esta documentação seja só um pontapé inicial para entender algumas palavras chave no que diz respeito a testes automatizados no Angular, sinta-se livre para contribuir com a evolução deste documento.