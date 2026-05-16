# Implementação e avaliação do algoritmo ACO para o problema do Caixeiro Viajante

## Introdução

O problema estudado neste trabalho é o Problema do Caixeiro Viajante, conhecido como TSP. Nesse problema, deseja-se encontrar a menor rota possível para visitar todas as cidades exatamente uma vez e retornar para a cidade inicial. Esse tipo de problema é importante porque aparece em situações como planejamento de rotas, logística, transporte e otimização de caminhos.

Para representar o problema, foi usado um grafo totalmente conexo. Isso significa que cada cidade possui ligação direta com todas as outras cidades. Cada ligação entre duas cidades possui um peso, que representa a distância entre elas. No caso da versão básica, foram usadas 5 cidades, chamadas de A, B, C, D e E, com uma matriz de distâncias definida no notebook.

O método escolhido para resolver o problema foi o algoritmo de Otimização por Colônia de Formigas, ou ACO. Esse algoritmo é inspirado no comportamento de formigas reais, que depositam feromônio nos caminhos percorridos. Com o tempo, caminhos melhores tendem a receber mais feromônio e passam a ser escolhidos com maior frequência. No contexto do TSP, cada formiga constrói uma rota completa, e as melhores rotas reforçam as arestas utilizadas.

## Materiais e Métodos

A implementação foi feita em Python, no notebook `aco_tsp.ipynb`. Foram utilizadas as bibliotecas `numpy` e `matplotlib` para os cálculos numéricos e para a geração dos gráficos. O algoritmo foi aplicado inicialmente ao problema com 5 cidades, seguindo a configuração exigida no item A: 30 iterações, alfa igual a 1, beta igual a 1, taxa de evaporação igual a 0.03, feromônio inicial igual a 0.1, Q igual a 10 e método de escolha por torneio.

No ACO, cada aresta possui um valor de feromônio, representado por `tau(i,j)`, e uma visibilidade, representada por `eta(i,j)`. A visibilidade foi calculada como o inverso da distância entre as cidades:

```text
eta(i,j) = 1 / distancia(i,j)
```

A atratividade de uma cidade candidata foi calculada por:

```text
atratividade(i,j) = tau(i,j)^alfa * eta(i,j)^beta
```

Na escolha por torneio, algumas cidades ainda não visitadas são selecionadas como candidatas, e a cidade com maior atratividade vence o torneio. Depois que todas as formigas constroem suas rotas, ocorre a evaporação do feromônio e, em seguida, cada formiga deposita feromônio nas arestas de sua rota. A quantidade depositada é proporcional à qualidade da solução, usando `Q / distancia_da_rota`.

O pseudocódigo geral do algoritmo implementado foi:

```text
Inicializar matriz de feromônios com o valor inicial
Para cada iteração:
    Para cada formiga:
        Colocar a formiga em uma cidade inicial
        Enquanto ainda existirem cidades não visitadas:
            Calcular a atratividade das cidades candidatas
            Escolher a próxima cidade pelo torneio
            Adicionar a cidade na rota
        Fechar a rota retornando para a cidade inicial
        Calcular a distância total da rota
    Atualizar a melhor rota encontrada
    Evaporar o feromônio das arestas
    Depositar feromônio nas arestas percorridas pelas formigas
Retornar a melhor solução encontrada
```

Para os itens B e C, foram executadas 10 repetições para cada configuração de parâmetro. A comparação foi feita usando média, mediana, moda, desvio padrão, mínimo, máximo e boxplot. Como o algoritmo possui aleatoriedade, esse número de execuções ajuda a observar melhor o comportamento médio de cada configuração.

No teste final, foram criados grafos completos com 10, 20 e 50 cidades. O algoritmo foi executado com os parâmetros escolhidos nos itens B e C. Nessa etapa, o critério de parada foi a estagnação: o algoritmo parou quando a melhor solução encontrada não melhorou durante 50 iterações consecutivas, com limite máximo de 500 iterações. Cada tamanho de problema foi executado 10 vezes, e também foi medido o tempo de execução.

## Resultados e Discussão

Na versão básica com 5 cidades, o algoritmo encontrou uma melhor rota com distância total igual a 29. A rota observada nesta execução foi:

```text
A -> E -> D -> C -> B -> A
```

Antes da execução do ACO, o problema é representado pelo grafo completo da Figura 1. Nesse grafo, todas as cidades possuem ligação com todas as outras, e os números nas arestas indicam as distâncias definidas para a instância de 5 cidades.

![Figura 1 - Grafo inicial completo do item A](imagens/item_a_grafo_inicial.png)

Essa rota usa as arestas A-E, E-D, D-C, C-B e B-A, cujos pesos são 8, 4, 2, 3 e 12. Somando esses valores, obtém-se a distância total 29. A Figura 2 mostra a rota encontrada dentro do grafo de 5 cidades. As arestas em verde representam a melhor rota selecionada pelo ACO, enquanto as demais ligações aparecem apenas como referência, pois o grafo original é totalmente conexo.

