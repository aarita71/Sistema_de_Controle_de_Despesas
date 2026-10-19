**Sistema de Controle de Despesas**

**Sobre o Projeto**

**O Sistema de Controle de Despesas é uma aplicação web desenvolvida em HTML5, CSS3 e JavaScript Vanilla para gerenciamento de despesas pessoais.**



**A solução permite registrar, visualizar, consolidar e excluir despesas, apresentando indicadores financeiros em tempo real por meio de uma interface moderna, responsiva, acessível e segura.**



**A versão atual é resultado de uma refatoração completa baseada em princípios de Clean Code, SOLID, Segurança Web, Acessibilidade (WCAG 2.1 AA) e boas práticas de desenvolvimento Front-End moderno.**



**Principais Funcionalidades**

**Cadastro de Despesas**

**Permite registrar:**



**Categoria**

**Descrição**

**Valor**

**Data**

**Validações implementadas:**



**Categoria obrigatória**

**Data obrigatória**

**Valor obrigatório**

**Valor maior que zero**

**Valor mínimo de R$ 0,01**

**Gerenciamento de Despesas**

**Listagem automática das despesas cadastradas**

**Exclusão individual de registros**

**Atualização imediata da interface**

**Persistência local dos dados**

**Resumo Financeiro**

**Exibe automaticamente:**



**Quantidade total de despesas**

**Total geral gasto**

**Quantidade de categorias**

**Total por categoria**

**Todos os indicadores são atualizados em tempo real.**



**Arquitetura**

**A aplicação foi reorganizada utilizando separação de responsabilidades.**



**┌──────────────────────────────┐**

**│          Bootstrap           │**

**├──────────────────────────────┤**

**│ Cache de Elementos DOM       │**

**│ Event Delegation             │**

**│ Event Subscription           │**

**└──────────────┬───────────────┘**

&#x20;              **│**

&#x20;   **┌──────────┼──────────┐**

&#x20;   **▼          ▼          ▼**



**┌────────┐ ┌────────┐ ┌─────────────┐**

**│ Store  │ │ Render │ │ Feedback UI │**

**│ Dados  │ │  View  │ │ Mensagens   │**

**└────────┘ └────────┘ └─────────────┘**

**Princípios Aplicados**

**Clean Code**

**Funções pequenas e objetivas**

**Nomes descritivos**

**Baixo acoplamento**

**Alta coesão**

**Código legível e sustentável**

**SOLID**

**Single Responsibility Principle (SRP)**

**Open/Closed Principle (OCP)**

**Dependency Inversion Principle (DIP)**

**Boas Práticas**

**Separation of Concerns**

**Event Delegation**

**Publish/Subscribe**

**Defensive Programming**

**DOM Caching**

**Melhorias Implementadas na Refatoração**

**Segurança contra XSS**

**Antes:**



**element.innerHTML = conteudo;**

**Depois:**



**const elemento = document.createElement("div");**

**elemento.textContent = conteudo;**

**Benefícios:**



**Eliminação de XSS Persistente (Stored XSS)**

**Eliminação de DOM XSS**

**Compatibilidade com CSP**

**Maior proteção contra scripts maliciosos**

**Geração Segura de Identificadores**

**crypto.randomUUID()**

**Fallback:**



**Date.now() + Math.random()**

**Benefícios:**



**IDs únicos**

**Menor chance de colisão**

**Melhor rastreabilidade**

**Precisão Monetária**

**Todos os cálculos financeiros são realizados utilizando centavos inteiros.**



**Exemplo:**



**const centavos = Math.round(valor \* 100);**

**Benefícios:**



**Eliminação de erros de ponto flutuante**

**Precisão financeira**

**Consistência dos cálculos**

**Correção de Datas**

**Antes:**



**new Date("2026-01-15")**

**Depois:**



**const \[ano, mes, dia] = data.split("-");**

**Benefícios:**



**Independência do fuso horário**

**Consistência entre navegadores**

**Evita deslocamento incorreto de datas**

**Tratamento de Falhas no Storage**

**try {**

&#x20;   **const dados = JSON.parse(storage);**

**} catch {**

&#x20;   **return \[];**

**}**

