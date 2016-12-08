## Острова
### Задача
Предположим, в один прекрасный день вы оказались на острове прямоугольный формы.
Ландшафт этого острова можно описать с помощью целочисленной матрицы размером MxN, каждый элемент которой задаёт высоту соответствующей области острова над уровнем моря.

К примеру, вот небольшой остров размером 3x3:
4 5 4
3 1 5
5 4 1

В сезон дождей остров полностью заливает водой и в низинах скапливается вода. Низиной будем считать такую область острова, клетки которой граничат с клетками, большими по высоте. При этом диагональные соседи не учитываются, а уровень моря принимается за 0. В приведённом выше примере на острове есть только одна низина — это клетка со значением 1 в середине острова (она граничит с клетками высотой 3, 5, 5 и 4).

Таким образом, после дождя высота клеток острова изменится и станет следующей:
4 5 4
3 3 5
5 4 1

Мы видим что в данном примере высота низины изменилась с 1 до 3, после чего вода начала переливаться на соседние клетки, а затем — в море. Общий объём воды, скопившейся на острове — 2 кубические клетки.

Вот пример посложнее:

5 3 4 5
6 2 1 4
3 1 1 4
8 5 4 3

После дождя карта острова принимает следующую форму:

5 3 4 5
6 3 3 4
3 3 3 4
8 5 4 2

Общий объём скопившейся после дождя воды на таком острове составляет целых 7 кубических клеток!

Ваша программа должна читать входные данные из stdin.
В первой строке указывается количество островов K, после чего в следующих строках описываются эти K островов.
В первой строке описания острова задаются его размеры N и M — целые числа в диапазоне [1, 50], разделённые пробелом.
В следующих строках описывается матрица NxM со значениями высот клеток острова, которые могут принимать значения из диапазона [1, 1000].

Вот пример входных данных:

3
3 3
4 5 4
3 1 5
5 4 1
4 4
5 3 4 5
6 2 1 4
3 1 1 4
8 5 4 3
4 3
2 2 2
2 1 2
2 1 2
2 1 2

Ваша программа должна выводить в stdout значения общего объёма воды, скапливающейся на острове после сезона дождей для каждого из входных примеров. Для приведённых выше данных, вывод программы должен быть следующим:

2
7
0



### Основная идея решения:
* Строим из данного нам массива орграф, где элементы соединены с соседними и все крайние элементы соединены с морем
* Веса рёбер подбираются в зависимости от разницы высот: 2 -> 3 - вес 1, 3 -> 2 - вес 0, потому что нас интересуют только подъёмы воды, спуски можно игнорировать
* Перебираем вершины (высоты) с наименьшего до наибольшего и строим кратчайший путь до "моря" используя алгоритм беллмана-форда (алгоритм я взял откуда то с просторов интернета, не стал писать его сам)
* Когда находится такой, путь, который "набирает воду", то мы строим последовательность высот, вида [1, 3, 2, 3] и считаем сколько она набирает воды (в текущем примере будет 1, считаем, что слева после 1 у нас море, которое 0, а справа условная стена, которая выдержит любую высоту воды, дабы дотянуться до максимально высокой левой стены)
* После этого обновляем высоты графа и повторяем пока не переберём все возможные пути
  
  
## Бесконечная последовательность
### Задача
Бесконечная последовательность
Возьмём бесконечную цифровую последовательность, образованную склеиванием последовательных положительных чисел: S = 123456789101112131415...
Определите первое вхождение заданной подпоследовательности A в бесконечной последовательности S (нумерация начинается с 1).

Программа должна читать данные из stdin и выводить ответы в stdout.

Пример входных данных (по одной подпоследовательности на строку, максимальная длина подпоследовательности — 50 символов):
6789
111

Пример выходных данных:
6
12

### Основная идея решения:
Алгоритм основывается на идее о том, что всю бесконечную последовательность можно разбить на такие группы чисел: (1-9)(10-99)(100-999)... Каждая группа может содержать в каком то виде наше число A. И мы последовательно проходим по каждой из наших групп, пытаясь найти позицию числа. Самое первое вхождение и даст нам правильный ответ. Идти необходимо максимум до len(A).

### Общий ход работы:
* По длине числа строим последовательности возможных значений, неизвестные числа заменяем звёздочками
* Заменяем звёздочки наиболее подходящими значениями и проверям является ли последовательность возрастающей
* Если да, то находим позицию первого числа последовательности и возвращаем результат, если нет, то строим дальше

### Примеры:

* Ищем A = 141
* [1, 4, 1] - смотрим не возрастающая ли, нет, идём к следующему порядку
* [\*1, 41], [14, 1\*] -> [31, 41], [14, 15] - заменили звёздочки и посмотрели: первое не подходит, второе вполне, выдаём индекс 14 как начало искомой последовательности
* Если бы мы не нашли, то пошли бы дальше и построили: [\*\*1, 41\*], [\*14, 1\*\*], [141] и попытались бы их максимально подходяще заменить на [141, 412], [114, 115], [141]


### Примечания:
* Работает быстро, однако неправильно находит позицию некоторых чисел, содержащих '9'. Например, на промежутке от 1 до 1000 он неправильно определяет 28 чисел, содержащих '9'. С девятками вообще всё нетривиально в этом алгоритме. Всё поправимо, но я не успел доделать
* В некоторых местах получилось очень запутанно, просто потому что несовсем тривиальная логика со звёздочками, возможно можно сделать проще
* Чтобы запустить выполнение нужно ввести числа и нажать enter повторно на пустой строке

  
