
Documento de Requisitos do Produto (PRD)
Sistema Web de Controle de Despesas
1. Visão Geral do Produto
Nome do Produto: Sistema de Controle de Despesas
Tipo: Aplicação Web
Objetivo Principal: Fornecer uma interface web simples e eficiente para gerenciamento e resumo de despesas pessoais, permitindo o registro de despesas com categorias e valores, e o cálculo automático de totais.

2. Objetivos de Negócio
Fornecer uma ferramenta intuitiva para controle financeiro pessoal
Permitir visualização rápida do resumo de gastos por categoria
Garantir validação de dados para manter a integridade das informações
Oferecer feedback imediato ao usuário sobre operações inválidas
3. Requisitos Funcionais
3.1 Registro de Despesas
RF-001: Adicionar Nova Despesa

O sistema deve permitir ao usuário inserir uma nova despesa com os seguintes campos:
Categoria: texto obrigatório (ex: Alimentação, Transporte, Moradia, Lazer, Saúde, Educação, Outros)
Descrição: texto opcional para detalhar a despesa
Valor: número decimal positivo obrigatório (formato: R$ 0,00)
Data: data da despesa (padrão: data atual)
RF-002: Validação de Valores Negativos

O sistema DEVE rejeitar qualquer tentativa de inserção de valor negativo
O sistema DEVE exibir mensagem de erro clara: "O valor da despesa não pode ser negativo"
O sistema DEVE rejeitar o valor zero como despesa válida
A validação deve ocorrer antes do processamento da transação
RF-003: Lista de Despesas Vazia

Quando não houver despesas cadastradas, o sistema DEVE exibir total geral igual a zero
O sistema DEVE exibir mensagem amigável: "Nenhuma despesa cadastrada"
Os totais por categoria não devem ser exibidos quando a lista estiver vazia
3.2 Resumo de Despesas
RF-004: Cálculo de Total por Categoria

O sistema deve calcular e exibir o somatório de despesas agrupadas por categoria
Formato de exibição: lista com nome da categoria e valor total formatado em Reais (R$)
As categorias devem ser ordenadas alfabeticamente
Categorias sem despesas não devem ser exibidas no resumo
RF-005: Cálculo do Total Geral

O sistema deve calcular e exibir o somatório de todas as despesas
O valor deve ser formatado em Reais (R$) com duas casas decimais
O total geral deve ser atualizado em tempo real a cada nova adição ou remoção de despesa
RF-006: Visualização do Resumo

O sistema deve exibir um painel de resumo contendo:
Lista de categorias com seus respectivos totais
Total geral em destaque
Quantidade total de despesas registradas
O resumo deve ser atualizado automaticamente após cada operação
3.3 Gerenciamento de Despesas
RF-007: Listagem de Despesas

O sistema deve exibir todas as despesas cadastradas em formato de tabela
Colunas: Data, Categoria, Descrição, Valor
Ordenação padrão: por data (mais recente primeiro)
Possibilidade de ordenar por outras colunas
RF-008: Remoção de Despesas

O sistema deve permitir a exclusão de despesas individuais
Confirmação necessária antes da exclusão
Atualização automática dos totais após exclusão
4. Requisitos Não-Funcionais
4.1 Usabilidade
RNF-001: Interface responsiva e amigável, compatível com dispositivos móveis e desktop
RNF-002: Feedback visual imediato para ações do usuário (loading, sucesso, erro)
RNF-003: Mensagens de erro claras e orientativas
RNF-004: Formulário com validação em tempo real

4.2 Performance
RNF-005: Tempo de resposta inferior a 2 segundos para operações CRUD
RNF-006: Cálculos de totais devem ser processados em tempo real
RNF-007: Suporte a pelo menos 1000 despesas sem degradação de performance

4.3 Segurança
RNF-008: Sanitização de todas as entradas para prevenir injeção de código
RNF-009: Validação server-side de todos os dados recebidos
RNF-010: Proteção contra CSRF (Cross-Site Request Forgery)

4.4 Acessibilidade
RNF-011: Conformidade com WCAG 2.1 nível AA
RNF-012: Navegação completa via teclado
RNF-013: Suporte a leitores de tela

5. Casos de Uso
Caso de Uso 1: Adicionar Despesa Válida
Ator: Usuário
Pré-condição: Usuário está na página principal
Fluxo Principal:

Usuário clica em "Adicionar Despesa"
Sistema exibe formulário
Usuário preenche: Categoria="Alimentação", Descrição="Almoço", Valor=25.50
Usuário clica em "Salvar"
Sistema valida os dados
Sistema armazena a despesa
Sistema atualiza a lista e os totais
Sistema exibe mensagem de sucesso
Resultado: Despesa adicionada, totais atualizados

Caso de Uso 2: Tentar Adicionar Valor Negativo
Ator: Usuário
Pré-condição: Usuário está no formulário de adição
Fluxo Principal:

