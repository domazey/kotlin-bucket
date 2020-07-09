# Kotlin Bucket
Random stuff in Kotlin language

# Not null check for multiple arguments. 
In this implementation, one can use existing classes `Pair` and `Triple`.
```kotlin
fun <A, B> Pair<A?, B?>.takeIfNotNull(): Pair<A, B>? {
  val a = first ?: return null
  val b = second ?: return null
  return Pair(a, b)
}

fun <A, B, C> Triple<A?, B?, C?>.takeIfNotNull(): Triple<A, B, C>? {
  val a = first ?: return null
  val b = second ?: return null
  val c = third ?: return null
  return Triple(a, b, c)
}
```
```kotlin
fun main() {
  val a: Int? = 32
  val b: Long? = 42
  (a to b).takeIfNotNull()?.let { (a, b) ->
    println("a: $a, b: $b")
  }
}
```

For shortcut, one could use: 
```kotlin
inline fun <A, B, R> Pair<A?, B?>.runIfNotNull(block: (A, B)->R): R? {
  return takeIfNotNull()?.let { (a, b) -> block(a, b) }
}

inline fun <A, B, C, R> Triple<A?, B?, C?>.runIfNotNull(block: (A, B, C)->R): R? {
  return takeIfNotNull()?.let { (a, b, c) -> block(a, b, c) }
}
```
```kotlin
fun main() {
  val a: Int? = 32
  val b: Long? = 42
  (a to b).runIfNotNull { a, b ->
    println("a: $a, b: $b")
  }
}
```

# Pair, Triple, Quadruple creation
In standard library only `Pair` has pleasant way of constructing it. Here is what one can do:

Optional: define `Quadruple`, `Quintuple` etc...
```kotlin
data class Quadruple<A, B, C, D> (val first: A, val second: B, val third: C, val fourth: D): Serializable {
  override fun toString(): String = "($first, $second, $third, $fourth)"
}
```
```kotlin
infix fun <A, B> A.nd(second: B): Pair<A, B> = this to second
infix fun <A, B, C> Pair<A, B>.rd(third: C) = Triple(first, second, third)
infix fun <A, B, C, D> Triple<A, B, C>.th(fourth: D) = Quadruple(first, second, third, fourth)
```
```kotlin
fun main() {
  val pair = 1 nd 2
  val triple = 3 nd 4 rd 5
  val anotherTriple = pair rd 6
  val quadruple = 7 nd 8 rd 9 th 10
}
```
