dalam kotlin ada dua konsep `equality`, yaitu
1. structural
2. referential

#### Structural

*Structural equality* adalah komparasi antara dua *value*, yang apakah dua *value* tersebut berasal dari `equals()` yang sama.

```kotlin
val a = "a"
val b = "a"
val person = Person("helo", age = 20)
val person2 = Person("helo", age = 20)

println(a == b) //true
pritnln(person == person2) //true

data class Person(val name: String, val age: Int)
```

#### Referential

*Referential equality* adalah komparasi antara dua value, apakah value tersebut berasal dari memory addresss yang sama.

```kotlin
val a = "a"
val b = a

val person = Person("helo", age = 20)
val person2 = Person("helo", age = 20)

println(a === b)
println(person === person2) // false
```

l