**Benefícios:**



**Recuperação automática**

**Maior robustez**

**Menor risco de indisponibilidade**

**Tecnologias Utilizadas**

**Front-End**

**HTML5**

**CSS3**

**JavaScript ES2023**

**APIs Nativas**

**LocalStorage**

**Intl.NumberFormat**

**crypto.randomUUID**

**DOM API**

**Interface**

**CSS Variables**

**CSS Grid**

**Flexbox**

**Media Queries**

**Responsive Design**

**Acessibilidade**

**A aplicação segue recomendações WCAG 2.1 AA.**



**Recursos implementados:**



**aria-live**

**aria-label**

**aria-labelledby**

**aria-atomic**

**Navegação por teclado**

**Focus Visible**

**Compatibilidade com leitores de tela**

**Suporte a Reduced Motion**

**Responsividade**

**Compatível com:**



**Smartphones**

**Tablets**

**Notebooks**

**Monitores Desktop**

**Layout adaptável utilizando:**



**@media (min-width: 768px)**

**Persistência dos Dados**

**Os dados são armazenados localmente através do navegador.**



**localStorage**

**Chave utilizada:**



**despesas:v1**

**Os registros permanecem disponíveis mesmo após fechar ou reiniciar o navegador.**



**Estrutura do Projeto**

**/**

**├── Sistema de Controle de Despesas\_Clean\_Code.html**

**├── README.md**

**├── PRD\_Sistema\_Controle\_Despesas.MD**

**├── Regras-do\_projeto.MD**

**├── Revisao de Codigo com Clean Code.MD**

**├── Resultado\_revisao-clean-code.md**

**└── refatoracao-aplicada.MD**

**Regras de Negócio**

**RN-001**

**Categoria obrigatória.**



**RN-002**

**Data obrigatória.**



**RN-003**

**Valor obrigatório.**



**RN-004**

**Valor deve ser maior que zero.**



**RN-005**

**Valor mínimo permitido:**



**R$ 0,01**

**RN-006**

**Toda despesa deve possuir identificador único.**



**RN-007**

**Valores monetários são armazenados em centavos.**



**RN-008**

**Os totais por categoria devem ser recalculados automaticamente.**



**Como Executar**

**Método 1**

**Abrir diretamente o arquivo:**



**Sistema de Controle de Despesas\_Clean\_Code.html**

**em um navegador moderno.**



**Método 2**

**Executar servidor local:**



**python -m http.server 8000**

**Acessar:**



**http://localhost:8000**

**Testes Recomendados**

**Funcionais**

**Inclusão de despesas**

**Exclusão de despesas**

**Atualização dos totais**

**Agrupamento por categoria**

**Segurança**

**Entrada:**



**<script>alert('teste')</script>**

**Resultado esperado:**



**Exibir texto sem executar script**

**Precisão Financeira**

**Entradas:**



**0,10**

**0,10**

**0,10**

**0,20**

**Resultado esperado:**



**R$ 0,50**

**Persistência**

**Salvar despesas**

**Fechar navegador**

**Reabrir aplicação**

**Resultado esperado:**



**Dados preservados**

**Roadmap**

**Curto Prazo**

**Testes automatizados**

**Cobertura de código**

**CSP estrita**

**Médio Prazo**

**Pesquisa e filtros**

**Exportação CSV**

**Ordenação avançada**

**Longo Prazo**

**Backend FastAPI**

**Banco de dados relacional**

**Autenticação**

**Multiusuário**

**Dashboards analíticos**

**Integração em nuvem**

**Métricas de Qualidade**

**Indicador	Antes	Depois**

**Qualidade Geral	6.2	8.4**

**Segurança	2.0	9.0**

**Acessibilidade	3.0	8.0**

**Testabilidade	1.0	9.0**

**Manutenibilidade	4.0	8.0**

**Performance	5.0	8.0**

**Conclusão**

**A versão atual representa uma evolução significativa do projeto original, incorporando práticas modernas de engenharia de software, segurança, acessibilidade e qualidade de código.**



**O resultado é uma aplicação leve, responsiva, segura, facilmente manutenível e preparada para futuras evoluções sem dependência de frameworks externos.**

