# Guia de Avaliação Técnica de Projeto Fullstack

Este documento reúne perguntas e critérios para conduzir a apresentação técnica de um projeto fullstack desenvolvido como task prática.

O objetivo não é apenas verificar se a aplicação funciona, mas também avaliar:
- **Capacidade de explicar o problema e a solução**
- **Entendimento da arquitetura**
- **Domínio das tecnologias utilizadas**
- **Clareza sobre decisões técnicas**
- **Maturidade para reconhecer limitações**
- **Raciocínio sobre melhorias e produção**

---

## Objetivo da Avaliação

A apresentação deve permitir identificar se a pessoa **apenas fez o projeto funcionar** ou se **realmente entendeu o que construiu**.

Uma boa apresentação técnica deve demonstrar:
- Visão geral do produto
- Clareza arquitetural
- Domínio básico do código
- Entendimento dos problemas enfrentados
- Capacidade de justificar decisões
- Consciência sobre limitações e próximos passos

---

## Estrutura Sugerida para a Apresentação

Antes das perguntas, espera-se uma explicação inicial cobrindo:
- Qual problema o projeto resolve?
- Qual stack foi utilizada?
- Como a aplicação está dividida?
- Como funciona o fluxo principal?
- Quais foram os principais desafios?
- O que foi entregue?
- O que ainda pode melhorar?

---

## Perguntas Gerais sobre o Projeto

### Visão Geral
- O que a aplicação faz?
- Qual era o objetivo principal do projeto?
- Quem seria o usuário da aplicação?
- Qual problema real estava sendo resolvido?
- O que foi entregue com sucesso?
- O que ficou incompleto, limitado ou pendente?

### Arquitetura
- Como o projeto está dividido entre frontend, backend e banco de dados?
- Quais tecnologias foram utilizadas? Por que essa stack foi escolhida?
- Com quem o frontend se comunica? De onde o backend obtém os dados?
- Onde os dados ficam armazenados?
- Como funciona o fluxo desde a ação do usuário até a resposta na interface?

### Frontend
- Como as páginas e componentes foram organizados?
- Como funciona a pesquisa no frontend?
- Como são tratados os estados de loading, erro e sucesso?
- Como o dashboard foi estruturado?
- Foi utilizado algum gerenciamento de estado?
- Como funciona a navegação entre páginas?
- Como páginas protegidas são tratadas, quando há autenticação?

### Backend / API
- Por que houve necessidade de criar uma API própria? Que problema ela resolveu?
- Quais endpoints existem atualmente? O que cada um retorna?
- A API apenas repassa dados ou também transforma informações?
- Como erros vindos de fontes externas são tratados?
- Como o sistema evita que o frontend fique fortemente acoplado à estrutura externa?

### Banco de Dados
- Quais tabelas foram criadas? Por que elas existem?
- Qual a relação entre elas?
- Como os campos foram definidos?
- O que é persistido no banco e o que é calculado em tempo de execução?
- Houve algum problema de modelagem? Qual?

### Dificuldades
- Qual foi o problema mais difícil enfrentado?
- Quais alternativas foram testadas antes da solução final?
- O que não funcionou na abordagem inicial?
- Como foram identificados conflitos de stack ou arquitetura? Como foram resolvidos?

### Entrega e Deploy
- Onde a aplicação está publicada?
- Como foi feito o processo de deploy?
- Quais diferenças surgiram entre ambiente local e produção?
- Como qualquer pessoa pode rodar o projeto localmente?

---

## Perguntas Técnicas para Validar Entendimento

### Fundamentos de JavaScript / TypeScript
- O que é `const`? Qual a diferença entre `const`, `let` e `var`?
- O que é uma função assíncrona? O que `await` faz?
- Qual a diferença entre uma Promise **pendente**, **resolvida** e **rejeitada**?
- O que acontece se uma operação assíncrona falhar sem tratamento de erro?
- O que é JSON?
- Qual a diferença entre `undefined` e `null`?
- O que é *destructuring*?
- Como percorrer um array de objetos?

### React
- O que é um componente React?
- O que são `props` e o que é `state`? Quando usar cada um?
- O que acontece quando o `state` muda?
- O que é `useEffect`?
- O que é renderização no React?
- Como evitar chamadas de API desnecessárias?
- Que problema pode surgir ao disparar pesquisa a cada tecla digitada?
- O que é *debounce* e em que cenário ele pode ser útil?

