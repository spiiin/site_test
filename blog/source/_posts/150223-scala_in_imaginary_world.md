---
title: Scala in imaginary world
tags:
  - nes
  - scala
  - hack
abbrlink: 3351183716
date: 2015-02-23 02:59:00
---
Прослушал [лекции Мартина Одерски на курсере по Scala](https://class.coursera.org/progfun-004).

Курс читался два раза, и следующего набора пока нет, поэтому вместо решения предложенных заданий и для теста языка, я решил попробовать портировать [Алгоритм разминирования бомб Джеймса Бонда](http://spiiin.livejournal.com/1766.html), написанный мной на Python'e 5 лет назад.

За основу взял код Мартина из итоговой лекции, которой использовался для решения задачи The Water Pouring Problem (как получить X литров воды, имея заданное число стаканов разного литража без маркировки), так как она решается в общем случае тем же алгоритмом - методом поиска в ширину.

Именно эту задачу было интересно решить по нескольким причинам: 

В лекции алгоритм оставлен "сырым" (в него добавлен для оптимизации список уже пройденных вершин и кеширование последнего состояния вместо его вычисления, и то, только для того, чтобы он не тормозил на совсем простых данных).

В моей задаче исходные данные из игры, из которых одна из загадок методом полного перебора решается очень долго, поэтому алгоритм нужно оптимизировать.

Так что была и простая подзадача - переделать алгоритм для решения задачи с бомбами, а не со стаканами; и более сложная - ускорить его работу.

После окончания можно сравнить результаты с решением оригинальной задачи. В частности, по количеству строк кода, а то Python нравится как раз лаконичностью и удобным Repl, простые задачки можно решать, не выходя из него.

Так что:
![james_bond_jr](http://ic.pics.livejournal.com/spiiin/20318251/41389/41389_original.png "james_bond_jr")

Оригинальный код, перезаточенный для решения задачи разминирования бомбы, решил 3 задачи из 5, на остальных стал выдавать *"java.lang.OutOfMemoryError: GC overhead limit exceeded*" из-за разрастающегося размера массива входных данных.

Решение проблемы - не исследовать все возможные пути, а в первую очередь подробно рассматривать те, которые приближают к ответу.

Для того, чтобы реализовать это, к классу описания пути нужно примешать `trait Ordered`: 

```scala
  class Path(path: List[Move], val endState: State) extends Ordered[Path]{
    def compare(that: Path): Int = {
      def diffFromEnd(x:Path) :Int = (x.endState zip endState).count( {case (x,y) => x!=y} )
      diffFromEnd(this) - diffFromEnd(that)
    }
   ...
  }
```

 функции сравнения - количество символов, которые стоят не своих местах (***diffFromEnd***).
 
Также надо сменить контейнер, хранящий Path, с Set на Vector или любой другой, поддерживающий хранение данных в отсортированном виде, и в определённый момент (например, после раскрытия всех вершин из начала потока), отсортировать вершины по порядку наибольшей близости к желаемому состоянию endState.
 
Ещё один шаг оптимизации - можно допустить, что из состояния, более близкого к решению (похожего на решение), до самого решения нужно будет сделать меньшее количество шагов, чем из состояния, менее похожего на решение.
 
Тогда можно отбросить более далёкие от решения варианты и перестать их обрабатывать(при этом есть риск выбросить и само оптимальное решение, всё зависит от качества оценивающей функции). Я решил обрубать по 50000 вариантов - решение при этом всё равно нашлось за 9 ходов, как и в Python-версии:  
```scala
val sortedMore = (more take 50000).sorted
```

 [![](http://pics.livejournal.com/spiiin/pic/00006g9d/s320x240)](http://pics.livejournal.com/spiiin/pic/00006g9d/)

 ```scala
 MoveLeft(2) //двигаем третью строку влево
 MoveUp(3) //двигаем четвёртый столбец вверх
 MoveRight(1) //двигаем вторую строку вправо
 MoveDown(2) //двигаем третий столбец вниз
 MoveRight(0) //двигаем первую строку вправо
 MoveDown(1) //двигаем второй столбец вниз
 MoveRight(0) //двигаем первую строку вправо
 MoveLeft(2) //двигаем третью строку влево
 MoveLeft(2) //двигаем третью строку влево
 ```

 Что полностью аналогично решению на Python для той же злополучной 4-й ракеты.
 [![](http://pics.livejournal.com/spiiin/pic/00008d4x/s320x240)](http://pics.livejournal.com/spiiin/pic/00008d4x/)
 В итоге, получилось по строкам кода:
 [Решение на Python](http://www.everfall.com/paste/id.php?wvfuemnvkvin) - 120 строк
 [Решение на Scala](https://gist.github.com/spiiin/257ff552ed1c9de6ed6f) - 65 строк
 (выбросил для подсчёта функции вывода результата на python, на scala результат по умолчанию оказался читаем - список из "Имя кейскласса + параметр" - это как раз то, в каком виде хотелось увидеть ответ).
 
 Кажется, можно попробовать поэкспериментировать со Scala Worksheets для решения простых задач вместо Idle with Python.