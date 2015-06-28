---
categories:
- scala
- intro
title: Разбиения и свёртки
---

### Функции возвращающие подмножества

**xs.tail** - все элементы xs кроме первого
{% highlight scala %}
scala> List(1, 2, 3).tail
res0: List[Int] = List(2, 3)

scala> List(1).tail
res1: List[Int] = List()

scala> List().tail
java.lang.UnsupportedOperationException: tail of empty list
{% endhighlight %}

**xs.init** - все элементы xs кроме последнего

**xs take n** - коллекция состоящая из первых n элементов xs (или любых n элементов, если порядок элементов в коллекции не определен)

{% highlight scala %}
scala> List(1, 2, 3).take(5)
res0: List[Int] = List(1, 2, 3)

scala> Set(1, 2, 3, 4, 5).take(2)
res1: scala.collection.immutable.Set[Int] = Set(5, 1)

scala> List(1, 2, 3).take(2)
res2: List[Int] = List(1, 2)
{% endhighlight %}

**xs drop n** - часть коллекции за вычетом (xs take n)

**xs takeWhile p** - префикс коллекции наибольшей длины в котором все элементы удовлетворяют функции-предикату p

{% highlight scala %}
scala> List(1, 2, 3, 4, 5).takeWhile(_ < 3)
res0: List[Int] = List(1, 2)

scala> Set(1, 2, 3, 4, 5).takeWhile(_ < 3)
res1: scala.collection.immutable.Set[Int] = Set()
{% endhighlight %}

**xs dropWhile p** - коллекция без наибольшего префикса элементов удовлетворяющих p

**xs filter p** - коллекция состоящая из всех элементов xs удовлетворяющих фильтру p

{% highlight scala %}
scala> List(1, 2, 3, 4, 5).filter(_ % 2 == 0)
res11: List[Int] = List(2, 4)
{% endhighlight %}

**xs withFilter p** - "ленивый фильтр"

**xs filterNot p** - коллекция состоящая из всех элементов xs не удовлетворяющих p

### Разбиения

**xs splitAt n**  - делит xs на индексе n, возвращая пару коллекций (xs take n, xs drop n)

{% highlight scala %}
scala> List("a", "b", "c", "d").splitAt(2)
res0: (List[String], List[String]) = (List(a, b),List(c, d))
{% endhighlight %}

**xs span p** - разбивает xs с начала относительно предиката p, возвращая пару коллекций (xs takeWhile p, xs.dropWhile p)

{% highlight scala %}
scala> val (smaller, bigger) = List(1, 2, 3, 4, 5).span(_ < 3)
smaller: List[Int] = List(1, 2)
bigger: List[Int] = List(3, 4, 5)
{% endhighlight %}

**xs partition p** - делит коллекцию на пару из двух коллекций, первая из элементов удовлетворяющих предикату p, а вторая нет (xs filter p, xs.filterNot p)

{% highlight scala %}
scala> scala> val (evens, odds) = List(1, 2, 3, 4, 5).partition(_ % 2 == 0)
evens: List[Int] = List(2, 4)
odds: List[Int] = List(1, 3, 5)
{% endhighlight %}

**xs groupBy f** - группирует элементы xs в соответствии с ключом-дискриминатором заданным функцией f

{% highlight scala %}
scala> List(-2, -1, 0, 1, 2).groupBy(x => x * x)
res0: scala.collection.immutable.Map[Int,List[Int]] = Map(4 -> List(-2, 2), 1 -> List(-1, 1), 0 -> List(0))

scala> Set("a", "b", "aa", "bb", "ccc").groupBy(_.length)
res1: scala.collection.immutable.Map[Int,scala.collection.immutable.Set[String]] = Map(2 -> Set(aa, bb), 1 -> Set(a, b), 3 -> Set(ccc))
{% endhighlight %}

### Проверки предикатов

**xs forall p** - возвращает true если все элементы xs удовлетворяют p

{% highlight scala %}
scala> List(1, 2, 3).forall(_ < 5)
res0: Boolean = true
{% endhighlight %}

**xs exists p** - возвращает true, если хоть один элемент в xs удовлетворяет p

{% highlight scala %}
scala> List(1, 2, 3).exists(_ > 5)
res0: Boolean = false
{% endhighlight %}

**xs count p** - возвращает количество элементов в xs удовлетворяющих предикату p

{% highlight scala %}
scala> List(1, 2, 3, 4, 5).count(_ % 2 == 0)
res0: Int = 2
{% endhighlight %}

