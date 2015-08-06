---
categories:
- scala
- intro
title: REPL. Переменные и выражения. Блоки
---

## Консоль

Много интересных вещей в Scala можно проверять быстро в REPL (read-eval-print-loop) консоли, которая может быть вызвана командой *sbt console* или *scala*:

{% highlight console %}
$ scala

scala> 1
res0: Int = 1

{% endhighlight %}

## Переменные

Переменные создаются ключевым словом *var*, их значения можно менять.

{% highlight scala %}
scala> var x: Int = 5
x: Int = 5

scala> x + 10
res2: Int = 15

scala> x = 15
x: Int = 15

scala> x + 10
res3: Int = 25
{% endhighlight %}

##  Неизменяемые значения

В Scala, как и в других функциональных языках, принято минимизировать количество побочных эффектов (side effects), одним из которых является работа с переменными. Вместо них используются неизменяемые значения (immutable values).

{% highlight scala %}
scala> val x: Int = 5
x: Int = 5

scala> x + 10
res4: Int = 15

scala> x = 15
<console>:8: error: reassignment to val
       x = 15
         ^

scala> x + 10
res5: Int = 15
{% endhighlight %}

Использование неизменяемых значений вместо переменных облегчает читаемость, тестируемость и параллелизацию программ.

## Выражения

Важно понимать, что в Scala нет деления на выражения возвращающие значения и действия, как в других императивных языках программирования.

Пример выражения и действия в C#:
{% highlight c# %}
// действие
int x;
if (y > 10)
{
  x = -1
}
else
{
  x = 1
}

// выражение
int x = (y > 10) ? -1 : 1;
{% endhighlight %}

Благодаря этому возможны конструкции непривычные в C#:

{% highlight scala %}
val x: Int = if (y > 10) -1 else 1

val z: Int = try {
  // load from file, return line count
  nLines
} catch {
  case _ => 0
}
{% endhighlight %}

## Unit

Но как же выражения которые не возвращают значений?

Аналогом ключевого слова *void* из других языков в Scala является тип *Unit* единственным значением которого является ().

{% highlight scala %}
scala> val p = println("hello")
hello
p: Unit = ()

scala> var x: Int = 5
x: Int = 5

scala> x += 5

scala> val y = x += 5
y: Unit = ()
{% endhighlight %}


## Блоки

Любое выражение в Scala может быть заменено блоком из нескольких выражений обернутым {}. Значением блока будет последнее значение в нем.

{% highlight scala %}
val x = 5
val y = {
  val z = x + 5 // переменная не видна снаружи блока
  z - 3
}
// здесь y = 7
{% endhighlight %}

## Упражнение

TODO