Usuário preenche: Categoria="Transporte", Valor=-15.00
Usuário clica em "Salvar"
Sistema valida e detecta valor negativo
Sistema exibe mensagem: "O valor da despesa não pode ser negativo"
Sistema mantém o formulário aberto com os dados preenchidos
Resultado: Despesa não adicionada, erro exibido

Caso de Uso 3: Visualizar Resumo com Lista Vazia
Ator: Usuário
Pré-condição: Nenhuma despesa cadastrada
Fluxo Principal:

Usuário acessa a página principal
Sistema verifica que não há despesas
Sistema exibe: "Nenhuma despesa cadastrada"
Sistema exibe Total Geral: R$ 0,00
Resultado: Resumo exibido com valor zero

Caso de Uso 4: Visualizar Resumo com Múltiplas Despesas
Ator: Usuário
Pré-condição: Existem despesas cadastradas de diferentes categorias
Fluxo Principal:

Usuário acessa a página principal
Sistema carrega todas as despesas
Sistema agrupa por categoria e calcula totais
Sistema exibe lista de categorias com totais
Sistema exibe total geral
Resultado: Resumo completo exibido com totais por categoria e geral

6. Regras de Negócio
RN-001: O valor da despesa deve ser sempre maior que zero
RN-002: A categoria é obrigatória e deve conter pelo menos 2 caracteres
RN-003: Valores devem ser formatados com duas casas decimais
RN-004: O separador decimal é vírgula (,) e o de milhar é ponto (.)
RN-005: Moeda padrão: Real Brasileiro (R$)
RN-006: Não é permitido duplicidade de despesas idênticas (mesma data, categoria, descrição e valor)
RN-007: Despesas removidas não podem ser recuperadas após confirmação

7. Especificações Técnicas
7.1 Arquitetura Sugerida
Frontend:

HTML5 semântico
CSS3 com framework responsivo (Bootstrap ou Tailwind)
JavaScript vanilla ou framework leve (React, Vue)
Validação client-side e server-side
Backend:

API RESTful
Endpoints:
POST /api/expenses - Adicionar despesa
GET /api/expenses - Listar despesas
GET /api/expenses/summary - Obter resumo
DELETE /api/expenses/{id} - Remover despesa
Banco de Dados:

Estrutura sugerida:
yaml
copy
download
expenses: {
  id: integer,
  category: string,
  description: string,
  amount: decimal(10,2),
  date: datetime,
  created_at: datetime
}
7.2 Estrutura de Dados para Resposta da API
Resposta do endpoint /api/expenses/summary:

json
copy
download
{
  "total_geral": 150.00,
  "quantidade_despesas": 5,
  "totais_por_categoria": {
    "Alimentação": 75.50,
    "Transporte": 45.00,
    "Lazer": 29.50
  }
}
8. Critérios de Aceitação
CA-001: Sistema aceita despesas com valor positivo e rejeita valores negativos ou zero
CA-002: Sistema exibe total geral correto somando todas as despesas
CA-003: Sistema agrupa e calcula totais por categoria corretamente
CA-004: Sistema exibe valor zero quando não há despesas cadastradas
CA-005: Sistema exibe mensagens de erro claras para entradas inválidas
CA-006: Interface é responsiva e funciona em dispositivos móveis
CA-007: Todos os cálculos são atualizados em tempo real
CA-008: Sistema permite adicionar, visualizar e remover despesas

9. Fluxo de Dados
arduino
copy
download
Usuário → Formulário → Validação Client → API → Validação Server → Banco de Dados
                                                                        ↓
Usuário ← Interface ← Atualização UI ← Cálculos ← Resposta API ← Persistência
10. Métricas de Sucesso
Taxa de sucesso de validação: > 95% das entradas devem passar na validação sem erros
Tempo médio de operação: < 2 segundos para adicionar/listar/resumo
Satisfação do usuário: Nota > 4/5 em pesquisa de usabilidade
Taxa de erro de validação: < 5% de tentativas com valores inválidos
11. Roadmap de Implementação
Fase 1 - MVP (2 semanas):

Interface básica de formulário
Validação de valores negativos
Cálculo de total geral
Persistência em memória
Fase 2 - Funcionalidades Essenciais (2 semanas):

Listagem de despesas
Cálculo de totais por categoria
Persistência em banco de dados
Mensagens de erro
Fase 3 - Refinamento (1 semana):

Design responsivo
Melhorias de usabilidade
Testes de integração
Deploy em produção
12. Riscos e Mitigações
Risco	Probabilidade	Impacto	Mitigação
Perda de dados	Baixa	Alto	Backup automático, validações rigorosas
Injeção de código	Média	Alto	Sanitização de entradas, prepared statements
Performance degradada	Baixa	Médio	Paginação, otimização de queries
Incompatibilidade de navegadores	Baixa	Baixo	Testes cross-browser, uso de polyfills
13. Aprovação
Produto: [A definir]
Data: [A definir]
Stakeholders: [A definir]

