# Kotlin Bucket
Random stuff in Kotlin language

# Not null check for multiple arguments. 
In this implementation, one can use existing classes `Pair` and `Triple`.
```kotlin
inline fun <reified A, reified B> Pair<A?, B?>.takeIfNotNull(): Pair<A, B>? {
  val a = first ?: return null
  val b = second ?: return null
  return Pair(a, b)
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
inline fun <reified A, reified B, R> Pair<A?, B?>.runIfNotNull(body: (A, B)->R): R? {
  return takeIfNotNull()?.let { (a, b) -> body(a, b) }
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
