##P-99: Ninety-Nine Prolog Problems (P-99: 99 задач на Прологе)  
Original: https://sites.google.com/site/prologsite/prolog-problems  
Автор: Werner Hett    

Данный задачник поможет читателю попрактиковаться в логическом программировании.  

Ваша задача - найти наиболее элегантное решение предложенной проблемы. Эффективность важна, но логическая стройность является приоритетом. Некоторые из простых задач могут быть решены с использованием встроенных в язык библиотек. Хотя полезней будет, если вы попытаетесь найти своё собственное решение.  

Каждый метод, который вы напишете, должен быть снабжён комментарием. В комментариях не следует слепо описывать какие операторы или конструкции вы использовали. Вместо этого попытайтесь описать логику, алгоритм, который вы реализовали. Также укажите, какие аргументы и каких типов вы используете.  

Задачи имеют различный уровень сложности. Помеченные одной звёздочкой (\*) - лёгкие. 
Если вы раньше сталкивались с подобными задачами, то вы без проблем должны решать задачу с одной звёздочкой за несколько (скажем 15) минут. 
Задачи, помеченные двумя звёздочками (\*\*) - среднего уровня сложности. 
Если вы опытный программист на Prolog, то решение задачи с двумя звёздочками не должно у вас занять больше 30-90 минут. 
Задачи, помеченные тремя звёздочками (\*\*\*) являются сложными. 
Вам может понадобиться несколько часов и больше для того, чтобы найти хорошее решение.  

##Задачи
###Списки

Список может быть либо пустым, либо состоять из головного элемента (head) и хвоста (tail), который в свою очередь является списком. В прологе пустой список помечается [], а непустой - [H|T], где H - головной элемент, T - хвост.

**1.01 (\*) Найти последний элемент списка.**    
Пример:  

    ?- my_last(X,[a,b,c,d]).    
    X = d  
    
**1.02 (\*) Найти предпоследний элемент списка.**  

**1.03 (\*) Найти К-тый элемент списка.**  
Первый элемент списка имеет порядковый номер - 1.  
Пример:  

    ?- element_at(X,[a,b,c,d,e],3).
    X = c

**1.04 (\*) Найти количество элементов в списке.**  

**1.05 (\*) Перевернуть список.**

**1.06 (\*) Определить, является ли список палиндромом.**  
Палиндром должен одинаково читаться в обоих направлениях, например [x,a,m,a,x].

**1.07 (\*\*) Сделать плоской структуру из вложенных списков.**  
Преобразовать список, содержащий списки в качестве элементов, в плоский список, заменяя каждый список его элементами (рекурсисвно).  
Пример:  

    ?- my_flatten([a, [b, [c, d], e]], X).
    X = [a, b, c, d, e]

    Подсказка: Используйте готовые конструкции is_list/1 и append/3

