---
name: skill-aco
description: Ant Colony Optimization (ACO) - Otimização por Colônia de Formigas para resolver o Problema do Caixeiro Viajante (TSP). Usar quando o usuário precisar implementar o algoritmo ACO, entender seus parâmetros (α, β, ρ), calcular probabilidades, atualizar feromônios, usar sistema de roleta para seleção de caminhos, ou explicar a simulação passo a passo do ACO com formigas, evaporação e depósito de feromônio em grafos.
---

# Ant Colony Optimization (ACO)

---

## 1. Fundamentos Biológicos do ACO

### 1.1 Comportamento Social de Formigas

O comportamento observado em formigas reais demonstra um processo de otimização coletiva:

- **Início do processo**: Cada formiga segue um caminho de forma aleatória na busca por alimento
- **Após algum tempo**: As formigas tendem a convergir para um único caminho, considerado ótimo (menor caminho)
- **Comunicação indireta**: Cada formiga indica para as outras quão bom foi o caminho escolhido através de uma substância química chamada **feromônio**

### 1.2 O Experimento do Aquário

Em experimentos controlados, foi observado que:
- Formigas iniciam explorando aleatoriamente
- Formigas que encontram alimentos retornam ao ninho depositando feromônio
- Outras formigas detectam a trilha e seguem-na, reforçando-a ainda mais
- Com o tempo, a trilha de feromônio se torna mais forte no caminho mais curto

### 1.3 Princípios Fundamentais do ACO

Do comportamento biológico, dois comportamentos principais são observados:

1. **Construção de trilha**: Formigas constroem trilhas de feromônio
2. **Recrutamento em massa**: Uma formiga exploradora descobre a fonte de alimento, retorna ao ninho e libera feromônio, iniciando a formação de uma trilha. Outras formigas detectam, seguem e reforçam essa trilha.

---

## 2. O Problema do Caixeiro Viajante (TSP)

### 2.1 Definição

O Problema do Caixeiro Viajante consiste em encontrar o menor caminho possível para visitar um conjunto de cidades exatamente uma vez e retornar à cidade de origem.

### 2.2 Representação no ACO

- O problema é modelado como um **grafo completo**
- Cada vértice representa uma cidade
- Cada aresta representa uma conexão entre duas cidades
- As formigas "caminham" pelas arestas, construindo soluções

---

## 3. Funcionamento do Algoritmo ACO

### 3.1 Passos Fundamentais

**Passo 1 - Inicialização:**
- Em cada vértice (cidade) é colocada uma formiga

**Passo 2 - Construção de Caminhos:**
- Cada formiga constrói um caminho seguindo uma função de probabilidade
- A função de probabilidade depende do feromônio depositado em cada aresta

**Passo 3 - Atualização de Feromônio:**
- Após a construção de todos os caminhos, a intensidade do feromônio em cada aresta é atualizada
- A intensidade é proporcional à qualidade da solução gerada
- **Menor caminho = mais feromônio depositado**

**Passo 4 - Iteração:**
- Novas soluções são construídas seguindo uma função de densidade de probabilidade proporcional ao nível de feromônio

### 3.2 Conceitos de Probabilidade

**Relação com distância:**
- Quanto **maior a distância**, **menor a probabilidade** de uma formiga seguir um determinado caminho
- A distância é inversamente proporcional à probabilidade

**Relação com feromônio:**
- Quanto **maior a quantidade de feromônio**, **maior a probabilidade** de uma formiga seguir o caminho
- O feromônio representa a "memória coletiva" do enxame

---

## 4. Formulação Matemática do ACO

### 4.1 Cálculo da Probabilidade (Equação 1)

A probabilidade de uma formiga *k* seguir o caminho entre as cidades *x* e *y* é calculada por:

```
P(x,y) = τ(x,y) × η(x,y) / Σ [τ(x,i) × η(x,i)]
```

Onde:
- **τ(x,y)**: Intensidade de feromônio na aresta (x,y) - representa o histórico de sucesso
- **η(x,y)**: Visibilidade (heurística), geralmente 1/d(x,y) - informação local sobre a qualidade do caminho
- **τ(x,i) × η(x,i)**: Somatório do produto (feromônio × visibilidade) de todos os caminhos possíveis