![Figura 2 - Melhor rota encontrada no item A](imagens/item_a_rota.png)

O gráfico da Figura 3 mostra a evolução da média das soluções ao longo das 30 iterações. No início, a média das rotas das formigas ainda é maior, pois as escolhas possuem mais aleatoriedade e o feromônio ainda não acumulou informação suficiente. Depois das primeiras iterações, as rotas de menor custo passam a receber mais feromônio. Com isso, as formigas tendem a repetir trechos melhores, e a média das soluções cai até estabilizar em 29.

Esse comportamento é importante porque confirma a ideia principal do ACO: as soluções boas reforçam as arestas usadas, enquanto a evaporação reduz a influência de caminhos menos eficientes. Assim, a estabilização da média próxima da melhor distância indica que o método está funcionando corretamente para a instância básica.

![Figura 3 - Evolução das soluções no item A](imagens/item_a_evolucao.png)

No item B, foi analisada a influência dos parâmetros alfa e beta. O alfa controla a influência do feromônio, enquanto o beta controla a influência da visibilidade, ou seja, da distância entre as cidades. Foram testados os pares `(0.6, 0.2)` e `(0.2, 0.6)`.

| Parâmetro | Média | Mediana | Moda | Desvio padrão | Mínimo | Máximo |
|---|---:|---:|---:|---:|---:|---:|
| α=0.6, β=0.2 | 29.00 | 29.00 | 29.00 | 0.00 | 29.00 | 29.00 |
| α=0.2, β=0.6 | 29.00 | 29.00 | 29.00 | 0.00 | 29.00 | 29.00 |

Como a instância de 5 cidades é pequena, os dois pares encontraram a mesma melhor solução nas 10 execuções. Isso significa que, para esse grafo específico, tanto uma configuração com maior peso para o feromônio quanto uma configuração com maior peso para a distância foram suficientes para encontrar a rota ótima observada. A média, a mediana e a moda ficaram iguais a 29, e o desvio padrão igual a 0, indicando ausência de variação nos melhores resultados finais.

Mesmo com esse empate estatístico, foi escolhido o par `alfa = 0.6` e `beta = 0.2` para continuar os testes. Essa escolha mantém uma influência um pouco maior do feromônio, o que favorece o aprendizado coletivo das formigas. Como os dois pares chegaram ao mesmo resultado final, a escolha não prejudica a continuidade do experimento. A Figura 4 mostra a comparação visual entre os dois pares de parâmetros.

![Figura 4 - Comparação entre alfa e beta no item B](imagens/item_b_alfa_beta.png)

No item C, foi testada a influência da taxa de evaporação do feromônio. Foram avaliados os valores 0.01, 0.05, 0.1 e 0.2, mantendo `alfa = 0.6` e `beta = 0.2`. A melhor solução final foi a mesma para todas as taxas, mas a média das distâncias ao longo das iterações permitiu observar diferenças no comportamento do algoritmo.

| Evaporação | Média | Média das distâncias | Mediana | Moda | Desvio padrão | Mínimo | Máximo |
|---|---:|---:|---:|---:|---:|---:|---:|
| ρ=0.01 | 29.00 | 30.06 | 29.00 | 29.00 | 0.00 | 29.00 | 29.00 |
| ρ=0.05 | 29.00 | 29.33 | 29.00 | 29.00 | 0.00 | 29.00 | 29.00 |
| ρ=0.1 | 29.00 | 30.22 | 29.00 | 29.00 | 0.00 | 29.00 | 29.00 |
| ρ=0.2 | 29.00 | 30.04 | 29.00 | 29.00 | 0.00 | 29.00 | 29.00 |

O melhor valor escolhido para a taxa de evaporação foi `0.05`, pois apresentou a menor média das distâncias ao longo das execuções. Embora todas as taxas tenham encontrado a melhor solução final de custo 29, a coluna "Média das distâncias" mostra como foi o comportamento geral das formigas durante o processo. Nesse ponto, a taxa 0.05 se destacou porque manteve a média mais baixa ao longo das iterações.

Esse resultado pode ser interpretado como um equilíbrio entre memória e exploração. Com evaporação muito baixa, como 0.01, o feromônio permanece por mais tempo e pode preservar caminhos encontrados no início, mesmo que nem todos sejam os melhores. Com evaporação mais alta, como 0.1 ou 0.2, o algoritmo esquece os rastros mais rapidamente, o que pode aumentar a exploração, mas também pode dificultar a consolidação das melhores trilhas. A taxa 0.05 apresentou melhor equilíbrio nesta instância. A Figura 5 apresenta a comparação entre as taxas.

![Figura 5 - Comparação entre taxas de evaporação no item C](imagens/item_c_evaporacao.png)