**1.08 (\*\*) Удалить идущие подряд дубликаты элементов списка.**  
Если список содержит повторяемые элементы, они должны быть заменены на одиночную копию повторяемого элемента. Порядок элементов не должен меняться.   
Пример:

    ?- compress([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [a,b,c,a,d,e]

**1.09 (\*\*) Разложить дубликаты элементов по подспискам.**  
Если список содержит повторяемые элементы, они должны быть помещены в отдельные вложенные подсписки.  
Пример:

    ?- pack([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[a,a,a,a],[b],[c,c],[a,a],[d],[e,e,e,e]]

**1.10 (\*) Кодирование повторов для списка.**  
Используйте результаты выполнения задачи 1.09 для реализации алгоритма сжатия данных методом [кодирования длин серий](http://en.wikipedia.org/wiki/Run-length_encoding). 
Последовательные дубликаты элементов кодируются записью [N,E], где N - это количество дубликатов элемента E.  
Пример:

    ?- encode([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[4,a],[1,b],[2,c],[2,a],[1,d][4,e]]

**1.11 (\*) Модификация алгоритма кодирования повторов.**  
Измените результат выполнения задачи 1.10 такми образом, что если элемент не имеет дубликатов, он просто копируется в результирующий список. 
Только элементы с дубликатами заменяйте записью [N,E].
Пример:

    ?- encode_modified([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[4,a],b,[2,c],[2,a],d,[4,e]]

**1.12 (\*\*) Разархивируйте список, запакованный алгоритмом кодирования повторов.**    
Для закодированного списка, полученного в задаче 1.11, постройте его изначальный список с дубликатами.   

**1.13 (\*\*) Кодирование повторов для списка (решение напрямую).**  
Реализуйте алгоритм сжатия методом кодирования повторов напрямую. То есть, не создавайте подсписки, содержащие дубликаты, как в задаче 1.09, а просто считайте их. Так же, как и в задаче 1.11, упростите результат, заменяя записи [1,X] на X.  

Пример:

    ?- encode_direct([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[4,a],b,[2,c],[2,a],d,[4,e]]

**1.14 (\*) Повторить элементы списка.**  
Пример:

    ?- dupli([a,b,c,c,d],X).
    X = [a,a,b,b,c,c,c,c,d,d]

**1.15 (\*\*) Повторить элементы списка заданное количество раз.**  
Пример:

    ?- dupli([a,b,c],3,X).
    X = [a,a,a,b,b,b,c,c,c]

Что будет являться результатом данного выражения?
 
    ?- dupli(X,3,Y).

**1.16 (\*\*) Удалить каждый N-ный элемент списка.**  
Пример:

    ?- drop([a,b,c,d,e,f,g,h,i,k],3,X).
    X = [a,b,d,e,g,h,k]

**1.17 (\*) Разбить список на две части с заданной длиной первой части.**  
Сделайте это без использования методов встроенных в язык библиотек.  

Пример:

    ?- split([a,b,c,d,e,f,g,h,i,k],3,L1,L2).
    L1 = [a,b,c]
    L2 = [d,e,f,g,h,i,k]

**1.18 (\*\*) Извлеките подсписок из списка.**  
Для заданных I и K, подсписком будет список с элементами с I-того по K-тый (с обеих сторон включительно).
Первый элемент имееть порядковый номер 1.

Пример:

    ?- slice([a,b,c,d,e,f,g,h,i,k],3,7,L).
    X = [c,d,e,f,g]

**1.19 (\*\*) Сдвинуть список на N позиций влево.**
Примеры:

    ?- rotate([a,b,c,d,e,f,g,h],3,X).
    X = [d,e,f,g,h,a,b,c]

    ?- rotate([a,b,c,d,e,f,g,h],-2,X).
    X = [g,h,a,b,c,d,e,f]

Подсказка: Используйте встроенные в язык методы length/2 и append/3, и результаты задачи 1.17.

**1.20 (\*) Удалить K-тый элемент из списка.**  
Пример:

    ?- remove_at(X,[a,b,c,d],2,R).
    X = b
    R = [a,c,d]

**1.21 (\*) Вставить элемент в список на заданную позицию.**  
Пример:

    ?- insert_at(alfa,[a,b,c,d],2,L).
    L = [a,alfa,b,c,d]

**1.22 (\*) Создать список, содержащий все целые числа заданного диапазона.**  
Пример:

    ?- range(4,9,L).
    L = [4,5,6,7,8,9]

**1.23 (\*\*) Извлечь заданное количество случайно выбранных элементов из списка.**  
Извлечённые элементы поместите в результирующий список.  
    
Пример:

    ?- rnd_select([a,b,c,d,e,f,g,h],3,L).
    L = [e,d,a]

Подсказка: Используйте встроенный генератор случайных чисел random/2 и результаты задачи 1.20.

**1.24 (\*) Лотерея: Выведите на экран N различных случайных чисел из диапазона 1..M.**  
Извлечённые элементы поместите в результирующий список.  
Пример:

    ?- rnd_select(6,49,L).
    L = [23,1,17,33,21,37]

Подсказка: Комбинируйте решения задач 1.22 и 1.23.

**1.25 (\*) Сгенерируйте случайную перестановку элементов списка.**  
Пример:

    ?- rnd_permu([a,b,c,d,e,f],L).
    L = [b,a,d,c,e,f]

Подсказка: Используйте решение к задаче 1.23.

**1.26 (\*\*) Выведите все комбинации неповторяемых K элементов, выбранных из набора N**
Сколько есть вариантов выбрать комитет из 3-х человек из группы в 12 человек? Мы все знаем, что это C(12,3) = 220 разных вариантов (C(N,K) обозначает биноминальный коэффициент). Для математиков данный результат был бы достаточен. Но мы хотим увидеть не только цифру, но и все возможные комбинации.  

Пример:

    ?- combination(3,[a,b,c,d,e,f],L).
    L = [a,b,c] ;
    L = [a,b,d] ;
    L = [a,b,e] ;
    ...

**1.27 (\*\*) Сгруппируй элементы из набора в непересекаемые подмножества.**
a) Сколькими способами группа из 9 человек может работать в трёх подгруппах по 2, 3 и 4 человека? Напишите метод, генерирующий все возможные варианты.  

Пример:

    ?- group3([aldo,beat,carla,david,evi,flip,gary,hugo,ida],G1,G2,G3).
    G1 = [aldo,beat], G2 = [carla,david,evi], G3 = [flip,gary,hugo,ida]
    ...

b) Обобщите решение таким образом, чтобы можно было задавать список с размерами групп, а метод будет возвращать список этих групп.   

Пример:

    ?- group([aldo,beat,carla,david,evi,flip,gary,hugo,ida],[2,2,5],Gs).
    Gs = [[aldo,beat],[carla,david],[evi,flip,gary,hugo,ida]]
    ...

Примечание: Исключите из результатов перестановки внутри группы, то есть результаты [[aldo,beat],...] и [[beat,aldo],...] рассматривайте как одно решение.
При этом рассматривайте как два разных решения перестановки самих групп. 
Например: [[aldo,beat],[carla,david],...] и [[carla,david],[aldo,beat],...] будут разными решениями.

Более детально ознакомиться с данной задачей на комбинаторику вы можете в любой хорошей книге по дискретной математике 
(под термином "мультиноминальные коэффициенты" - multinomial coefficients).

**1.28 (\*\*) Сортировка списка списков по длине его элементов (подсписков)**
a) Для данной задачи мы предполагаем, что список InList состоит из элементов, которые также являются списками. 
Наша цель - отсортировать элементы списка InList по длине, от самых коротких, к длинным, или наоборот.    

Пример:

    ?- lsort([[a,b,c],[d,e],[f,g,h],[d,e],[i,j,k,l],[m,n],[o]],L).
    L = [[o], [d, e], [d, e], [m, n], [a, b, c], [f, g, h], [i, j, k, l]]

b) Также, как и в подпункте а), мы предполагаем, что список (InList) содержит элементы-списки.
Но в этот раз задача - отсортировать элементы списка InList по частоте появления длин списков.  
То есть, сначала идут списки, длина которых наиболее редкая, затем те, длина которых появляется чаще.   

Пример:

    ?- lfsort([[a,b,c],[d,e],[f,g,h],[d,e],[i,j,k,l],[m,n],[o]],L).
    L = [[i, j, k, l], [o], [a, b, c], [f, g, h], [d, e], [d, e], [m, n]]

Отметьте, что в приведённом примере, первые два списка из результирующего списка L имеют длину 4 и 1, 
такие длины встречаются среди всех списков всего лишь по одному разу. 
Третий и четвёртый списки имеют длину 3, то есть имеется два списка этой длины. 
И наконец, последние три списка имеют длину 2. Такая длина наиболее часто встетилась в исходном наборе.