### 4.2 Atualização do Feromônio (Equação 2)

Após cada iteração, o feromônio é atualizado seguindo um modelo específico que considera:
- O feromônio depositado pelas formigas
- A evaporação natural do feromônio ao longo do tempo

A fórmula geral considera:
- **Deposição**: Feromônio depositado proporcional à qualidade da solução
- **Evaporação**: Redução gradual do feromônio em todas as arestas

---

## 5. Evaporação do Feromônio

### 5.1 Conceito

A evaporação é um mecanismo crucial no ACO que:
- **Retira 1 feromônio de cada aresta por iteração**
- Funciona como um "mecanismo de esquecimento"
- Evita que erros iniciais ou caminhos subótimos se tornem verdades absolutas
- Permite que novas rotas sejam exploradas mesmo após terem sido abandonadas inicialmente

### 5.2 Importância

Sem evaporação:
- O algoritmo ficaria preso em soluções locais subótimas
- Não haveria adaptação a mudanças no ambiente
- Caminhos ruins continuariam sendo reforçados

Com evaporação:
- Feromônio acumulado em maus caminhos gradualmente desaparece
- Novos caminhos podem ser descobertos e explorados
- O algoritmo mantém flexibilidade e capacidade de adaptação

---

## 6. Procedimento Algorítmico

### 6.1 Pseudocódigo

```
PROCEDIMENTO [s*] = ACO(max_it)

1. INICIALIZAÇÃO:
   - inicialize τ(xy) // sempre com o mesmo valor inicial
   
2. COLOCAÇÃO INICIAL:
   - coloque cada formiga k em uma "cidade" selecionada aleatoriamente
   
3. ITERAÇÃO (t = 1 até max_it):
   
   a) PARA CADA FORMIGA:
      - construa um caminho usando (eq.1)
      - // Escolha probabilística baseada em feromônio e visibilidade
   
   b) AVALIAÇÃO:
      - avalie cada solução encontrada
      - calcule a qualidade (comprimento do caminho)
   
   c) ATUALIZAÇÃO GLOBAL:
      - atualize s* (melhor solução, caso necessário)
      - atualize τ(xy) usando (eq.2)
      - // Aqui ocorre a deposição e evaporação do feromônio
   
4. RETORNE s* (melhor solução encontrada)

FIM
```

### 6.2 Fluxo de Execução

1. **Inicialização**: Define valores iniciais de feromônio para todas as arestas
2. **Colocação**: Distribui as formigas aleatoriamente nas cidades
3. **Construção**: Cada formiga constrói seu caminho probabilisticamente
4. **Avaliação**: Calcula a qualidade de cada solução
5. **Atualização**: Reforça feromônio em boas soluções, aplica evaporação
6. **Repetição**: Volta ao passo 3 até atingir critério de parada

---

## 7. Simulação Prática - Exemplo Detalhado

### 7.1 Cenário Inicial

Considere o seguinte cenário com 6 formigas (F1 a F6):

**Configuração do problema:**
- 6 cidades: A, B, C, D, E, F
- A = Ninho (início)
- F = Alimento (destino)
- Cada formiga pode dar até 12 passos de cada vez
- Na primeira iteração, a escolha do caminho é aleatória

### 7.2 Regras da Simulação

- Cada formiga vai e volta pelo mesmo caminho
- Na primeira iteração: escolha aleatória
- Das iterações seguintes em diante: escolha baseada no feromônio
- Cada vez que uma formiga passa por uma aresta, deposita '1' feromônio
- Evaporação: retira 1 feromônio de cada aresta por iteração

### 7.3 Primeira Iteração - Fase de Ida

**Estado Inicial:**
Todas as arestas têm feromônio = 2 (valor inicial)

**Caminhos percorridos (escolha aleatória):**
- F1: Rota ABE (anda todos os 12 passos)
- F2: Rota ABC (anda apenas 8 passos, pode andar mais 4)
- F3: Rota ACD (anda apenas 6 passos, pode andar mais 6)
- F4, F5, F6: Seguem as mesmas rotas de F1, F2 e F3 respectivamente

