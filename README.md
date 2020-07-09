# Kotlin Bucket
Random stuff in Kotlin language

# Not null check for multiple arguments. 
In this implementation, one can use existing classes `Pair` and `Triple`.
```kotlin
inline fun <reified T1, reified T2> Pair<T1?, T2?>.takeIfNotNull(): Pair<T1, T2>? {
  first ?: return null
  second ?: return null
  return Pair(first!!, second!!)
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
inline fun <reified T1, reified T2, R> Pair<T1?, T2?>.runIfNotNull(body: (T1, T2)->R): R? {
  return takeIfNotNull()?.let { (t1, t2) -> body(t1, t2) }
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
