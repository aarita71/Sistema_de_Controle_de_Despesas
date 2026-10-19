# 📋 Revisão de Código — Sistema de Controle de Despesas
**Arquivo:** `sistema_despesas.html` (607 linhas) | **Contexto:** MVP de página única sem backend

---

## 📈 Resumo Executivo

**Score: 6,2 / 10** | **Severidade geral:** 🟡 MÉDIA

O código atende ao PRD (validação de negativos, lista vazia = total zero), mas possui **4 defeitos críticos que impedem a promoção a produção**: XSS armazenado, bug de timezone em datas, dinheiro em float com erro de precisão, e remoção por índice posicional frágil.

| Categoria | Status |
|---|---|
| ✅ Pontos fortes | Separação inicial dados/render · Validação dupla (HTML+JS) · Nomes coerentes |
| ❌ Pontos fracos | XSS em `innerHTML` · Float para dinheiro · Bug de data · Estado global mutável |

---

## 🔴 Top 5 Problemas Críticos

| # | Problema | Linha | Impacto |
|---|---|---|---|
| 1 | **XSS armazenado:** `description` e `category` injetados via `innerHTML` sem sanitização | L442, L490, L518, L523 | Script injetado no `localStorage` executa em todas as sessões futuras |
| 2 | **Bug de timezone:** `new Date("YYYY-MM-DD")` é UTC; exibe "15/10" em vez de "16/10" para UTC-3 | L398-401, L387, L597 | Data errada para usuário final |
| 3 | **Dinheiro em float:** `0.1 + 0.2 = 0.30000000000000004`; total pode fechar com centavos fantasmas | L474, L530, L560 | Totais incorretos em agregações |
| 4 | **Remoção por índice:** `splice(index)` aponta para despesa errada após reordenação | L506-523, L545-551 | Corrupção de dados |
| 5 | **`JSON.parse` sem `try/catch`:** storage corrompido quebra o app no boot | L375 | App indisponível sem intervenção manual |

---

## 🏆 Top 10 Melhorias Prioritárias

| # | Melhoria | Severidade | Esforço | Impacto |
|---|---|---|---|---|
| 1 | Substituir `innerHTML`/`onclick` inline por `textContent` + delegação de eventos | 🔴 | Médio | Alto |
| 2 | Corrigir `formatDate` (split da string ISO) e `valueAsDate` (construtor local) | 🔴 | Baixo | Alto |
| 3 | Armazenar em centavos inteiros via `parseAmountToCents` com retorno `{ok, cents}` / `{ok, reason}` | 🔴 | Médio | Alto |
| 4 | Extrair Store puro (agregações em função única) + `try/catch` no storage + `STORAGE_KEY` versionada | 🔴 | Alto | Alto |
| 5 | Remoção por `id` com `filter` em vez de `splice(index)` | 🔴 | Baixo | Alto |
| 6 | `<main>` + `<section>` + `aria-live="polite"` em mensagens + `aria-label` contextual em "Remover" | 🔴 | Baixo | Alto |
| 7 | Consolidar timers com `clearTimeout` em `showError`/`showSuccess` | 🟡 | Baixo | Médio |
| 8 | Design tokens em CSS Custom Properties (`--color-primary`, etc.) + `:focus-visible` | 🟡 | Baixo | Médio |
| 9 | Quebrar submit monolítico em `readDraft` / `validate` / `handleSubmit` | 🟡 | Médio | Médio |
| 10 | `crypto.randomUUID()` + JSDoc em funções públicas + remover comentários redundantes | 🟢 | Baixo | Baixo |

---

## 📝 Checklist Final

| Critério | Status |
|---|---|
| SOLID | ❌ REPROVADO — Estado global mutável; render faz agregação (SRP/OCP) |
| Funções pequenas | ⚠️ ATENÇÃO — `updateUI` ok, mas submit tem 45 linhas |
| Nomes significativos | ✅ APROVADO — ressalva: `categories` é ambíguo entre contextos |
| Sem duplicação (DRY) | ❌ REPROVADO — `reduce` em 2 lugares; `#667eea` em 11 ocorrências |
| Tratamento de erros | ⚠️ ATENÇÃO — Validação existe, mas sem `try/catch` no storage |
| HTML semântico/a11y | ❌ REPROVADO — Sem `<main>`, `aria-live`, `<time>` |
| CSS organizado | ⚠️ ATENÇÃO — Sem tokens, sem `focus-visible`, desktop-first |
| JS moderno | ⚠️ ATENÇÃO — `const`/`let` ✅; `onclick` inline ❌; sem módulos ❌ |
| Performance | ⚠️ ATENÇÃO — `new Date` por comparação; agregações recalculadas |
| Segurança | ❌ REPROVADO — XSS armazenado + storage sem schema |
| Testável | ❌ REPROVADO — Lógica presa em funções de render |
| Documentação | ⚠️ ATENÇÃO — Sem JSDoc; comentários redundantes no lugar da API doc |

---

## 🎯 Plano de Ação

### 🚀 Curto Prazo — Quick Wins (1-2h, alto impacto)
1. Corrigir `formatDate` (split da string) e `valueAsDate` — **bug de data visível hoje**
2. Unificar `reduce` do total em função única — fim da divergência L474/L530
3. `clearTimeout` compartilhado nas mensagens
4. Adicionar `role="status" aria-live="polite"` + `<main>`/`<section>`
5. Converter para CSS Custom Properties
6. Renomear `categories` ambíguo e remover comentários "o que faz"

### 🔧 Médio Prazo — Refatoração Estrutural (1-2 dias)
7. `textContent` + delegação de eventos (elimina os 4 `innerHTML` + `onclick` global)
8. `parseAmountToCents` com retorno discriminado `{ok, ...}`
9. `removeExpenseById` com `filter` — apagar `splice(index)`
10. Quebrar submit em `readDraft` / `validate` / `handleSubmit`
11. `crypto.randomUUID()` + `try/catch` no storage

### 🏗️ Longo Prazo — Arquitetura (pós-MVP)
12. Extrair store para módulo puro (`expense-store.js`) — habilita testes unitários
13. `type="module"` + CSP estrita
14. Virtualização da lista para >200 itens (RNF-007 exige 1000)
15. Troca do `localStorage` por API REST (camada de domínio já preparada no passo 12)

---

## 💬 Parecer Final

O código é um **bom protótipo funcional** — validações centrais do PRD estão corretas e o visual é coerente. Para produção, as correções 🔴 **#1 (XSS), #2 (timezone), #3 (float) e #5 (índice)** são **inegociáveis**, não por estilismo Clean Code, mas porque hoje o app pode: executar script injetado, exibir data errada, somar centavos fantasmas, e remover a despesa errada.

Aplicando as 11 melhorias de curto+médio prazo, o score sobe para **~8,5/10** com esforço de menos de um dia. O único ❌ que exige arquitetura (testabilidade) se resolve com o passo 12.

---

*Relatório gerado em 2026-10-17 — Revisor: Engenheiro de Software Sênior (Clean Code / Frontend)*