**Resultado após ida:**
- Arestas usadas por F1: feromônio aumenta em 1
- Arestas usadas por F2: feromônio aumenta em 1
- Arestas usadas por F3: feromônio aumenta em 1

### 7.4 Primeira Iteração - Fase de Volta

As formigas retornam pelo mesmo caminho, depositando mais feromônio nas mesmas arestas.

### 7.5 Após a Primeira Iteração

Ao final da primeira iteração, as arestas têm seus valores de feromônio atualizados de acordo com o uso.

### 7.6 Evaporação

Após a primeira iteração, aplica-se a evaporação:
- Cada aresta perde 1 unidade de feromônio
- Arestas não utilizadas podem chegar a 0
- Arestas utilizadas ficam com feromônio positivo

---

## 8. Sistema de Roleta para Seleção de Caminhos

### 8.1 Conceito

A partir da segunda iteração, as formigas usam uma **roleta** para decidir qual caminho seguir. Este é o mesmo método usado em Algoritmos Genéticos.

### 8.2 Como Funciona

O feromônio total é convertido em setores de uma roleta:
- Cada setor representa uma possível rota
- O tamanho do setor é proporcional à quantidade de feromônio
- Um número aleatório é gerado para determinar qual setor é selecionado

### 8.3 Exemplo Prático - Primeira Iteração

**Após a primeira iteração com evaporação:**

Considere que o feromônio total está distribuído assim:
- ABE: 2 feromônio
- AD: 2 feromônio
- AC: 2 feromônio
- Total: 6 feromônio

**Roleta correspondente (intervalos de 1 a 33):**
- Intervalo 1-9: Rota ABE (proporção 3/6 = 9/33)
- Intervalo 10-23: Rota AD (proporção 2/6 = 14/33)
- Intervalo 24-33: Rota AC (proporção 1/6 = 10/33)

### 8.4 Exemplo de Decisão

**Para F1 (número gerado: 12):**
- 12 está no intervalo 10-23
- F1 segue a rota AD

**Para F2 (número gerado: 27):**
- 27 está no intervalo 24-33
- F2 segue a rota AC

**Para F3 (número gerado: 10):**
- 10 está no intervalo 10-23
- F3 segue a rota AD

**Para F4 (número gerado: 1):**
- 1 está no intervalo 1-9
- F4 segue a rota ABE

**Para F5 (número gerado: 23):**
- 23 está no intervalo 10-23
- F5 segue a rota AD

**Para F6 (número gerado: 19):**
- 19 está no intervalo 10-23
- F6 segue a rota AD

### 8.5 Evolução das Probabilidades

À medida que as iterações progridem:
- Rotas mais curtas acumulam mais feromônio
- Rotas mais longas perdem feromônio por evaporação
- A roleta favorece cada vez mais os caminhos ótimos

**Exemplo após algumas iterações:**
- Intervalo 1-12: Rota ABE (12% de chances)
- Intervalo 13-57: Rota AD (74% de chances)
- Intervalo 58-72: Rota AC (14% de chances)

O caminho AD torna-se cada vez mais provável!

---

## 9. Construção de Matriz de Probabilidades

### 9.1 Preparação dos Dados

Para aplicar o ACO a um problema real, precisamos de:

**Matriz de Distâncias:**
- Representa a distância de uma cidade para outra
- Cada entrada d(x,y) contém a distância entre as cidades x e y

**Matriz de Feromônios:**
- Representa o feromônio entre uma cidade e outra
- Inicializada com valores iguais para todas as arestas

### 9.2 Cálculo das Probabilidades

Dado:
- 5 cidades
- 5 formigas (uma por cidade)
- Parâmetros α (alfa) e β (beta) definidos

**Passo a passo:**

1. Para cada cidade, calcula-se o produto τ(x,y) × η(x,y) para cada aresta
2. Soma-se todos os produtos para obter o denominador
3. Calcula-se P(x,y) = (τ × η) / soma_total
4. A formiga escolhe probabilisticamente Based on the probabilities

### 9.3 Exemplo de Cálculo (Formiga em A)**

Para a formiga posicionada na cidade A, considerando as rotas A-B, A-C, A-D e A-E:

1. Calcular τ(A,B) × η(A,B) para cada opção
2. Calcular τ(A,i) × η(A,i) para todos os i
3. P(A,B) = τ(A,B) × η(A,B) / Σ τ(A,i) × η(A,i)

