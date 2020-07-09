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
