##Графы

**Предварительное замечание: Словарь понятий в теории графов не всегда постоянен. 
Некоторые авторы используют одинаковые слова для описания различных понятий. Иногда же разные слова объясняют один и тот же термин. Я надеюсь, что наши определения свободны от противоречивых толкований.**  

**Граф определяется как совокупность множества вершин (узлов) и рёбер (связей между вершинами).**  

![alt text](https://github.com/schastny/p99/raw/master/img/graph1.gif)  
Существует несколько способов представления графов в Прологе. 

Один из методов - представлять каждую вершину отдельно как одно целевое утверждение (факт). 
Таким образом граф, представленный выше может быть записан как следующий предикат:

    edge(h,g).
    edge(k,f).
    edge(f,b).    
    ...

Мы называем такую запись **edge-clause form**. 
Очевидно, что таким образом невозможно записать изолированные вершины.  

Другой метод - записывать весь граф как один объект. 
Исходя из определения графа как двух наборов (вершин и рёбер), 
мы можем использовать следующую запись на Пролог (показано для графа, приведённого на изображении):

    graph(
        [b, c, d, f, g, h, k], 
        [e(b, c), e(b, f), e(c, f), e(f, k), e(g, h)]
    )

Мы называем такую запись **graph-term form**. 
Отметьте, что списки хранятся в отсортированном виде, и на самом деле это наборы (Sets), без повторяющихся элементов. 
Каждое ребро появляется в списке ребёр только один раз. То есть, ребро из вершины x до вершины y представлено записью *e(x, y)*, 
а записи *e(y, x)* в списке нету.   
**Запись вида graph-term будет нашей формой представления графа по умолчанию**. 
В SWI-Prolog (реализация языка Пролог) есть предопределённые предикаты для работы с наборами.  

Третий вид представления - ассоциировать с каждой вершиной набор примыкающих вершин. 
Мы называем такую запись **adjacency-list form**. 
Запись для примера выше:

    [
        n(b,[c,f]), 
        n(c,[b,f]), 
        n(d,[]), 
        n(f,[b,c,k]), 
        ...
    ]

Приведённые методы представления являются валидными записями на Пролог и поэтому хорошо подходят для автоматической обработки,
но при этом их синтаксис не очень читабельный для пользователя. 
Набирать записи в таком формате для пользователя будет достаточно трудно и можно допустить ошибку. 
Мы можем определить более компактную и понятную для человека запись:   
Граф будет записываться как список атомов и терминов типа X-Y (то есть фукнтор '-' с арностью 2)
Атомы будут обозначать изолированные вершины, а термин X-Y - описывать рёбра. 
Если выясняется, что X - конец ребра, то он автоматически определяется, как вершина. 
Наш пример выше может быть описан следующим образом:  

    [b-c, f-c, g-h, d, f-b, k-f, h-g]

Мы будем называть такую запись **human-friendly form**. 
Как показано на примере, список не обязательно должен быть отсортирован и даже может содержать одно и то же ребро несколько раз. 
Отметьте также изолированную вершину d. 
(Вообще-то, изолированные вершины могут быть и не атомами в Прологе, а например составными терминами: d(3.75,blue) вместо простого d).

![alt text](https://github.com/schastny/p99/raw/master/img/graph2.gif)  
Если рёбра имеют направление, то из называют дугами. 
**Дуга** — это ориентированное ребро. Дуги записываются в виде упорядоченных пар. 
А сам граф — **ориентированный** или орграф (directed graph, digraph). 
Для записи ориентированных графов форма записи немного меняется. 
Например, граф, изображённый на картинке, записывается следующим образом:  

*Arc-clause form*

    arc(s,u).
    arc(u,r).
    ...
    
*Graph-term form*

    digraph([r,s,t,u,v],[a(s,r),a(s,u),a(u,r),a(u,s),a(v,u)])

*Adjacency-list form*

    [n(r,[]),n(s,[r,u]),n(t,[]),n(u,[r]),n(v,[u])]
(Отметьте, что в данной записи не будет информации, является ли этот граф простым или ориентированным)

*Human-friendly form*

    [s > r, t, u > r, s > u, u > s, v > u] 

![alt text](https://github.com/schastny/p99/raw/master/img/graph3.gif)  
И наконец, графы и орграфы могут иметь дополнительную информацию, добавленную к вершинам и рёбрам (дугам). 
Для вершин это не проблема, так как мы без труда можем заменить единичный символ на совтавной термин, например city('London',4711). 
Тогда как для рёбер нам придётся расширить нашу нотацию.
Графы с дополнительной информацией, прикреплённой к рёбрам, называются **помеченный граф** (labeled graph). 

*Arc-clause form*

    arc(m,q,7).
    arc(p,q,9).
    arc(p,m,5).
    
*Graph-term form*

    digraph([k,m,p,q],[a(m,p,7),a(p,m,5),a(p,q,9)])
    
*Adjacency-list form*

    [n(k,[]),n(m,[q/7]),n(p,[m/5,q/9]),n(q,[])]
(Отметьте, как информация по рёбрам была упакована с соответствующей вершиной в терм с функтором '/' и арностью 2)

*Human-friendly form*

    [p>q/9, m>q/7, k, p>m/5]

Нотация для помеченных графов так же может использоваться для так называемых мультиграфов ([multigraph](http://en.wikipedia.org/wiki/Multigraph)), 
в которых разрешено иметь более одного ребра (дуги) между двумя вершинами.  

**6.01 (\*\*\*) Преобразования**  
Напишите методы для преобразования между различными представлениями графов. 
Эта задача помечена тремя звёздами не ввиду своей сложности, 
а потому что здесь много работы по написанию методов для всех форм представления. 

**6.02 (\*\*) Путь от одной вершины к другой**  
Напишите метод path(G,A,B,P) для поиска ациклического пути P из вершины A в вершину B в графе G. 
Метод должен возвращать все возможные пути.

**6.03 (\*) Cycle from a given node**  
Напишите метод cycle(G,A,P) для поиса замкнутого пути (цикла), начинающегося в заданной вершине A в графе G. 
Метод должен возвращать все возможные варианты решения.

**6.04 (\*\*) Построить все каркасные (остовные) деревья**  
![alt text](https://github.com/schastny/p99/raw/master/img/p83.gif)
*Каркасное дерево* ([spanning tree](http://en.wikipedia.org/wiki/Spanning_tree)) состоит из некоторого подмножества рёбер графа, 
таких, что из любой вершины графа можно попасть в любую другую вершину, двигаясь по этим рёбрам, и в нём нет циклов.  

Напишите метод s_tree(Graph,Tree) для построения всех возможных каркасных деревьев для заданного графа.
Используя данный метод, определите, сколько каркасных деревьев существует у графа, приведённого слева на картинке.
Строчное представление к данному графу находится в файле [p6_04.dat](https://github.com/schastny/p99/raw/master/files/p6_04.dat). 
Когда у вас будет корректная реализация s_tree/2, используйте её для определения двух других полезных методов: 
is_tree(Graph) и is_connected(Graph). 
Оба метода не должны занять у вас много времени!

**6.05 (\*\*) Построить наименьшее каркасное дерево**  
![alt text](https://github.com/schastny/p99/raw/master/img/p84.gif)
Напишите метод ms_tree(Graph,Tree,Sum) для поиска наименьшего карскасного дерево для заданного помеченного графа. 
Подсказка: Используйте алгоритм Прима ([Prim's algorithm](http://en.wikipedia.org/wiki/Prim%27s_algorithm)). 
Вам нужно будет всего лишь немного модифицировать решение к задаче 6.04. 
Строчное представление к аднному графу находится в файле [p6_05.dat](https://github.com/schastny/p99/raw/master/files/p6_05.dat).

**6.06 (\*\*) Изоморфизм графов**  
Два графа G1(N1,E1) и G2(N2,E2) являются изоморфными если существует биекция
 
    f: N1 -> N2,

такая, что для любых вершин X,Y из N1 являются смежными только если f(X) and f(Y) также являются смежными.
Напишите предикат, который определяет, являются ли два графа изоморфными.
Подсказка: Используйте open-ended list для описания функции f. 

**6.07 (\*\*) Степерь вершины графа раскраска графа**  
a) Напишите метод degree(Graph,Node,Deg) для определения степени заданной вершины. 
b) Напишите предикат, возвращающий список всех вершин графа в отсортированном по степени вершины виде. 
c) Используйте алгоритм жадной расскраски (так же иногда называемый Welch-Powell's algorithm) 
для раскраски вершин графа такм образом, чтобы смежные вершины всегда имели разные цвета.  

**6.08 (\*\*) Обход графа в глубину**  
Напишите метод, который выводит последовательность обхода вершин графа в глубину. 
Стартовая позиция должна быть указана, а в качестве результата должен быть список вершин, 
упорядоченных по мере доступности от начальной позиции.

**6.09 (\*\*) Компоненты связности**  
Напишите метод, разбивающий граф на компоненты связности.  

**6.10 (\*\*) Биграфы (двудольные графы)**  
Напишите предикат, определяющий, является ли заданный граф двудольным ([bipartite graphs](en.wikipedia.org/wiki/Bipartite_graph)).
     
**6.11 (\*\*\*) Сгенерировать регулярный граф степени K, имеиющий N вершин**  
В регулярном графе степени K (K-regular simple graph) все вершины имеют степень K, то есть количество соседей у каждой вершины постоянное - K.
Сколько существует регулярных графов третьей степени, имеющих 6 вершин?   

Смотри также таблицу результатов в [p6_11.txt](https://github.com/schastny/p99/raw/master/files/p6_11.txt). 

[Предыдущая глава](multiwaytrees.md) | [Оглавление](README.md) | [Следующая глава](misc.md)