### HTTP e APIs
- O que é uma requisição HTTP?
- Qual a diferença entre `GET` e `POST`? Quando usar `PUT` ou `PATCH`?
- O que significam os status codes `200`, `400`, `404` e `500`?
- O que é um *payload*?
- O que é um endpoint REST? O que significa serializar uma resposta?
- Por que nem sempre é bom o frontend acessar diretamente uma fonte externa?
- Qual a vantagem de criar uma API intermediária? O que acontece se a estrutura da fonte externa mudar?

### Axios
- O que é Axios? Por que usar Axios em vez de `fetch`?
- Em que ponto do projeto o Axios entra?
- Qual a diferença entre usar Axios no frontend e no backend?
- O que Axios resolve e o que ele não resolve?

### Prisma e Banco de Dados
- O que é Prisma? Ele é um banco de dados?
- O Prisma substitui PostgreSQL ou trabalha sobre ele?
- O que é um ORM? Quais vantagens ele oferece?
- O que é uma *migration*?
- O que é um *model* no Prisma?
- Como relacionamentos entre tabelas são representados?
- O que é chave primária? O que é chave estrangeira?
- Por que IDs diferentes para a mesma entidade podem gerar problema? Como resolver?
- É melhor usar ID interno ou ID da fonte externa como referência principal? Por quê?

### Modelagem e Consistência
- Que risco existe quando a mesma entidade aparece em mais de uma tabela com identificadores diferentes?
- Como identificar corretamente que dois registros representam a mesma entidade?
- Nome pode ser uma chave única confiável? O que fazer se o nome puder mudar?
- Que tipo de normalização faria sentido?
- Quando vale a pena persistir dados já tratados?

### Integração Externa / Scraping
- Por que consumir diretamente um site pode ser frágil?
- O que é *rate limit* e bloqueio por IP?
- Quais são os riscos de scraping em produção?
- Como lidar com lentidão da fonte externa? Como reduzir a quantidade de chamadas?
- Onde cache poderia ser aplicado?
- Como diferenciar falha interna de falha da fonte externa?

### Arquitetura e Decisões Técnicas
- Por que misturar Next.js e Vite pode gerar conflito? Qual a diferença de proposta entre eles?
- Em que cenário Next.js faria mais sentido? Em que cenário Vite puro faria mais sentido?
- A arquitetura final ficou simples, acoplada ou escalável?
- O que deveria ser refatorado primeiro? O que funciona, mas ainda não está ideal?

---

## Perguntas de Raciocínio e Maturidade
- Se a fonte externa ficar indisponível por alguns minutos, como a aplicação deveria se comportar?
- Se muitos usuários fizerem pesquisas ao mesmo tempo, onde o sistema provavelmente sofreria primeiro?
- Como explicar a arquitetura do projeto em dois minutos para alguém que nunca viu o código?
- Qual foi a decisão técnica mais importante do projeto?
- Qual decisão foi tomada e depois seria refeita de outra maneira?
- O que foi aprendido de fato além da implementação?
- O que foi gambiarra consciente e o que foi solução sólida?
- Como diferenciar um MVP de uma aplicação pronta para produção?
- O que mais impede esse sistema de ir para produção hoje?
- Se outro desenvolvedor assumisse o projeto amanhã, o que não poderia faltar de documentação?

---

## Perguntas de Recrutador / Tech Lead
- O que foi priorizado: velocidade de entrega, robustez ou escalabilidade?
- Onde houve atalho técnico?
- Como foi validado que a solução estava correta?
- Quais sinais indicaram que a primeira abordagem não era boa o suficiente?
- A necessidade de uma API própria já existia desde o início ou surgiu durante o processo?
- O que seria melhorado com mais uma semana de trabalho?
- Quais partes do projeto estão maduras para explicação técnica e quais ainda exigem estudo?
- Como diferenciar “aprendi a usar” de “realmente entendi”?
- Qual foi o maior erro técnico do projeto?
- O que esse projeto revela sobre o perfil do desenvolvedor?

---

## Perguntas Práticas sobre o Código
- Onde começa a execução da aplicação frontend?
- Qual componente é responsável pela pesquisa?
- Onde acontece a chamada para a API? Onde os erros são tratados?
- Como é a estrutura da resposta da API? Onde os dados recebidos são transformados?
- Onde está definido o schema do Prisma?
- Como o banco é executado localmente?
- Onde ficam as variáveis de ambiente? Como segredos são protegidos?
- Onde ficam migrations ou definições estruturais do banco?
- Qual arquivo melhor representa a regra central da API?
- Onde seria feita uma alteração para adicionar um novo campo à resposta?
- Qual trecho do código melhor representa uma boa solução?
- Qual trecho funciona, mas ainda precisa de refatoração?

---

## Perguntas para Validar Precisão Conceitual