---

## 10. Atualização de Feromônios - Exemplo Numérico

### 10.1 Cenário Após Primeira Iteração

Suponhamos que ao final da primeira iteração, as 5 formigas fizeram os seguintes caminhos:

- Formiga 1: A → B → E → A
- Formiga 2: A → C → D → A
- Formiga 3: A → B → C → A
- Formiga 4: A → D → E → A
- Formiga 5: A → E → B → A

### 10.2 Cálculo da Atualização

Para cada aresta utilizada:
1. Identificar a qualidade da solução (comprimento do caminho)
2. Calcular a quantidade de feromônio a depositar
3. Aplicar a fórmula de atualização

**Fórmula típica:**
```
τ(x,y) = (1 - ρ) × τ(x,y) + Σ Δτ_k(x,y)
```

Onde:
- ρ: taxa de evaporação
- Δτ_k(x,y): feromônio depositado pela formiga k na aresta (x,y)

### 10.3 Exemplo Detalhado

Se uma formiga fez o caminho A-B-E com distância total 10:
- Deposita feromônio adicional nas arestas A-B e B-E
- A quantidade depositada pode ser proporcional a 1/distância

---

## 11. Parâmetros do ACO e Seus Pesos

### 11.1 Parâmetro Alfa (α) - Peso do Feromônio

**Definição:**
O α define o peso da memória coletiva. Ele determina o quanto a formiga será influenciada pelo rastro deixado pelas companheiras anteriores.

**Comportamentos:**

| Valor de α | Comportamento |
|------------|---------------|
| α ≈ 0 | O algoritmo ignora o aprendizado coletivo e foca apenas na proximidade (heurística local). Transforma-se quase em um algoritmo guloso (greedy) estocástico. |
| α Alto | As formigas tendem a seguir cegamente as rotas já estabelecidas. Isso leva a uma convergência prematura, podendo ficar preso em mínimos locais. |

### 11.2 Parâmetro Beta (β) - Peso da Visibilidade

**Definição:**
O β define o peso da informação local (geralmente 1/dij, onde dij é a distância). É o "olho" da formiga.

**Comportamentos:**

| Valor de β | Comportamento |
|------------|---------------|
| β ≈ 0 | A formiga ignora a distância e foca apenas no feromônio. A busca torna-se menos eficiente no início, pois não há "norte" geográfico antes dos primeiros rastros serem depositados. |
| β Alto | O algoritmo prioriza o caminho mais curto imediato. Se for muito alto, a formiga ignora o feromônio e o algoritmo perde sua característica de inteligência coletiva, agindo como uma busca de vizinhança simples. |

### 11.3 Taxa de Evaporação (ρ)

**Definição:**
A evaporação é o mecanismo de esquecimento do algoritmo. Ela é crucial para evitar que erros iniciais ou caminhos subótimos se tornem verdades absolutas.

**Efeitos:**
- ρ alto: Esquecimento rápido, mais exploração
- ρ baixo: Lentidão para abandonar maus caminhos, menos exploração

### 11.4 Parâmetro Q (Quantidade de Feromônio)

Define a quantidade de feromônio depositada por cada formiga. Pode ser:
- Constante (todas as formigas depositam o mesmo)
- Proporcional à qualidade da solução

### 11.5 Equilíbrio Ideal

Os valores ideais de α e β devem encontrar um equilíbrio entre:
- **Exploração**: Buscar novas soluções
- **Exploração local**: Refinar boas soluções encontradas

---

## 12. Aspectos de Implementação

### 12.1 Estruturas de Dados

**Matriz de Distâncias:**
```
d[i][j] = distância entre cidade i e cidade j
```

**Matriz de Feromônios:**
```
τ[i][j] = feromônio entre cidade i e cidade j
```

**Lista de Formigas:**
- Cada formiga mantém:
  - Cidade atual
  - Lista de cidades visitadas
  - Caminho construído
  - Distância total percorrida

### 12.2 Observações Importantes

- As formigas não existem de fato fisicamente
- O que existe são os **caminhos feitos por elas** (solução para o problema)
- O algoritmo trabalha com representações matemáticas desses caminhos