### Свертки
Эти функции позволяют заменить большую часть императивных циклов (все остальные можно заменить рекурсией).
Суть свёртки заключается в том, чтобы взяв некоторое начальное значение применить некоторую бинарную операцию к нему и к первому элементу элементу коллекции, затем применить эту же операцию к результату и второму элементу коллекции и т.д.

Рассмотрим простой императивный цикл суммирующий все элементы xs:
{% highlight scala %}
scala> val xs = List(1, 2, 3, 4, 5)
var sum = 0
for (x <- xs)
  sum = sum + x
sum
res0: Int = 15
{% endhighlight %}

Здесь можно выделить начальное значение sum равное 0 и бинарную операцию (a, b) => a + b. Мы применяем эту операцию к 0 и первому элементу коллекции, затем к результату и второму элементу и т.д.

То есть, суть свёртки в том, чтобы взяв в качестве аккумулятора начальное значение z, прогнать его через всю коллекцию применяя некоторую операцию op.

**xs.foldLeft(z)(op)** - применить бинарную операцию op к последовательным элементам xs двигаясь слева направо и начиная с z (левая свёртка)

{% highlight scala %}
// (((((0 + 1) + 2) + 3) + 4) + 5)
scala> List(1, 2, 3, 4, 5).foldLeft(0) { (a, b) => a + b }
res0: Int = 15

// начальное значение не должно иметь один тип с элементами xs
scala> List("a", "bb", "ccc").foldLeft(0) { (acc, x) => acc + x.length }
res0: Int = 6
{% endhighlight %}

**xs.foldRight(z)(op)** - применить бинарную операцию к последовательным элементам xs двигаясь справа налево и начиная с z (правая свёртка)

{% highlight scala %}
// (1 - (2 - (3 - (4 - (5 - 0)))))
scala> List(1, 2, 3, 4, 5).foldRight(0){ (x, acc) => x - acc }
res0: Int = 3

scala> List(1, 2, 3, 4, 5).foldRight(0){ (x, acc) => acc - x }
res0: Int = 15
{% endhighlight %}

**(z /: xs)(op)** - то же, что и xs.foldLeft(z)(op)

**(xs :\ z)(op)** - то же, что и xs.foldRight(z)(op)

**xs reduceLeft op** - foldLeft для непустой коллекции, где в качестве начального значения аккумулятора берется ее первый элемент

{% highlight scala %}
scala> List(1, 2, 3).reduceLeft {_ + _}
res0: Int = 6

scala> List(1, 2, 3).filter(_ > 5).reduceLeft {_ + _}
java.lang.UnsupportedOperationException: empty.reduceLeft
{% endhighlight %}

**xs reduceRight op** - для foldRight то же, что и reduceLeft для foldLeft

### Специализированные свёртки

Некоторые виды свёрток применяются настолько часто, что для них есть специализированные методы.

**xs.sum** - возвращает сумму элементов xs

{% highlight scala %}
scala> List(1, 2, 3, 4, 5).sum
res0: Int = 15

// то же самое через foldLeft
scala> List(1, 2, 3, 4, 5).foldLeft(0) {_ + _}
res1: Int = 15
{% endhighlight %}

**xs.product** - возвращает произведение элементов xs

**xs.min** - минимальный из элементов коллекции xs, если на них определен порядок

{% highlight scala %}
scala> List(1, 2, 3).min
res0: Int = 1

scala> Map(1 -> "a", 2 -> "b").min
res1: (Int, String) = (1,a)

scala> List(List(1, 2), List(1, 2, 3)).min
<console>:8: error: No implicit Ordering defined for List[Int].
              List(List(1, 2), List(1, 2, 3)).min

// то же самое явной свёрткой
scala> List(4, 3, 1, 2).reduceLeft { (acc, x) => if (x < acc) x else acc }
res3: Int = 

// или еще проще
scala> List(4, 3, 1, 2).reduceLeft(Math.min)
res4: Int = 1
{% endhighlight %}

**xs.max** - максимальный из упорядоченных элементов коллекции xs

**xs.mkString(sep)** - перевести в строку все элементы коллекции разделив их опциональным разделителем sep 

{% highlight scala %}
scala> List(42, 43, 44).mkString(" -> ")
res0: String = 42 -> 43 -> 44

// то же самое через явную свёртку
scala> List(42, 43, 44).map(_.toString).reduceLeft { (str, x) => str + " -> " + x}
res1: String = 42 -> 43 -> 44
{% endhighlight %}