### Prisma
> **Pergunta:** “Como o Prisma participa da aplicação?”
>
> **Objetivo:** Verificar se existe clareza de que Prisma é um ORM e uma camada de acesso/modelagem, e não o banco de dados em si.

### Axios
> **Pergunta:** “O que exatamente o Axios faz no projeto?”
>
> **Objetivo:** Verificar se há entendimento de que Axios é um cliente HTTP usado para realizar requisições, e não algo ligado especificamente a “HTTPS” como conceito isolado.

### Separação entre Frontend e API
> **Pergunta:** “Por que foi importante separar frontend e API?”
>
> **Objetivo:** Verificar entendimento sobre desacoplamento, centralização de regras, padronização de resposta, proteção contra mudanças externas e possibilidade de cache.

### Persistência
> **Pergunta:** “Por que foi necessário banco de dados em vez de buscar tudo online a cada requisição?”
>
> **Objetivo:** Verificar entendimento sobre persistência, histórico, performance, independência parcial da fonte externa e possibilidade de enriquecimento de dados.

### TypeScript - Interfaces vs Types
> **Pergunta:** “Quando você escolhe usar uma `interface` em vez de um `type` (ou vice-versa) no TypeScript?”
>
> **Objetivo:** Avaliar se o candidato conhece as sutilezas de extensibilidade (declaration merging) e a intenção semântica de cada um.

### TypeScript - Generics
> **Pergunta:** “Para que servem os Generics (`<T>`) e como eles ajudaram na tipagem de funções ou componentes do projeto?”
>
> **Objetivo:** Verificar se o desenvolvedor entende como criar código reutilizável mantendo a segurança de tipos, sem recorrer ao `any`.

### TypeScript - Union vs Intersection Types
> **Pergunta:** “Qual a diferença prática entre usar `|` (Union) e `&` (Intersection) na definição de um tipo?”
>
> **Objetivo:** Validar o domínio sobre composição de tipos e como o TypeScript lida com a lógica de conjuntos.

### TypeScript - Utility Types (Pick/Omit)
> **Pergunta:** “Em que cenário faria sentido usar `Pick<T, K>` ou `Omit<T, K>` em vez de criar um novo tipo do zero?”
>
> **Objetivo:** Verificar maturidade no uso das ferramentas nativas do TS para evitar duplicação de definições de tipos.

### React - Virtual DOM
> **Pergunta:** “Como o React decide o que precisa ser atualizado na tela quando o estado muda?”
>
> **Objetivo:** Checar o entendimento sobre o processo de *reconciliation* e por que o React é eficiente em atualizações de interface.

### React - Hooks (Regras)
> **Pergunta:** “Por que não podemos chamar um Hook dentro de um `if` ou de um `loop`?”
>
> **Objetivo:** Validar o conhecimento sobre a ordem de execução dos Hooks e como o React rastreia internamente o estado.

### React - Context API
> **Pergunta:** “Quando você opta por usar Context API em vez de apenas passar props (prop drilling)?”
>
> **Objetivo:** Avaliar a capacidade de arquitetar o fluxo de dados e identificar problemas de performance ou complexidade.

### React - Keys em Listas
> **Pergunta:** “Por que é desencorajado usar o `index` do array como `key` em listas dinâmicas?”
>
> **Objetivo:** Verificar se o candidato entende como o React identifica mudanças de posição e identidade de elementos na árvore.

### Angular - Dependency Injection
> **Pergunta:** “Como o sistema de Injeção de Dependência do Angular influencia na testabilidade do código?”
>
> **Objetivo:** Validar o entendimento sobre desacoplamento e o papel do `@Injectable`.

### Angular - Observables (RxJS)
> **Pergunta:** “Qual a vantagem de usar um `Observable` em vez de uma `Promise` comum para lidar com requisições HTTP?”
>
> **Objetivo:** Avaliar o conhecimento sobre fluxos de dados, cancelamento de requisições e operadores de transformação.

### Angular - Components vs Directives
> **Pergunta:** “Um Componente é considerado uma Diretiva? Qual a diferença fundamental entre eles?”
>
> **Objetivo:** Checar o entendimento da hierarquia e do propósito de cada elemento na estruturação da UI.

### Angular - Lifecycle Hooks
> **Pergunta:** “Qual a diferença entre o `ngOnInit` e o construtor da classe do componente?”
>
> **Objetivo:** Verificar se o desenvolvedor sabe em que momento os inputs estão disponíveis e quando a inicialização do Angular de fato ocorre.

### Vue - Sistema de Reatividade
> **Pergunta:** “Como o Vue 3 consegue detectar mudanças em objetos e arrays para atualizar a tela?”
>
> **Objetivo:** Validar o conhecimento sobre `Proxy` (no Vue 3) e o rastreamento automático de dependências.

