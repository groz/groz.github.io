---
categories:
- scala
- intro
title: Коллекции
---

Источник: [документация](http://docs.scala-lang.org/overviews/collections/introduction.html) + собственные примеры

Все коллекции в Scala разделены на множество мутабильных (mutable) и неизменяемых (immutable) коллекций. В первых можно добавлять или удалять элементы, вторые же неизменны с момента создания. В них тоже можно добавлять элементы и использовать остальные принятые операции, но все они возвращают новую коллекцию.

Практически все необходимые коллекции находятся в пакетах scala.collection, scala.collection.mutable и scala.collection.immutable. 

По умолчанию Scala использует неизменяемый вариант коллекции, то есть если вы указываете Map или Set без префикса, то будет использован scala.collection.immutable вариант.

Все коллекции (mutable и immutable) реализуют один из следующих абстрактных классов или интерфейсов (trait):

![Collections]({{ site.url }}/images/collections.png)

Работа с неизменяемыми коллекциями проще и безопаснее, особенно в многопоточной среде. В дальнейшем будем обсуждать только их.

Иерархия конкретных immutable коллекций:

![Imutable collections]({{ site.url }}/images/collections.immutable.png)

У всех коллекций много общей функциональности, например, их можно создавать указывая имя коллекции и перечисляя элементы:

{% highlight scala %}
Traversable(1, 2, 3)
Iterable("x", "y", "z")
Map("x" -> 24, "y" -> 25, "z" -> 26)
Set(Color.red, Color.green, Color.blue)
SortedSet("hello", "world")
Buffer(x, y, z)
IndexedSeq(1.0, 2.0)
LinearSeq(a, b, c)
{% endhighlight %}

## Traversable и операции

Остальные общие операции доступны на базовом интерфейсе Traversable, который объявлен следующим образом:

{% highlight scala %}
  def foreach[U](f: A => U): Unit
{% endhighlight %}

Этого достаточно для использования множества общих операций, которые можно разделить на следующие категории:

### Перечисление

**xs foreach p** - вызывает функцию p на каждом элементе коллекции xs

{% highlight scala %}
scala> List(1, 2, 3).foreach(println)
1
2
3
{% endhighlight %}

### Сложение

**xs ++ ys** - возращает коллекцию из элементов коллекций xs и ys
{% highlight scala %}
scala> List(1, 2, 3) ++ List(4, 5)
res4: List[Int] = List(1, 2, 3, 4, 5)

scala> Set(1, 2, 3) ++ Set(4, 5)
res5: scala.collection.immutable.Set[Int] = Set(5, 1, 2, 3, 4)
{% endhighlight %}

### Отображения

**xs map f** - коллекция полученная применением функции f ко всем элементам xs
{% highlight scala %}
scala> List(1, 2, 3).map(x => x + 1)
res2: List[Int] = List(2, 3, 4)

scala> List("a", "ab", "abc").map(s => s.length)
res3: List[Int] = List(1, 2, 3)
{% endhighlight %}

**xs flatMap f** - передает все элементы коллекции функции f, которая в свою очередь возвращает коллекцию, и конкатенирует результаты

{% highlight scala %}
scala> List("a", "bb", "ccc").flatMap(s => s.toList)
res10: List[Char] = List(a, b, b, c, c, c)

scala> Set("a", "bb", "ccc").flatMap(s => s.toList)
res11: scala.collection.immutable.Set[Char] = Set(a, b, c)
{% endhighlight %}

**xs collect f** - применяет частичную функцию (partial function) f ко всем элементам xs на которых она определена и собирает результаты

{% highlight scala %}
scala> List(1, 2.5, "some string", 3).collect {
  case x: Int => x
  case y: Double => y.toInt
}
res7: List[Int] = List(1, 2, 3)
{% endhighlight %}

Примечание: 

Синтаксис 
{% highlight scala %}
{
  case <условие 1> => <выражение>
  case <условие 2> => <выражение>
}
{% endhighlight %}

является сокращенным для 
{% highlight scala %}
x => x match {
  case <условие 1> => <выражение>
  case <условие 2> => <выражение>
}
{% endhighlight %}

и возвращает частично-определенную функцию.

### Преобразования

**xs.toArray** - преобразует xs в массив (иногда полезно для взаимодействия с Java-кодом)

**xs.toList** - преобразует xs в список List

**xs.toStream** - пробразует xs в лениво-вычисляемый поток Stream (полезно если вы применяете много операций над коллекцией и не хотите проходить по ней много раз). Важное отличие от некоторых аналогов в других языках (например IEnumerable в C#) в том, что коллекция будет развернута 0 или 1 раз, а не с каждым новым проходом.

**xs.toSet** - преобразует xs в множество Set

**xs.toMap** - преобразует коллекцию пар (a, b) в отображение Map (не компилируется, если элементы коллекции не являются парами)

{% highlight scala %}
scala> List(1, 2, 3).toArray
res0: Array[Int] = Array(1, 2, 3)

scala> Map("a" -> 1, "b" -> 2).toList
res1: List[(String, Int)] = List((a,1), (b,2))

scala> List(1, 2, 3).toStream
res2: scala.collection.immutable.Stream[Int] = Stream(1, ?)

scala> List(1, 2, 2, 3).toSet
res3: scala.collection.immutable.Set[Int] = Set(1, 2, 3)

scala> List(("a", 10), ("b", 20), ("a", 30)).toMap
res4: scala.collection.immutable.Map[String,Int] = Map(a -> 30, b -> 20)
{% endhighlight %}

### Информация о размере

**xs.isEmpty**

**xs.nonEmpty**

**xs.size** - количество элементов в xs

**xs.hasDefiniteSize** - известен ли у xs конечный размер

{% highlight scala %}
scala> List(1).isEmpty
res0: Boolean = false

scala> List().nonEmpty
res1: Boolean = false

scala> List().hasDefiniteSize
res2: Boolean = true

scala> Stream(1, 2).hasDefiniteSize
res3: Boolean = false
{% endhighlight %}

### Доступ к элементам

**xs.head** - первый элемент коллекции (или какой-либо элемент, если порядок не определен)

**xs.headOption** - первый элемент обернутый в Option или None, если коллекция пуста

**xs.last** - последний элемент коллекции (или какой-либо элемент, если порядок не определен)

**xs.lastOption** - последний элемент обернутый в Option или None, если коллекция пуста

{% highlight scala %}
scala> List(1, 2, 3).head
res0: Int = 1

scala> List().headOption
res1: Option[Nothing] = None

scala> Set(1, 2, 3, 4, 5).last
res2: Int = 4
{% endhighlight %}

**xs find p** - Option содержащий первый элемент удовлетворяющий условию p или None, если таких элементов нет

{% highlight scala %}
scala> List(1, 2, 3).find(_ % 2 == 0)
res0: Option[Int] = Some(2)

scala> List(1, 2, 3).find(_ > 5)
res1: Option[Int] = None
{% endhighlight %}

