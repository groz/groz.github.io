---
categories:
- scala
- intro
title: Функции
---

Функция в Scala объявляется при помощи ключевого слова *def*:

{% highlight scala %}
def <имя функции>(<параметры>): <возвращаемое значение> = <выражение>
{% endhighlight %}

Например:
{% highlight scala %}
def sum(a: Int, b: Int): Int = a + b
def abs(x: Int) = if (x > 0) x else -x

scala> sum(2, 2)
res1: Int = 4

scala> abs(-5)
res2: Int = 5
{% endhighlight %}

Из предыдущего урока вспомним, что любое выражение можно заменить блоком, последнее выражение которого будет результатом. Таким образом можно писать более нагруженные функции:

{% highlight scala %}
// находит максимум из трех чисел
def max(a: Int, b: Int, c: Int): Int = {
  val max_ab = if (a > b) a else b
  if (max_ab > c) max_ab else c
}
{% endhighlight %}

## Методы

Скала объектно-ориентированный язык, поэтому функции могут принадлежать объектам.

Некоторые правила делают работу с объектами и их методами более естественной:

- если метод класса принимает один аргумент, то .() можно опустить
- идентификатором в Scala может быть практически любая последовательность символов

{% highlight scala %}
class Vector2(val x: Double, val y: Double) {
  def add(other: Vector2) = new Vector2(x + other.x, y + other.y)
  def +(other: Vector2) = add(other)
}
// ...
val v1 = new Vector(10, 20)
val v2 = new Vector(5, 6)
val v3 = v1 add v2
val v4 = v3 + v1
{% endhighlight %}

## Функции как значения

Одним из важнеших свойств функционального программирования является возможность использовать функции как значения. Соответственно, каждая функция имеет определенный тип:

{% highlight scala %}
<типы аргументов> => <тип возвращаемого значения>
{% endhighlight %}

Например, функция принимающая число и возвращающая строку имеет тип

{% highlight scala %}
Double => String
{% endhighlight %}

Переменную такого типа можно объявить в коде и присвоить ей значение, а так же передать в другую функцию:

{% highlight scala %}
def f1(x: Double): String = s"$x RUB"
def f2(x: Double): String = s"$x USD"

val f: (Double => String) = f1
f(125) // значением будет "125.0 RUB"

class BankAccount(amount: Double) {
  def printSum(moneyFormatter: Double => String) =
    println( moneyFormatter(amount) )
}
{% endhighlight %}

## Лямбда-выражения

Для краткости объявления функции можно использовать так называемые лямбда-выражения вида:

{% highlight scala %}
(аргументы) => выражение
{% endhighlight %}

Пример:
{% highlight scala %}
bankAccount.printSum(x => s"$x printed by lambda function")
{% endhighlight %}

## Функции как возвращаемые значения

Одно из нетривиальных использований функций - возвращаемые значения из других функций.

{% highlight scala %}

def sum(a: Double, b: Double) = a + b

def bind1st(value: Double): (Double => Double) = b => sum(value, b)

def plus10 = bind1st(10)

println( plus10(5) ) // 15

{% endhighlight %}