### Vue - Options vs Composition API
> **Pergunta:** “Por que a Composition API foi introduzida no Vue 3 e que problema ela resolve em relação à Options API?”
>
> **Objetivo:** Avaliar a capacidade de organizar a lógica por funcionalidades em vez de tipos de opções (data, methods, etc.).

### Vue - Computed vs Watch
> **Pergunta:** “Quando é melhor usar uma propriedade `computed` e quando é necessário usar um `watch`?”
>
> **Objetivo:** Verificar o entendimento sobre cache de valores reativos e execução de efeitos colaterais.

### Vue - Slots
> **Pergunta:** “Para que servem os `slots` e como eles ajudam na criação de componentes genéricos (como modais ou layouts)?”
>
> **Objetivo:** Checar o conhecimento sobre composição de componentes e distribuição de conteúdo.

### PHP - Traits
> **Pergunta:** “O PHP não suporta herança múltipla. Como as `Traits` ajudam a contornar essa limitação?”
>
> **Objetivo:** Avaliar o conhecimento sobre reutilização de código e composição horizontal de classes.

### PHP - Namespaces
> **Pergunta:** “Qual a importância do uso de `Namespaces` em projetos PHP modernos e como eles se relacionam com o autoloader?”
>
> **Objetivo:** Validar o entendimento sobre organização de código e prevenção de colisões de nomes de classes.

### PHP - Composer
> **Pergunta:** “O que acontece 'por baixo dos panos' quando rodamos o comando `composer dump-autoload`?”
>
> **Objetivo:** Verificar se o desenvolvedor entende o ecossistema de dependências e como o PHP localiza as classes instaladas.

### PHP - Dependency Injection
> **Pergunta:** “Que benefício temos ao tipar uma dependência no construtor de uma Controller em frameworks como Laravel ou Symfony?”
>
> **Objetivo:** Validar o conhecimento sobre o Service Container e a resolução automática de dependências.

---

## Simulação de Banca — Sequência de 20 Perguntas

1. Apresente o projeto em até 2 minutos.
2. Qual problema ele resolve?
3. Quais tecnologias foram usadas e por quê?
4. Como a arquitetura está dividida?
5. Por que foi criada uma API própria?
6. Como essa API funciona em alto nível?
7. Que dados eram buscados e de onde vinham?
8. Quais dificuldades surgiram ao integrar com a fonte externa?
9. O que é Axios dentro desse contexto?
10. O que é Prisma dentro desse contexto?
11. Qual banco de dados está sendo utilizado?
12. Como as tabelas foram modeladas?
13. Qual problema surgiu com IDs diferentes?
14. Como esse problema foi tratado?
15. O que foi mais difícil: frontend, backend ou modelagem?
16. O que seria feito de forma diferente se o projeto começasse hoje?
17. Quais pontos ainda precisam de estudo ou amadurecimento?
18. O que foi aprendido em JavaScript/React durante o processo?
19. O que impediria esse sistema de ir para produção hoje?
20. Qual parte da entrega melhor representa o potencial técnico demonstrado?

---

## Critérios de Avaliação das Respostas

Ao ouvir as respostas, observar:
- **Clareza:** A explicação é organizada ou confusa?
- **Precisão:** Os termos técnicos são usados corretamente?
- **Honestidade:** Existe transparência ao falar sobre limitações e dúvidas?
- **Entendimento:** A resposta mostra causa e efeito ou apenas descrição superficial?
- **Evolução:** Existe capacidade de reconhecer erros e explicar como melhoraria?

---

## Estrutura Ideal de Resposta Técnica

Ao responder qualquer pergunta, uma boa estrutura é:
1. **O que é**
2. **Como funciona no projeto**
3. **Por que foi escolhido**
4. **Qual limitação ou trade-off existe**

**Exemplo:**
*“Axios é uma biblioteca para fazer requisições HTTP. No projeto, ele foi usado para buscar dados de uma fonte externa e alimentar a API. Foi escolhido por praticidade no tratamento de respostas e erros. A limitação é que ele não resolve sozinho problemas de latência, indisponibilidade ou bloqueio da fonte.”*

---

## Conclusão

O objetivo desta avaliação é identificar não apenas a capacidade de construir algo funcional, mas também a capacidade de defender decisões técnicas, explicar arquitetura, reconhecer limitações e comunicar a solução de forma madura.

**Em uma apresentação técnica, código funcional importa muito. Mas entendimento, clareza e raciocínio técnico importam tanto quanto.**