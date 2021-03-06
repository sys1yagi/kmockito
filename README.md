# DEPRECATED

use https://github.com/nhaarman/mockito-kotlin

# kmockito

[![Circle CI](https://circleci.com/gh/sys1yagi/kmockito.svg?style=svg)](https://circleci.com/gh/sys1yagi/kmockito)
[![Release](https://jitpack.io/v/sys1yagi/kmockito.svg)](https://jitpack.io/sys1yagi/kmockito)

Mockito for Kotlin.

`when` is a reserved word of Kotlin. You should write like below on test.

```java
val item = mock(Item::class.java)
`when`(item.length()).thenReturn(10)
```

hmm... OK. I'll add extensions for Kotlin.

## mock

__before__

```java
var item = mock(Item::class.java)
`when`(item.length()).thenReturn(10)

assertThat(item.length(), `is`(10))
verify(item).length()
```

__after__

```java
var item: Item = mock()
item.length().invoked.thenReturn(10)

assertThat(item.length(), `is`(10))
item.verify().length()
```

I recommend that you also use [knit](https://github.com/ntaro/knit). It is JUnit API for Kotlin.

## spy

__before__

```java
var item = spy(Item(10))
doReturn(11).`when`(item).length()

assertThat(item.length(), `is`(11))
verify(item, times(1)).length()
```

__after__

```java
var item = Item(10).spy()
item.doReturn(11).length()

assertThat(item.length(), `is`(11))
item.verify(times(1)).length()
```

## Answer

__before__

```java
model.someMethod(anyInt(), anyString(), any(), any())
    .invoked
    .thenAnswer {
      val a = it.arguments[0] as Int
      val b = it.arguments[1] as String
      val c = it.arguments[2] as Item
      val d = it.arguments[3] as Name

      // do something
    }
```

__after__

```java
model.someMethod(anyInt(), anyString(), any(), any())
    .invoked
    .thenAnswer {
      val (a, b, c, d) = it.arguments4<Int, String, Item, Name>()

      // do something
    }
```


## Installation

This library is distributed by [JitPack](https://jitpack.io/). Add dependencies your build.gradle

Add it in your root build.gradle at the end of repositories:

```groovy
allprojects {
  repositories {
    ...
    maven { url "https://jitpack.io" }
  }
}
```

 Add the dependency

```groovy
testCompile 'com.github.sys1yagi:kmockito:0.1.2'
```

## Development


__Show version__

```
$ ./gradlew version
```

__Bump version__

```
$ ./gradlew bumpMajor
$ ./gradlew bumpMinor
$ ./gradlew bumpPatch
```

__Generate README__

```
$ ./gradlew genReadMe
```