### 12.3 Critérios de Parada

O algoritmo pode parar quando:
- Atinge um número máximo de iterações
- Encontra uma solução com qualidade satisfatória
- Não há mais melhoria significativa entre iterações
- Tempo de execução excede limite

---

## 13. Comparação com Abordagens Determinísticas

### 13.1 Vantagens do ACO

- **Busca distribuída**: Múltiplas formigas exploram simultaneamente
- **Adaptabilidade**: Adapta-se a mudanças no ambiente
- **Robustez**: Não depende de uma única solução
- **Paralelismo**: Formigas podem trabalhar independentemente

### 13.2 Comparação

| Aspecto | Determinístico | ACO |
|---------|---------------|-----|
| Busca | Única solução | Múltiplas soluções |
| Flexibilidade | Fixa | Adaptativa |
| paralelismo | Limitado | Natural |
| Memória | Externa | Distribuída (feromônio) |

---

## 14. Referências Teóricas

### 14.1 Conceitos Fundamentais

1. **Comportamento de formigas reais**: Comunicação indireta via feromônio
2. **Otimização por enxame**: Inteligência coletiva emergente
3. **Metaheurística**: Algoritmo de nível superior para resolver problemas de otimização

### 14.2 Leitura Complementar Sugerida

Para aprofundamento, deve-se investigar:
- Como os autores definiram os parâmetros α, β, ρ?
- Quais valores para α, β e Q eles usam?
- Quantas cidades foram usadas no estudo?
- Comparação com outras estratégias metaheurísticas
- Viabilidade para diferentes tipos de problemas TSP
- Análise de resultados experimentais

---

## 15. Resumo do Fluxo Completo do ACO

```
┌─────────────────────────────────────────┐
│          INICIALIZAÇÃO                   │
│  - Definir parâmetros (α, β, ρ, Q)      │
│  - Inicializar matriz de feromônios      │
│  - Colocar formigas em cidades aleatórias│
└─────────────────┬───────────────────────┘
                  ▼
┌─────────────────────────────────────────┐
│     PARA CADA ITERAÇÃO                  │
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  PARA CADA FORMIGA                 ││
│  │   - Escolher próxima cidade (roleta)││
│  │   - Construir solução               ││
│  │   - Calcular distância total       ││
│  └─────────────────────────────────────┘│
│                  ▼                      │
│  ┌─────────────────────────────────────┐│
│  │  AVALIAR SOLUÇÕES                   ││
│  │   - Identificar melhor solução (s*) ││
│  │   - Armazenar se for a melhor       ││
│  └─────────────────────────────────────┘│
│                  ▼                      │
│  ┌─────────────────────────────────────┐│
│  │  ATUALIZAR FEROMÔNIOS               ││
│  │   - Evaporação em todas as arestas  ││
│  │   - Deposição nas arestas usadas    ││
│  └─────────────────────────────────────┘│
└─────────────────┬───────────────────────┘
                  │
                  ▼
        ┌─────────────────┐
        │ CRITÉRIO DE     │
        │ PARADA ATINGIDO?│
        └────────┬────────┘
                 │
        ┌────────┴────────┐
        │                 │
       NÃO               SIM
        │                 │
        ▼                 ▼
    ┌────────────────┐  ┌────────────────┐
    │ VOLTAR PARA    │  │ RETORNAR s*    │
    │ PRÓXIMA        │  │ (MELHOR SOLUÇÃO│
    │ ITERAÇÃO       │  │  ENCONTRADA)   │
    └────────────────┘  └────────────────┘
```

---

## 16. Conclusão

O algoritmo de Otimização por Colônia de Formigas (ACO) é uma poderosa técnica de inteligência computacional inspirada na natureza. Sua eficácia deriva de:

1. **Equilíbrio entre exploração e exploitação** através dos parâmetros α e β
2. **Memória coletiva distribuída** através do feromônio
3. **Esquecimento adaptativo** através da evaporação
4. **Paralelismo natural** através de múltiplas formigas

O ACO é especialmente eficaz para problemas de otimização combinatória como o TSP, onde a busca por múltiplas soluções simultâneas leva a resultados melhores do que buscas determinísticas tradicionais.