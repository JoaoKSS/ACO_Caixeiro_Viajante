# Implementação e avaliação do algoritmo ACO para o Problema do Caixeiro Viajante

---

Este repositório apresenta a implementação do algoritmo Ant Colony Optimization (ACO) aplicado ao Problema do Caixeiro Viajante (TSP) em grafos totalmente conexos com 5, 10, 20 e 50 cidades.

O projeto tem como objetivo avaliar o comportamento do ACO utilizando diferentes tamanhos de instância e analisar a influência dos parâmetros do algoritmo na qualidade das soluções encontradas.

## ITEM A) Versão Básica

A versão básica utiliza:
- 5 cidades
- 30 iterações;
- alfa = 1;
- beta = 1;
- taxa de evaporação do feromônio = 0.03;
- feromônio inicial = 0.1;
- Q = 10;
- método de escolha da rota por torneio.

Os experimentos incluem análise das distâncias percorridas pelas formigas, médias das soluções por iteração, convergência do algoritmo e visualização gráfica da evolução das rotas encontradas.