Depois da definição dos parâmetros, foi realizado o teste final com 10, 20 e 50 cidades. Foram usadas 10 execuções para cada tamanho de problema. O algoritmo executou até a melhor solução ficar sem melhoria por 50 iterações consecutivas, respeitando o limite máximo de 500 iterações.

| Problema | Média | Mediana | Moda | Desvio padrão | Mínimo | Máximo | Tempo médio (s) | Iterações médias |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| 10 cidades | 281.00 | 281.00 | 281.00 | 0.00 | 281.00 | 281.00 | 0.0175 | 59.10 |
| 20 cidades | 525.70 | 521.00 | 511.00 | 18.07 | 507.00 | 564.00 | 0.1125 | 92.40 |
| 50 cidades | 1380.30 | 1379.00 | 1348.00 | 30.10 | 1348.00 | 1421.00 | 0.8027 | 99.60 |

Os resultados mostram que, conforme o número de cidades aumenta, a distância média das soluções também aumenta. Isso é esperado, pois uma rota com mais cidades possui mais trechos e mais possibilidades de combinação. Além disso, o espaço de busca cresce rapidamente: com 5 cidades existem poucas combinações possíveis, mas com 20 ou 50 cidades o número de rotas possíveis se torna muito maior. Por isso, o problema fica mais difícil.

O tempo médio de execução também aumentou com o tamanho do problema. Para 10 cidades, o algoritmo foi bastante rápido e sempre encontrou a mesma solução nas 10 execuções, o que gerou desvio padrão igual a 0. Para 20 cidades, a média foi 525.70 e o desvio padrão foi 18.07, mostrando que as execuções passaram a variar mais. Para 50 cidades, a média foi 1380.30 e o desvio padrão foi 30.10, indicando uma dispersão ainda maior nos resultados.

Essa variação é normal em metaheurísticas como o ACO, pois existe aleatoriedade na construção das rotas. Em problemas maiores, a chance de cada execução explorar regiões diferentes do espaço de busca aumenta. O boxplot da Figura 6 deixa essa diferença mais visível: a instância de 10 cidades aparece concentrada em um único valor, enquanto as instâncias de 20 e 50 cidades apresentam maior espalhamento.

![Figura 6 - Boxplot do teste final](imagens/teste_final_boxplot.png)

De modo geral, o ACO apresentou bom comportamento para o problema proposto. Na instância básica, a queda das médias mostrou que o feromônio guiou as formigas para rotas melhores. Nos testes de parâmetros, a diferença entre os valores foi pequena para 5 cidades, porque a instância é simples, mas a análise estatística permitiu escolher uma configuração final. No teste com 10, 20 e 50 cidades, o aumento do tamanho do problema tornou a busca mais difícil, aumentando tanto a dispersão dos resultados quanto o tempo médio de execução.

## Conclusão

O algoritmo ACO implementado apresentou resultados satisfatórios para o problema do Caixeiro Viajante nas instâncias analisadas. Na versão básica com 5 cidades, o método conseguiu encontrar uma rota de custo 29 e a média das soluções diminuiu ao longo das iterações, mostrando que o mecanismo de feromônio estava funcionando de forma adequada. Esse comportamento confirma a ideia central do ACO: as melhores rotas recebem reforço e passam a influenciar as escolhas seguintes das formigas.

Nos testes de parâmetros, foi possível observar que a instância de 5 cidades é simples e, por isso, diferentes configurações chegaram à mesma melhor distância final. Mesmo assim, a análise com 10 execuções, média, mediana, moda, desvio padrão e boxplot foi importante para comparar o comportamento do algoritmo de maneira mais organizada. O par escolhido para alfa e beta foi `alfa = 0.6` e `beta = 0.2`, mantendo maior peso para o feromônio. Para a evaporação, a taxa escolhida foi `0.05`, pois apresentou melhor média das distâncias durante o processo de busca.

Com esses parâmetros definidos, o algoritmo foi aplicado aos problemas com 10, 20 e 50 cidades. Os resultados mostraram que, conforme o número de cidades aumenta, a dificuldade do problema também cresce. Isso ficou evidente pelo aumento da distância média, pelo maior desvio padrão e pelo aumento do tempo médio de execução. Para 10 cidades, os resultados foram bastante estáveis. Já para 20 e 50 cidades, houve maior variação entre as execuções, o que é esperado em uma metaheurística estocástica aplicada a um espaço de busca maior.

Portanto, conclui-se que o ACO é uma alternativa viável para buscar boas soluções no problema do Caixeiro Viajante, principalmente por combinar exploração de novas rotas com reforço das melhores soluções encontradas. Ao mesmo tempo, os experimentos mostram que a escolha dos parâmetros influencia o comportamento do algoritmo e que problemas maiores exigem mais cuidado na avaliação, pois tendem a apresentar maior variação nos resultados. Como melhoria futura, poderiam ser testadas mais combinações de parâmetros, maior número de execuções e outras estratégias de escolha de rota, comparando o desempenho do ACO com outros métodos de otimização.
