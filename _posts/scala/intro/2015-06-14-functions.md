---
categories:
- scala
- intro
title: Функции
---

Объявление функции по правилам Scala:

{% highlight scala %}
def abs(x: Int): Int = {
  if (x > 0)
    return x
  else
    return -x
}
{% endhighlight %}

Оно упрощается несколькими правилами:

- если функция содержит одно выражение, то { } можно опустить
- последнее выражение в функции автоматически возвращается (return)
- тип возвращаемого значения не-рекурсивной функции может быть выведен из правой части

Используя эти правила функция *abs* может быть переписана в следующем виде:

{% highlight scala %}
def abs(x: Int) = if (x > 0) x else -x
{% endhighlight %}

## Методы

Еще два правила делают работу с объектами и методами более естественной:

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

Одним из важнеших свойств функционального программирования является возможность использовать функции как значения. Соответственно, каждая функция имеет определенный тип.
Например, функция принимающая число и возвращающая строку имеет тип

{% highlight scala %}
Double => String
{% endhighlight %}

Переменную такого типа можно объявить в коде и присвоить ей значение, а так же передать в другую функцию:

{% highlight scala %}
def f1(x: Double): String = s"$x RUB"
def f2(x: Double): String = s"$x USD"

val f: Int => String = f1
f(125) // 125 RUB

class BankAccount(amount: Double) {
  def printSum(moneyFormatter: Double => String) =
    println( moneyFormatter(amount) )
}
{% endhighlight %}

## Лямбда-выражения

Функцию не обязательно объявлять при помощи ключевого слова *def*. Есть еще один синтаксис, который принято называть лямбда-выражением:
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

