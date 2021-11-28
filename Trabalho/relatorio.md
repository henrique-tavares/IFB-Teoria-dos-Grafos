# <p style="text-align: center;">Relatório: <br> Biblioteca de Grafo + Estudos de Caso</p>

Instituto Federal de Brasília  
Disciplina: Teoria dos Grafos  
Professor: Raimundo Vasconcelos  
Aluno(a): Cinthia Mie Nagahama Ungefehr e Henrique Tavares Aguiar

---

1. **A Biblioteca**

    O presente trabalho consiste de uma biblioteca para manipular grafos não dirigidos criada na linguagem de programação Python e testada em ambiente virtual linux (WSL2) Ubuntu 20.04.

    A biblioteca é composta por uma classe concreta "Edge" e uma classe fachada "Graph".

    - **Edge**

        A classe Edge é bastante simples e possui apenas dois atributos: src e dest. Ela foi criada para facilitar a inserção de arestas no grafo ao mesmo tempo que prepara a biblioteca para ser escalada para grafos direcionados.

    - **Graph**

        A classe Graph foi criada tendo como base o padrão de projeto estrutural Facade (ou Fachada) e tem como objetivo fornecer uma interface simples para o usuário fornecendo apenas as funcionalidades necessárias para criar e manipular grafos, abstraíndo as implementações feitas com Lista de Adjacência e Matriz de Adjacência.

        Uma instância da classe Graph é criada passando-se o tipo de representação a ser utilizada e a quantidade de vértices presentes no grafo. Através dessa instância é possível acessar os métodos disponíveis:

        - **insert_relation(self, edge: Edge) -> None:** Insere uma nova aresta no grafo.
        - **get_graph_degrees(self) -> Dict[str, int]:** Retorna o grau de cada vértice do grafo.
        - **out_graph(self, out_path: str) -> None:** Gera um arquivo texto com o número de vértices, o número de arestas e o grau de cada vértice.
        - **breadth_first_search(self, origin: str, out_path: Optional[str] = None) -> None:** Faz uma busca em largura a partir de um vértice *origin* e gera um arquivo com a árvore de busca gerada, informando, para cada vértice, seu pai e nível.
        - **depth_first_search(self, origin: str, out_path: Optional[str] = None) -> None:** Faz uma busca em profundidade a partir de um vértice *origin* e gera um arquivo com a árvore de busca gerada, informando, para cada vértice, seu pai e nível.
        - **find_connected_components(self) -> List[Set[str]]:** Retorna uma lista de componentes conexas; cada componente conexa é composta pelos vértices do componente. Esse formato torna fácil calcular a quantidade de componentes conexas presentes no grafo e o tamanho de cada uma, mesmo fornecendo apenas os vértices de cada componente.

        Para cada uma das representações foi criada uma classe própria que implementa os métodos acima.

        - Lista de Adjacência:

            A classe _GraphList é responsável por criar e manipular a representação por lista de adjacência, sendo esta um dicionário no qual a chave é o vértice de "origem" e o valor é um *set* (conjunto de valores não ordenados distintos) com os vértices ligados ao vértice chave.

            Na _GraphList também estão as implementações para os métodos pedidos pela classe Graph.

            - **insert_relation(self, edge: Edge) -> None:** Como a biblioteca foi feita para grafos não direcionados, o vértice *dest* é adicionado ao *set* (valor) correpondente ao vértice *src* (chave) no dicionário e vice-versa.
            - **get_graph_degrees(self) -> Dict[str, int]:** É feito um *dict comprehension* que retorna um dicionário que tem como chave o vértice e como valor a quantidade de elementos no *set* de adjacência.
            - **out_graph(self, out_path: str) -> None:** Gera um arquivo texto com o número de vértices, o número de arestas e o grau de cada vértice.
            - **breadth_first_search(self, origin: str) -> Optional[Dict[str, Tuple[str, int]]]:** Inicialmente são criadas três estruturas:

                - *vertices_queue*: uma fila (estrutura *deque* da biblioteca *collections* do Python) que guardará tuplas compostas por: vértice, seu pai e seu nível;
                - *visited_vertices*: um dicionário cujas chaves são os vértices e os valores são uma tupla composta pelo pai e pelo nível do vértice chave;
                - *to_be_visited_vertices*: um *set* com os vértices a serem visitados, ou seja, já estão na fila (*vertices_queue*).

                Depois disso, é feita a busca em largura utilizando para checagem o *visited_vertices* e o *to_be_visited_vertices*, que, por serem um dicionário e um *set* respectivamente, são muito mais velozes do que se a checagem fosse feita na fila, aumentando o desempenho por um custo consideravelmente pequeno de memória extra.

                No fim, é retornado o dicionário *visited_vertices*.

            - **depth_first_search(self, origin: str) -> Optional[Dict[str, Tuple[str, int]]]:** Inicialmente são criadas três estruturas:

                - *vertices_stack*: uma pilha (estrutura *deque* da biblioteca *collections* do Python) que guardará tuplas compostas por: vértice, seu pai e seu nível;
                - *visited_vertices*: um dicionário cujas chaves são os vértices e os valores são uma tupla composta pelo pai e pelo nível do vértice chave;
                - *to_be_visited_vertices*: um *set* com os vértices a serem visitados, ou seja, já estão na pilha (*vertices_stack*).

                Depois disso, é feita a busca em profundidade utilizando para checagem o *visited_vertices* e o *to_be_visited_vertices*, que, por serem um dicionário e um *set*, são muito mais velozes do que se a checagem fosse feita na pilha, aumentando o desempenho por um custo consideravelmente pequeno de memória extra.

                No fim, é retornado o dicionário *visited_vertices*.

            - **find_connected_components(self) -> List[Set[str]]:** Inicialmente são criadas duas estruturas:

                - *connected_components*: uma lista de *sets*, onde cada *set* representa uma componente conexa e é composto pelos vértices da componente; e
                - *visited_vertices*: um *set* com os vértices que já foram visitados.

                Depois disso, para cada vértice do grafo que não estiver em visited_vertices é feita uma busca em profundidade (*depth_first_search*), com a adição da checagem feita com o *visited_vertices*, para evitar redundância, e otimizar o algoritimo. Depois, todos os vértices na árvore de busca gerada são adicionados à *connected_components* como um componente.

                No fim, é retornada a *connected_components*.

        - Matriz de Adjacência:

            A classe _GraphMatrix é responsável por criar e manipular a representação por matriz de adjacência. Nessa classe são armazenados um dicionário cujas chaves é o vértice e os valores são o índice do vértice chave na matriz de adjacência e uma matriz de booleanos de tamanho VxV, com V sendo o número de vértices do grafo, onde 1 representa a existência de aresta e 0 a não-existência de aresta.

            Na _GraphMatrix também estão as implementações para os métodos pedidos pela classe Graph.

            - **insert_relation(self, edge: Edge) -> None:** Como a biblioteca foi feita para grafos não direcionados, os valores cujas coordenadas são os vértices são trocados para 1.
            - **get_graph_degrees(self) -> Dict[str, int]:** É feito um *dict comprehension* que retorna um dicionário que tem como chave o vértice e como valor a quantidade de elementos não nulos na linha correspondente ao vértice chave.
            - **out_graph(self, out_path: str) -> None:** Gera um arquivo texto com o número de vértices, o número de arestas e o grau de cada vértice.
            - **breadth_first_search(self, origin: str) -> Optional[Dict[str, Tuple[str, int]]]:** Inicialmente são criadas três estruturas:

            - *vertices_queue*: uma fila (estrutura *deque* da biblioteca *collections* do Python) que guardará tuplas compostas por: vértice, seu pai e seu nível;
            - *visited_vertices*: um dicionário cujas chaves são os vértices e os valores são uma tupla composta pelo pai e pelo nível do vértice chave;
            - *to_be_visited_vertices*: um *set* com os vértices a serem visitados, ou seja, já estão na fila (*vertices_queue*).

            Depois disso, é feita a busca em largura utilizando para checagem o *visited_vertices* e o *to_be_visited_vertices*, que, por serem um dicionário e um *set*, são muito mais velozes do que se a checagem fosse feita na fila, aumentando o desempenho por um custo consideravelmente pequeno de memória extra.

            No fim, é retornado o dicionário visited_vertices.

            - **depth_first_search(self, origin: str) -> Optional[Dict[str, Tuple[str, int]]]:** Inicialmente são criadas três estruturas:

                - *vertices_stack*: uma pilha (estrutura *deque* da biblioteca *collections* do Python) que guardará uma tuplas compostas por: vértice, seu pai e seu nível;
                - *visited_vertices*: um dicionário cujas chaves são os vértices e os valores são uma tupla composta pelo pai e pelo nível do vértice chave;
                - *to_be_visited_vertices*: um *set* com os vértices a serem visitados, ou seja, já estão na pilha (*vertices_stack*).

                Depois disso, é feita a busca em profundidade utilizando para checagem o *visited_vertices* e o *to_be_visited_vertices*, que, por serem um dicionário e um *set*, são muito mais velozes do que se a checagem fosse feita na lista de prioridade, aumentando o desempenho por um custo consideravelmente pequeno de memória extra.

                No fim, é retornado o dicionário *visited_vertices*.

            - **find_connected_components(self) -> List[Set[str]]:** Inicialmente são criadas duas estruturas:

                - *connected_components*: uma lista de *sets*, onde cada *set* representa uma componente conexa e é composto pelos vértices da componente; e
                - *visited_vertices*: um *set* com os vértices que já foram visitados.

                Depois disso, para cada vértice do grafo que não estiver em visited_vertices é feita uma busca em largura (*breadth_first_search*), com a adição da checagem feita com o *visited_vertices*, para evitar redundância, e otimizar o algoritimo. Depois, todos os vértices na árvore de busca gerada são adicionados à *connected_components* como um componente.

                No fim, é retornada a *connected_components*.

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

      - Grafo lista: 0.140s
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
