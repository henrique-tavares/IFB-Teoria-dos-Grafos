# <p style="text-align: center;">Relatório: <br> Biblioteca de Grafo + Estudos de Caso</p>

Instituto Federal de Brasília  
Disciplina: Teoria dos Grafos  
Professor: Raimundo Vasconcelos  
Aluno(a): Cinthia Mie Nagahama Ungefehr e Henrique Tavares Aguiar

---

1. A Biblioteca

   1. Método Facade -> Classe Graph
   2. Lista de Adjacência -> Classe \_GraphList
   3. Matriz de Adjacência -> Classe \_GraphMatrix

   EXTRA: Classe Edge

2. Estudo de Caso 1 - "collaboration_graph.txt"

   1. Gasto de Memória

      - Grafo lista: 91300kb (~89Mb) (1.49s)
      - Grafo matriz: 5025008kb (~4.8Gb) (23.47s)

   2. Comparação - Busca em largura

      - Grafo lista:

        - teste - bfs - médias:
          - 13681: 2.26e-04s
          - 21383: 9.71e-02s
          - 352: 7.90e-05s
          - 53446: 9.30e-02s
          - 67379: 5.76e-05s
        - insert_relation: O(1)
        - bfs: O(V + E)
        - dfs: O(V + E)
        - fcc obsoleto: (V^2) - possíveis piores casos: (1 componente; V componentes)
        - fcc otimizado:
          - caso: V componentes -> O(V)
          - caso: 1 componente (complexidade da bfs) -> O(V + E)

      - Grafo matriz:
        - teste - bfs - médias:
          - 13681: 7.886e-04s
          - 21383: 7.35e-01s
          - 352: 2.41e-04s
          - 53446: 5.99e-01s
          - 67379: 1.62e-04s
        - insert_relation: O(1)
        - bfs: O(V^2)
        - dfs: O(V^2)
        - fcc:
          - caso: V componentes -> O(V)
          - caso: 1 componente (complexidade da bfs) -> O(V^2)

   3. Componentes conexos: 14384

      - Grafo lista: 0.1s
      - Grafo matriz: 8.663s

      - Maior: 33533 vértices
      - Menor: 1 vértice

3. Estudo de Caso 2 - "as_graph.txt"

   1. Graus do Grafo

      - Maior grau possível: 32384
      - Maior grau no grafo: 2159
      - Menor grau no grafo: 1

        ![Imagem](https://cdn.discordapp.com/attachments/740548974343094332/914239959483969586/unknown.png)

   2. Componentes Conexos do Grafo

      - Número de componentes do grafo: 1
      - Maior componente conexo possui 32385 vértices
      - Menor componente conexo possui 32385 vértices

        OBS: Eles são o mesmo componente

   3. Busca em Largura

      - O maior nível encontrado durante a BFS a partir de 1 foi 6
      - O maior nível encontrado durante a BFS a partir de 728 foi 7
      - O maior nível encontrado durante a BFS a partir de 16379 foi 8
      - O maior nível encontrado durante a BFS a partir de 29382 foi 8

        Logo, a árvore de busca criada a partir de vértices diferentes tem profundidades diferentes.

   4. Diâmetro da Internet é 11
