# AFBuilder

> **Construtor visual completo de Autômatos Finitos** — AFD, AFND e AFND-ε com simulação, minimização, conversão e operações sobre linguagens.

---

## 📸 Visão Geral

O **AFBuilder** é uma ferramenta interativa de uso acadêmico para construção, simulação e análise de autômatos finitos. Desenvolvida como arquivo HTML único sem dependências externas, roda direto no navegador.

Ideal para disciplinas de **Teoria da Computação**, **Linguagens Formais** e **Compiladores**.

---

## ✨ Funcionalidades

### 🏗️ Construção Visual
- Criação de estados por **clique no canvas** ou via formulário lateral
- Adição de **transições normais**, **múltiplos símbolos por seta** e **ε-transições**
- **Arrastar e reposicionar** estados livremente no canvas
- **Duplo-clique** para editar qualquer estado ou transição
- **Botão direito** com menu de contexto completo (editar, tornar inicial/aceitação/morto, calcular ε-fecho, excluir)
- **Auto Layout** — organiza os estados em elipse automaticamente

### 🔵 Tipos de Autômato
| Tipo | Suporte |
|------|---------|
| AFD — Autômato Finito Determinístico | ✅ |
| AFND — Não-Determinístico (sem ε) | ✅ |
| AFND-ε — Não-Determinístico com ε-transições | ✅ |

> O badge no header detecta automaticamente se o autômato é **AFD** ou **AFND**.

### ▶️ Simulação
- **Teste individual** — simula palavra a palavra com trace visual passo a passo
- **Teste em lote** — cole várias palavras e veja ✓/✗ para cada uma
- Suporte a **palavra vazia** (`ε` ou campo em branco, ou `epsilon` no lote)
- A simulação usa **conjunto de estados ativos** (algoritmo correto para AFND)
- O trace exibe os **passos de ε-fecho** intermediários em destaque

### 🔢 Algoritmos Implementados

#### ε-Fecho
- Calcula `ε-Fecho(q)` para cada estado
- Usado internamente na simulação AFND e na conversão AFND→AFD
- Visualizado na aba **ε-Fecho** do painel direito

#### ⇒ Conversão AFND → AFD (Construção de Subconjuntos)
- Cada estado do AFD resultante = subconjunto de estados do AFND
- Exibe todos os passos `δ({q0,q1}, a) = {q2}` calculados
- Botão para **carregar o AFD resultante direto no canvas**

#### ✔ Completar AFD
- Adiciona estado morto `qd` para todas as transições indefinidas
- Self-loop em `qd` para todo símbolo do alfabeto
- Pré-requisito automático para Complemento e Minimização

#### ⊟ Minimização (Algoritmo de Hopcroft)
- Partição inicial: `{estados de aceitação}` vs `{demais}`
- Refinamento iterativo por símbolo até estabilização
- Exibe cada **split** da partição passo a passo
- Botão para **carregar o AFD mínimo no canvas**

#### ∁ Complemento
- Inverte todos os estados de aceitação
- Completa o AFD automaticamente antes de aplicar

#### ≡ Verificação de Equivalência
- Cole o JSON de um segundo autômato e compare com o atual
- Testa palavras de comprimento 0 a 5 sobre o alfabeto union
- Aponta a palavra testemunha da diferença (se existir)

### 📊 Tabela δ
- Gerada automaticamente em tempo real
- Células com **múltiplos destinos** destacadas (AFND)
- Células com **transição faltando** (∅) marcadas em vermelho
- Coluna `ε` aparece automaticamente quando há ε-transições

### 🔎 Validação
- Detecta estado inicial ausente
- Detecta ausência de estados de aceitação
- Lista **estados inalcançáveis**
- Informa se o AFD está **completo ou incompleto**

### 💾 Exportar / Importar
- Exporta o autômato como arquivo **JSON**
- Importa JSON para restaurar autômatos salvos
- Formato compatível entre AFD e AFND

---

## 🚀 Como Usar

### Opção 1 — Abrir direto no navegador
```bash
# Não requer servidor. Basta abrir o arquivo:
open af_builder.html
# ou arraste o arquivo para o navegador
```

### Opção 2 — Clonar e servir localmente
```bash
git clone https://github.com/seu-usuario/af_builder.git
cd automata-lab

# Com Python:
python -m http.server 8080

# Com Node:
npx serve .
```
Acesse `http://localhost:8080` no navegador.

---

## 📖 Guia Rápido

### Criando um AFD do zero

1. **Defina o alfabeto** no campo "Alfabeto" (ex: `0, 1`)
2. **Adicione estados** clicando no canvas ou pelo formulário lateral
   - Marque o estado inicial com `→ Inicial`
   - Marque os estados de aceitação com `✓ Aceitação`
3. **Adicione transições** no modo `→ Transição`: clique no estado origem → clique no destino → informe o símbolo
4. **Teste palavras** no painel direito, aba `▶ Testar`

### Criando um AFND-ε

- Use o modo `ε Epsilon` para adicionar ε-transições (aparecem em roxo tracejado)
- O badge no header muda automaticamente para **AFND**
- A simulação lida automaticamente com o ε-fecho

### Minimizando um AFD

1. Certifique-se de que o autômato é um **AFD determinístico**
2. Clique em `⊟ Minimizar` no header (ou na aba "⊟ Minimizar")
3. Veja os passos do algoritmo de Hopcroft
4. Clique em **"Carregar AFD Mínimo"** para substituir o autômato atual

### Convertendo AFND → AFD

1. Construa o AFND normalmente
2. Clique em `⇒ AFND→AFD` no header
3. Veja as transições calculadas pela construção de subconjuntos
4. Clique em **"Carregar no Canvas"**

## 📐 Formato JSON

Os autômatos exportados seguem este formato:

```json
{
  "name": "L = { w | w ∈ {0,1}* | termina em '01' }",
  "alphabet": ["0", "1"],
  "nodes": [
    { "id": "n0", "name": "q0", "x": 120, "y": 200, "start": true,  "accept": false, "dead": false },
    { "id": "n1", "name": "q1", "x": 300, "y": 200, "start": false, "accept": false, "dead": false },
    { "id": "n2", "name": "q2", "x": 480, "y": 200, "start": false, "accept": true,  "dead": false }
  ],
  "transitions": [
    { "id": "t0", "from": "n0", "to": "n0", "symbols": ["1"] },
    { "id": "t1", "from": "n0", "to": "n1", "symbols": ["0"] },
    { "id": "t2", "from": "n1", "to": "n1", "symbols": ["0"] },
    { "id": "t3", "from": "n1", "to": "n2", "symbols": ["1"] },
    { "id": "t4", "from": "n2", "to": "n2", "symbols": ["0", "1"] }
  ]
}
```

> Use `"symbols": ["ε"]` para ε-transições.

---

## 🎓 Contexto Acadêmico

Este projeto cobre os principais tópicos de **Linguagens Formais e Autômatos**:

- Definição formal de AFD: M = (Q, Σ, δ, q₀, F)
- Autômatos Finitos Não-Determinísticos (AFND)
- ε-transições e ε-fecho
- Equivalência AFND ≡ AFD (Teorema de Rabin-Scott)
- Minimização de AFD (Algoritmo de Hopcroft)
- Operações sobre linguagens regulares: complemento, união, interseção

