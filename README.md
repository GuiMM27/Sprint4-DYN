# Desafio de Programação Dinâmica: Otimização do Controle de Estoque de Insumos

**Contexto do Problema:**
O desafio consiste em resolver a baixa visibilidade no apontamento do consumo de insumos (reagentes e descartáveis) em unidades de diagnóstico. A dificuldade em monitorar e registrar o consumo com precisão dificulta o controle de estoque e a previsão de reposição, gerando discrepâncias, falta de materiais essenciais e desperdícios.

**Solução Proposta:**
Modelagem do problema de reposição de insumos como um processo de decisão sequencial usando Programação Dinâmica (PD), buscando a política ótima de pedidos que minimize o custo total de operação (compra, armazenamento e penalidade por falta) ao longo de um horizonte de tempo.

---

## 1. Formulação do Problema 

A Programação Dinâmica é aplicada para determinar a sequência ótima de pedidos ($D_t$) que minimize o custo total acumulado ao longo de $T$ dias.

### 1.1. Definição de Estados, Decisões, Função de Transição e Função Objetivo 

| Componente | Variável / Notação | Definição no Contexto |
| :--- | :--- | :--- |
| **Estado** | $S_t$ | O nível de estoque do insumo no início do dia $t$. |
| **Decisão** | $D_t$ | A quantidade de insumos a ser pedida/reposta no dia $t$. |
| **Função de Transição** | $S_{t+1}$ | Define o novo estado de estoque para o dia $t+1$: $$S_{t+1} = \max(0, S_t + D_t - C_t)$$ (Onde $C_t$ é o consumo do dia $t$). |
| **Função Objetivo (Recorrência)** | $f(S_t)$ | O custo mínimo total acumulado de $t$ até $T$, dado o estoque $S_t$. $$f(S_t) = \min_{D_t} \left\{ \text{Custo Diário}(S_t, D_t, C_t) + f(S_{t+1}) \right\}$$ |

**Custo Diário ($\text{Custo}(S_t, D_t, C_t)$):**
$$\text{Custo Total Diário} = \text{Custo de Pedido} + \text{Custo de Armazenamento} + \text{Custo de Falta}$$

---

## 2. Implementação, Resultados e Relatório 

### 2.1. Explicação da Estrutura e Algoritmo 

O uso da Programação Dinâmica é justificado pela necessidade de otimizar uma sequência de decisões interdependentes (a decisão de pedido de hoje afeta o estoque de amanhã).

| Estrutura/Algoritmo | Uso no Contexto do Controle de Estoque |
| :--- | :--- |
| **Programação Dinâmica (Algoritmo)** | Utiliza o **Princípio da Otimilidade** para garantir que a política de pedidos minimiza o custo total. Isso fornece uma solução preditiva, substituindo a incerteza do registro manual impreciso. |
| **Memorização (Estrutura - Top-Down)** | Usada na versão recursiva. Armazena o custo mínimo de cada subproblema `(Dia, Estoque)` em um dicionário. Isso impede o recálculo de estados repetidos, tornando o algoritmo eficiente (complexidade polinomial). |
| **Tabela DP (Matriz NumPy - Bottom-Up)** | Usada na versão iterativa. A matriz preenchida de trás para frente armazena o custo mínimo para todos os estados em todos os tempos. A tabela de política gerada é o **produto operacional** que o sistema de diagnóstico usa: se o estoque é $S$ no Dia $t$, a quantidade a pedir é $D_t$ (lida diretamente da tabela). |

### 2.2. Verificação de Equivalência e Política Ótima 

Ambas as versões de implementação convergiram para o mesmo resultado, validando a correção do modelo:

| Versão | Custo Mínimo Total |
| :--- | :--- |
| Recursiva com Memorização | R$ 556.00 |
| Iterativa (Bottom-Up) | R$ 556.00 |

**Política Ótima (Caminho da Solução):**
A política demonstra que, devido ao alto custo de penalidade por falta, o modelo otimiza os pedidos para cobrir o consumo projetado, permitindo o esgotamento do estoque em dias de alto consumo seguido de reposição estratégica (Decisão) para cobrir a demanda futura.

| Dia | Estoque Inicial | Consumo | Decisão (Pedido) | Estoque Final |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 5 | 5 | 0 | 0 |
| 2 | 0 | 8 | 11 | 3 |
| 3 | 3 | 3 | 0 | 0 |
| 4 | 0 | 10 | 14 | 4 |
| 5 | 4 | 4 | 0 | 0 |
| 6 | 0 | 7 | 13 | 6 |
| 7 | 6 | 6 | 0 | 0 |
