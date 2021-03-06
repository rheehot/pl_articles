이 글을 시작으로 총 네 개의 글을 통해 기초적인 함수형 프로그래밍에 대하여 다루려고 한다. 함수형 프로그래밍을 지금까지 접해보지 않은 사람은 네 글을 모두 차근차근 읽는 것이 앞으로 프로그래밍 언어 과목에서 과제를 하는 데 도움이 될 것이다. 반대로, 프로그래밍의 이해 과목을 수강한 사람 등 이미 함수형 프로그래밍에 대해 어느 정도 알고 있는 사람은 함수형 프로그래밍 부분을 통째로 건너뛰거나, 가볍게 Scala 문법 정도만 확인하면서 훑어보고 넘어가도 무방하다. 이번 글은 2018년 가을 학기에 진행한 Scala 세미나 중 첫 번째 세미나인 "Introduction to Functional Programming" 내용을 바탕으로 작성하였다. 세미나 [자료](https://hjaem.info/files/scala/18f/1_itfp.pdf), [코드](https://hjaem.info/files/scala/18f/1_itfp.zip), [영상](https://youtu.be/vGaWV_qs0yg) 모두 온라인으로 이용 가능하니 필요한 사람은 참고하길 바란다.

## 함수형 프로그래밍의 정의

함수형 프로그래밍이란 무엇일까? 한국어 Wikipedia에서는 다음과 같이 이야기한다. (영어 Wikipedia도 동일하다.)

> 함수형 프로그래밍은 자료 처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임의 하나이다.

여기서 '가변'은 내 글에서 '수정 가능한'이라 번역하고 있다.

책 Functional Programming in Scala에서는 다음과 같이 이야기한다.

> Functional programming (FP) is based on a simple premise with far-reaching implications: we construct our programs using only pure functions—in other words, functions that have no side effects.

책 1장의 첫 문장으로, 오직 *순수* *함수*(pure function), 즉 *부작용*(side effect)이 존재하지 않는 함수만을 사용하여 프로그램을 구성하는 것이 함수형 프로그래밍의 전제라고 설명하고 있다.

이 두 문장이면, 함수형 프로그래밍이 무엇인지 알기 위해 필요한 말은 다 나왔다고 볼 수 있다. 먼저, '수학적 함수의 계산'이라는 구절이다. 지난 글에서도 Scala에서는 모든 것이 식이며, 식은 어떤 값으로 계산된다고 언급하였다. 이처럼, 함수형 프로그래밍은 프로그램을 하나의 수식으로 생각하고 그 수식이 나타내는 값을 계산하는 것을 프로그램의 실행으로 보는 프로그래밍 방식이다. 다음 두 코드를 보면서 명령형 프로그래밍과 비교했을 때 어떤 차이가 존재하는지 생각해보자.

```c
int x = 1;
int y = 2;
if (y < 3)
    x = x + 4;
else
    x = x - 5;
```

```ocaml
let x = 1 in
  let y = 2 in
    if y < 3 then x + 4 else x - 5
```

첫 번째 코드는 명령형 언어의 대표라고 할 수 있는 C 코드이다. 명령형 프로그래밍은 실제 컴퓨터가 작동하는 방식과 유사하다. 프로그램을 실행할 때 컴퓨터의 메모리라고 생각할 수 있는 어떤 *상태(*state)가 존재하고, 코드를 실행하면서 그 상태를 변경해나간다. 위 프로그램의 실행 과정은 다음과 같다.

1. `x`와 `y` 모두 초기화되지 않은 상태
2. `x`는 `1`이고 `y`는 초기화되지 않은 상태
3. `x`는 `1`이고 `y`는 `2`인 상태
4. 현 상태에서 `y < 3`이 `true`이므로 다음 줄로 이동
5. `x`는 `5`이고 `y`는 `2`인 상태

두 번째 코드는 대표적인 함수형 언어 중 하나인 OCaml 코드이다. 프로그램은 하나의 식이며, 실행 결과는 그 식을 계산한 결과이고, 실행할 때 상태는 필요하지 않다. 위 프로그램의 실행 과정은 다음과 같다.

1. `x`는 `1`일 때, `let y = 2 in if y < 3 then x + 4 else x - 5` 계산
2. `x`는 `1`, `y`는 `2`일 때, `if y < 3 then x + 4 else x - 5` 계산
3. `x`는 `1`, `y`는 `2`일 때 `y < 3`을 계산하면 `true`이므로 `if true then x + 4 else x - 5` 계산
4. `x`는 `1`, `y`는 `2`일 때, `x + 4` 계산
5. `5`

매우 간단한 프로그램이기 때문에, 두 코드 사이에 큰 차이가 보이지는 않지만, 그 밑에 존재하는 프로그램이 무엇인지를 바라보는 관점의 차이를 명확하게 이해하는 것이 함수형 프로그래밍의 시작이다.

두 번째로 '상태와 가변 데이터를 멀리하는'과 'using only pure functions'라는 구절을 봐야 한다. 지난 글에서 함수형 프로그래밍에서는 수정 가능한 변수를 거의 사용하지 않는다고 이야기하였듯이 함수형 프로그래밍에서는 변수와 *객체*(object) 등의 데이터를 변경하는 일이 없다. 따라서 앞에서 말했듯 프로그램의 흐름에 따라 변화하는 상태가 존재하지 않는다. 또한, 변화하는 상태가 존재하지 않으므로, 한 함수는 언제나 동일한 동작을 수행하며 동일한 인자에 대해서 언제나 동일한 결괏값을 만들어낸다. 이런 함수를 순수 함수라고 한다. 이는 수학에서 함수가 가진 특징과 동일하며, 앞에서 말한 '수학적 함수의 계산'이라는 특징과도 다시 일치한다. 이처럼 함수형 프로그래밍에서 가장 중요한 개념은 *불변성*(immutability)이다.

다만, 실제 대형 프로젝트를 진행한다면, 전체 코드에서 수정 가능한 변수나 객체를 한 번도 사용하지 않는 것은 비효율적인 경우가 많다. 그렇기에, Haskell을 제외한 대부분의 실제 사용되는 함수형 언어는 Scala의 `var`, OCaml의 `ref`, Racket의 `set!`과 `box`처럼 수정 가능한 변수나 저장 공간을 제공한다. 하지만, 함수형 프로그래밍에서는 대부분의 경우 수정 불가능한 변수와 객체를 사용하며, 이런 방식으로 매우 많은 것을 큰 어려움 없이 할 수 있을 뿐만 아니라, 여러 장점을 가진다는 것을 앞으로 보게 될 것이다.

## 현업에서의 함수형 프로그래밍

함수형 프로그래밍에 대해 더 자세히 알아보기 전에, 함수형 프로그래밍을 공부할 동기를 키우기 위해서 현업에서 함수형 프로그래밍이 어떻게 사용되고 있는지 간단히 보려 한다. 지난 글에서 Scala가 어디서 사용되고 있는지 보았으니, 이번에는 다른 함수형 언어를 알아보자.

### OCaml

OCaml은 Facebook에서 Java, C, C++, Objective-C 정적 분석기인 [Infer](https://fbinfer.com/)를 만드는 데 사용되고 있다. Infer는 Facebook뿐 아니라 Amazon, Mozilla 등 여러 기업에서 코드에서 버그를 정적으로 검사하기 위해서 사용하고 있으며, 그 중요성은 Infer의 이론적 배경을 제시한 논문인 Compositional shape analysis by means of bi-abduction이 2019년 POPL에서 [ACM SIGPLAN Most Influential POPL Paper Award를 수상](https://research.fb.com/popl-2019-most-influential-paper-award-for-research-that-led-to-facebook-infer/)하여, 2009년 POPL에서 발표된 논문 중 가장 영향력 있는 논문으로 평가받았다는 사실에서도 확인할 수 있다. 사족으로, KAIST의 양홍석 교수님께서 해당 논문의 공동 저자 중 한 분이시기도 하다. Facebook에서는 JavaScript 정적 타입 검사기인 [Flow](https://flow.org/)도 OCaml로 만들고 있다.

[Jane Street](https://www.janestreet.com/)는 OCaml로 프로그램을 개발하는 금융회사로, 흔하지 않은 경우이다 보니 프로그래밍 언어 커뮤니티에서는 널리 알려져 있다.

이 밖에도 Docker를 비롯한 여러 회사에서 OCaml을 사용 중이라고 [OCaml 공식 웹 사이트에서 설명](http://ocaml.org/learn/companies.html)하고 있다.

### Haskell

[Haskell Wiki](http://wiki.haskell.org/Haskell_in_industry)에서는 Google, Facebook, Microsoft, Nvidia 등 여러 기업에서 Haskell을 사용한다고 설명하고 있다.

### Erlang과 Elixir

Erlang은 동시성 및 병렬 컴퓨팅에 특화된 함수형 언어이고, Elixir는 Erlang 가상 기계 위에서 동작하는 마찬가지로 동시성 및 병렬 컴퓨팅에 특화된 함수형 언어이다. [Code Sync](https://codesync.global/media/successful-companies-using-elixir-and-erlang/)에서는 WhatsApp, Pinterest, Goldman Sachs 등의 기업에서 사용되고 있다고 설명하고 있다.

일반 사용자가 사용할 프로그램을 함수형 언어를 통해 만드는 경우는 잘 없으나 프로그램 코드와 같은 복잡하고 추상적인 구조를 다루는 경우, 높은 신뢰성이 요구되는 프로그램인 경우, 동시성이나 병렬 컴퓨팅을 목적으로 하는 경우 등 특정한 목적을 위하여 많은 기업에서 사용하고 있음을 볼 수 있다.

## 불변성이 가지는 장점

불변성은 함수형 프로그래밍의 가장 중요한 개념이며, 불변성이 가지는 장점은 곧 함수형 프로그래밍의 장점과 직접적으로 연결된다. 책 Programming in Scala에서는 불변성이 가지는 네 개의 장점을 소개하고 있다. 원문은 다음과 같다.

> First, immutable objects are often easier to reason about than mutable ones, because they do not have complex state spaces that change over time. Second, you can pass immutable objects around quite freely, whereas you may need to make defensive copies of mutable objects before passing them to other code. Third, there is no way for two threads concurrently accessing an immutable to corrupt its state once it has been properly constructed, because no thread can change the state of an immutable. Fourth, immutable objects make safe hash table keys. If a mutable object is mutated after it is placed into a `HashSet`, for example, that object may not be found the next time you look into the `HashSet`.

책에는 단순하게 설명되어 있으니, 예시와 함께 각 장점을 살펴보도록 하겠다.

### 프로그램을 논증하기 쉽다

```scala
val x = 1
...
f(x)
```

이 코드에서는 처음에 변수 `x`를 `1`로 정의한다. `x`는 수정 불가능한 변수이므로, 마지막에 함수 `f`의 인자로 `x`를 전달할 때, 중간에 있는 코드를 전혀 보지 않아도 `x`의 값이 `1`이라고 확신할 수 있다.

```scala
var x = 1
...
f(x)
```

반면, 이 경우 `x`를 수정 가능한 변수로 선언했기 때문에, 중간에 있는 코드를 모두 읽기 전까지는 `x`가 `f`의 인자로 사용될 때 어떤 값을 가지고 있을지 알 수 없다.

이 차이는 프로그램이 작거나, 내가 처음부터 끝까지 모든 코드를 작성했거나, 프로그램을 지속해서 유지 보수할 필요가 없는 경우에는 크게 의미 있지 않다. 그러나, 이 코드가 어떤 일을 할지 전혀 모르는 상태에서 코드를 읽는다고 가정하면 큰 차이가 발생한다. 물론, 코드의 시작과 끝만 읽는 경우는 없겠지만, 첫 번째 코드는 `x`가 등장할 때 `x`가 선언된 줄만 봐도 `x`의 값을 알 수 있지만, 두 번째 코드는 `x`의 값이 어떻게 변화하는지 코드를 읽는 내내 기억하지 않으면, 코드가 어떻게 동작하는지 이해하는데 문제가 생긴다. 당연히, 수정 가능한 변수가 여러 개 존재한다면 더 어려워진다. 코드의 동작을 정확히 파악하기 어려우면, 코드가 옳은지 그른지 논증하기 힘들어지며 버그가 있을 가능성이 커진다.

수정 가능한 *자료* *구조*(data structure)를 사용하는 경우에도 비슷한 문제가 발생한다.

```scala
val x = List(1, 2)
...
f(x)
...
x
```

`List`는 Scala 표준 라이브러리에서 제공하는 수정 불가능한 리스트 자료 구조이다. 이 경우, `x`는 처음부터 끝까지 `1`과 `2`를 가진 리스트이다.

```scala
import scala.collection.mutable.ListBuffer
val x = ListBuffer(1, 2)
...
f(x)
...
x
```

`ListBuffer`는 Scala 표준 라이브러리에서 제공하는 수정 가능한 리스트 자료 구조이다. 이번에는 `x`가 가리키는 리스트에 언제든지 새로운 원소가 추가되거나 가지고 있던 원소가 삭제될 수 있다. 따라서, 함수 `f`에 인자로 넘길 때 `x`가 가지고 있는 원소를 알기 위해서는 중간의 코드를 모두 확인해야 한다. 더 큰 문제는 함수 `f` 안에서 인자로 받은 `x`에 원소를 추가하거나 삭제할 수 있다는 점이며, 함수가 반환된 뒤 `x`가 가지고 있는 원소를 알기 위해서는 `f`의 코드를 확인해야 한다. 물론, 실제 코딩을 하는 경우에는 대부분 `f`가 인자로 받은 리스트를 수정하는 함수인지 아닌지를 알 수 있으나, 수정하지 않을 것이라고 예상했음에도 사실은 수정이 발생하는 일이 분명히 존재하며, 이는 버그를 일으킨다.

*전역*(global) 변수가 수정 가능하다면, 지역 변수가 수정 가능한 것보다도 더 코드를 읽기 어렵게 만들 수 있다.

```scala
def f(x: Int) = g(x, y)
```

함수 `f`의 결괏값은 전역변수 `y`의 값에 의존하며, `y`가 수정 가능한 변수인 경우, 함수 `f`를 호출할 때 사용한 인자의 값이 동일하더라도 결괏값이 변화할 수 있어 `f`가 어떻게 동작할지 예상하기 어렵게 만든다. 또한, `y`는 전역 변수이기 때문에, 함수 `f`가 정의된 파일이 아닌 다른 파일에 정의된 변수일 수 있고, 심지어는 프로젝트에서 사용하는 외부 라이브러리의 코드에 존재해서 정의된 부분을 소스 코드로는 확인할 수 없을 수도 있으며, 다른 파일에서도 접근하여 읽고 쓰는 것이 가능하므로 `y`의 값이 프로그램 실행에 따라 어떻게 변화할지 정확히 알기 어렵다. 물론, IDE의 도움으로 `y`가 사용되는 부분을 쉽게 찾을 수는 있으나, 그 부분을 모두 읽고 언제 `y`가 어떻게 바뀌는지 확인하는 것은 전혀 다른 문제이다.

지금까지 불변성이 왜 프로그램을 논증하는 것을 도와주는지 간단한 예시와 함께 살펴보았다. 예시가 인위적이고, 작은 프로그램에서는 수정 가능한 데이터로 인해서 코드가 어려워지는 경우가 많지 않다 보니 설명이 잘 와 닿지 않을 수 있으나, 크고 복잡한 프로그램을 작성할수록 불변성이 가지는 이점은 커진다.

### 함수 호출 시에 인자를 복사해둘 필요가 없다.

```scala
val x = ListBuffer(1, 2)
...
f(x)
...
x
```

앞에서 설명했던 내용과 연결되는 이야기로, `ListBuffer`는 수정 가능한 리스트이기 때문에, 함수 `f`를 호출할 때 리스트의 내용물이 바뀌지 않는다는 보장이 없다. 따라서, `x`의 값이 변화하지 않는 것을 반드시 보장해야 한다면, 아래와 같이 `x`를 복사하여 같은 값을 가지고 있는 리스트를 만들어야 한다.

```scala
val x = ListBuffer(1, 2)
val y = x.clone
...
f(y)
...
x
```

`x`가 가지고 있는 원소가 많고 이 코드가 여러 번 사용될 코드라면, 함수 호출 시 항상 `x`를 복사하는 것은 실행 시간을 늘리는 원인이 된다.

또한, 위 코드에서는 `x`가 가지고 있는 원소가 수정 불가능한 값인 `Int` 값뿐이므로 단순히 `clone` 메서드를 사용하는 것만으로도 안전하게 복사할 수 있지만, `x`가 수정 가능한 객체를 원소로 가지고 있다면, `y`가 가지고 있는 객체를 수정하는 행동이 `x`가 가지고 있는 객체에도 반영되므로, 안전하게 *깊은* *복사*(deep copy)를 수행하도록 추가적인 메서드를 정의해야 할 수 있다.

### 여러 스레드에서 안전하게 접근할 수 있다

```scala
val x = ListBuffer(1, 2)
def a() = f(x)
def b() = g(x)
```

함수 `a`와 함수 `b`가 동시에 두 개의 다른 *스레드*(thread)에서 실행된다고 생각해보자. 이 경우 실행 순서를 보장할 수 없기 때문에, 원하지 않는 결과가 나올 수 있다. 심지어는, `ListBuffer`는 여러 스레드에서 접근되는 상황을 고려하지 않도록 설계되었기 때문에, *예외*(exception)가 발생해서 프로그램이 비정상적으로 종료되는 것도 가능하다. 이를 막기 위해서는 각 함수의 *임계* *구역*(critical region)에 `synchronized`와 같은 키워드를 통해 *뮤텍스*(mutex)를 사용하거나 Java의 `CopyOnWriteArrayList`처럼 동시성 프로그래밍을 위해 만들어진 별도의 컬렉션을 사용해야 한다.

반면, `x`가 `ListBuffer`가 아닌 `List`를 사용하여 정의되었다면, 값을 읽을 수는 있지만 값을 쓸 수 없기 때문에, 실행 순서가 결과에 아무런 영향을 주지 못하여, 경쟁 상태에 대한 걱정 없이 두 개의 스레드에서 각각 `a`와 `b`를 실행할 수 있다.

### 안전한 해시값을 만든다

*해시값*(hash value)은 *해시테이블*(hash table)에서 원하는 자료를 찾기 위한 *열쇠*(key)로 사용되기 때문에, 동일한 값을 나타내는 객체의 해시값은 동일해야 한다. 그러나, 수정 가능한 객체의 경우 객체를 수정하면 값도 변하기 때문에 해시값도 바뀌게 된다. 이는 해시테이블을 사용하여 구현된 *집합*(set)이나 *사전*(dictionary) 같은 자료 구조를 사용할 때 문제가 된다.

```scala
val x = ListBuffer(0)
val y = Set(x, ...)
val z = Map(x -> 0, ...)

y.contains(x)
z(x)

x += 1

y.contains(x)
z(x)
```

위 코드를 실행하면 첫 번째 `y.contains(x)`는 정상적으로 `true`, `z(x)`는 `0`를 반환한다. 그러나, `x`에 `1`을 추가하고 나면, `x`는 여전히 각 집합과 사전에 들어있음에도 불구하고 `x`의 해시값이 바뀌었기 때문에, `y.contains(x)`는 `false`를 반환하고 `z(x)`는 `NoSuchElementException`라는 예외를 발생시킨다. (단, Scala에서는 원소가 4개 이하인 `Set`과 `Map`은 최적화를 위하여 해시를 사용하지 않도록 구현되어 있고, 원소가 5개 이상인 경우에 해시를 사용하기 때문에, `y`와 `z`가 원소를 4개 이하로 가지고 있다면 `x`에 `1`을 추가한 다음에도 `true`와 `0`이 나온다.) 이처럼 수정 가능한 객체를 해시테이블의 열쇠로 사용하면 버그를 유발할 수 있으며, 수정 불가능한 객체를 사용함으로써 간단히 해결할 수 있다.

지금까지 불변성이 가지는 장점에 대해 알아보았다. 이런 이유로 함수형 프로그래밍은 불변성을 매우 중요하게 생각하며, 수정 불가능한 변수와 객체를 대부분의 경우에 사용한다. 그러나, 당연하게도, 모든 경우에 불변성이 코딩을 돕는 것은 아니다. 예를 들면, 우리가 알고 있는 *정렬*(sort)을 비롯한 수많은 알고리즘은 *배열*(array)과 같은 수정 가능한 자료 구조를 사용하고 수정 가능한 변수와 함께 `for`이나 `while` 같은 *반복문*(loop)을 사용하여 구현하는 것이, 수정 불가능한 자료 구조와 변수를 사용하는 것보다 훨씬 효율적이고 읽기 쉬운 코드를 만든다. 따라서, 본인이 구현하고자 하는 목표에 따라 그에 적합한 프로그래밍 방식을 선택하는 것이 중요하다.

## 재귀

프로그래밍을 할 때 동일한 계산을 여러 번 반복하는 경우는 흔하다. 반복문을 사용함으로써, 이러한 경우를 간결한 코드로 표현할 수 있다. 그러나, 수정 불가능한 변수와 객체만을 사용한다면, 반복문의 시작 지점으로 돌아오더라도 이전과 비교했을 때 상태가 달라지지 않으므로, 반복문을 돌면서 다른 값에 대해 동일한 계산을 하는 것도, 반복문을 빠져나가는 것도 불가능하다. 따라서, 불변성과 반복문을 함께 사용하는 것은 대체로 무의미하다. 따라서, 함수형 프로그래밍에서 반복 작업을 나타내기 위해서는 반복문이 아닌 다른 방법이 필요하다. 그 방법은 바로 함수가 자기 자신을 다시 호출하는 *재귀*(recursion)이다. 같은 함수에 다른 인자를 전달함으로써 다른 값에 대해 동일한 계산을 하고, 더 할 계산이 없다면 함수를 호출하는 대신 바로 결괏값을 반환하면 된다.

다음은 인자로 주어진 정수의 *계승*(factorial)을 계산하는 `factorial` 함수이다. 문제를 간단하게 만들기 위해서, 음의 정수에 대해서는 1을 결괏값으로 하도록 하겠다. 처음은 명령형으로 반복문을 사용하여 구현한 것이고, 두 번째는 함수형으로 재귀를 통해 구현한 것이다.

```scala
def factorial(n: Int) = {
  var i = 1, res = 1
  while (i <= n) {
    res *= i
    i += 1
  }
  res
}

def factorial(n: Int): Int = if (n <= 0) 1 else n * factorial(n - 1)
```

*재귀* *함수*(recursive function)의 경우 반드시 결과 타입을 명시해주어야 한다.

재귀를 통한 구현은 반복문보다 함수의 수학적 정의를 직접적으로 드러낸다는 특징을 가진다.

\[n!=\begin{cases}1 & \text{if } n=0\\n \times (n-1)! & \text{otherwise}\end{cases}\]

구현이 정의와 완벽히 일치하는 것을 볼 수 있다. 이처럼, 재귀는 단순히 수정 가능한 상태 없이 반복 계산을 할 수 있게 해주는 역할뿐 아니라, 수학적인 대상을 함수로 표현할 때 직관적이고 간결한 표현을 가능하게 한다. 또한, 함수를 올바르게 구현했음을 증명하기에도 *수학적* *귀납법*(mathematical induction)을 적용하기 용이하다. 명령형으로 구현한 경우에도 얼마든지 증명을 할 수는 있으나, 더 큰 노력을 필요로 한다. 꼭 엄밀하게 증명을 하지 않더라도, 재귀를 통한 구현이 직관적으로 코드가 올바르다는 것을 확인하기도 더 좋다. 물론 불변성과 마찬가지로, 알고리즘 구현처럼 재귀보다 반복문이 효율적인 경우도 있으며, 필요에 따라 더 나은 구현 방법을 선택해야 한다.

재귀의 단점은 함수 호출로 인해 발생하는 *비용*(overhead)과 재귀 호출이 여러 차례 이루어졌을 때 발생하는 *스택* *넘침*(stack overflow)이 있다. 함수 호출의 비용은 현재 사용되는 CPU 수준에서 대부분의 경우에 충분히 무시할 수 있지만, 매우 성능이 중요하고 반복되는 코드의 크기가 아주 작다면 반복문을 사용하는 것을 고려할 수 있다. 한편, 반복된 함수 호출로 인하여 스택의 공간이 부족해져 스택 넘침이 발생하면 프로그램이 비정상적으로 종료되므로 원하는 결과를 얻지 못하기 때문에 심각한 문제가 된다. 심지어, 웹 서버처럼 종료 없이 같은 작업을 무한히 반복해야 하는 프로그램은 무조건 언젠가는 스택 넘침이 발생하기 때문에 재귀를 통해서는 구현이 불가능하다. 그렇기 때문에, 함수형 언어들은 *꼬리* *호출* *최적화*(tail call optimization)를 함으로써 스택 넘침을 막는다. 꼬리 호출에 대해서는 이 글의 마지막 부분에서 다시 자세히 다룰 예정이다.

## 리스트

리스트는 무슨 프로그램을 만들든 많이 사용되는 중요한 자료 구조이다. 또, 함수형 언어에서 리스트를 어떻게 정의하고 사용하는지 살펴보는 것은 함수형 프로그래밍을 처음 시작하기에 아주 좋은 예시이다. 지금부터는 리스트와 리스트를 다루는 함수를 직접 구현해 볼 것이다. Scala에 이미 `List`가 존재하지만, 직접 정의를 해보는 것이 의미가 있다고 생각하기 때문에, 이번 글에서는 직접 리스트를 구현해본 뒤, 함수형 프로그래밍에 대한 세 번째 글에서 Scala 표준 라이브러리의 `List`에 대해 살펴볼 것이다.

### 리스트의 정의

리스트는 유한한 개수의 원소를 가지고 있는 자료 구조로 원소들 사이에 순서가 존재하며 동일한 원소가 두 번 이상 나타날 수 있다. 간단히 이야기해서, 유한한 개수의 값들을 나열한 것으로 볼 수 있다. 그러나, 리스트를 함수형으로, 또는 수학적으로 바라보고 다루기 위해서는 보다 재귀적인 정의가 필요하다. 그래야 재귀 함수를 통해서 리스트를 처리할 수 있다. 함수형 언어에서는 리스트를 다음과 같이 정의한다.

어떤 리스트는

* `Nil`: 빈 리스트이거나,
* `Cons`: 어떤 값과 어떤 리스트의 *순서쌍*(pair)이다.

(`Nil`과 `Cons`는 Lisp과 그 변종 언어에서 주로 사용하는 이름이지만, 함수형 리스트를 설명할 때 일반적으로 많이 사용하며, 다른 언어에서도 사용된다.)

빈 리스트가 아닌 모든 리스트는 최소 하나의 원소를 가지고 있고, 따라서 첫 번째 원소와 나머지 원소들을 포함하는 리스트로 나눌 수 있다. 이때, 첫 번째 원소를 *머리*(head), 나머지 원소들의 리스트를 *꼬리*(tail)라고 부른다. 우리는 위 정의가 모든 리스트를 포함하며, 위 정의에 부합하는 대상은 리스트임을 확인할 수 있다. 다음은 `Nil`과 `Cons`를 사용해서 `1`, `2`, `3`을 차례로 가지고 있는 리스트를 표현한 것이다.

```
Cons(1, Cons(2, Cons(3, Nil)))
```

리스트의 정의를 Scala 코드로 표현하면 다음과 같다. 구현을 간단히 하기 위해서, 지금부터는 리스트가 `Int` 값만을 원소로 가진다고 가정하겠다.

```scala
trait List
case object Nil extends List
case class Cons(head: Int, tail: List) extends List
```

이렇게 정의한 타입을 *대수적* *데이터* *타입*(algebraic data type)이라고 부르며, 함수형 언어에서 많이 사용되는 방식이다. 대수적 데이터 타입과 위 코드에서 사용한 `trait`, `case` 같은 키워드에 대해서는 함수형 프로그래밍에 대한 네 번째 글에서 자세히 다룰 예정이기 때문에, 여기서는 간단히만 설명하도록 하겠다.

`trait List`는 `List`라는 타입을 정의한다.

`case object Nil extends List`는 `Nil`이라는 값을 정의하고, `Nil`이 `List` 타입의 값이라고 선언한다. 여기에서 `object` 키워드는 *단독* *객체*(singleton object)를 정의하기 위한 것으로 이 코드는 아래 코드의 *문법* *설탕*(syntactic sugar)인 것처럼 해석할 수 있다.

```scala
class Nil$ extends List
val Nil = new Nil$
```

단독 객체는 어떤 클래스의 객체가 프로그램 전체에서 유일하게 존재한다는 뜻이다. 빈 리스트는 모두 동일한 단 하나의 빈 리스트이기 때문에, 굳이 `Nil` 클래스를 정의한 뒤 빈 리스트를 필요로 할 때마다 매번 *생성자*(constructor)를 호출하여 `Nil` 클래스의 객체를 만들어서 사용하는 것보다는, 앞서 한 것처럼 프로그램 전체에서 유일한 `Nil`이라는 객체를 만들어 빈 리스트가 필요할 때 그 객체를 사용하는 것이 효율적이다. `Nil`이란 값은 수정 불가능하기 때문에, 모든 빈 리스트가 한 객체를 가리키고 있더라도 문제가 되지 않는다

`case class Cons(head: Int, tail: List) extends List`는 `Cons`라는 클래스를 정의하고, `Cons` 클래스의 객체는 `List` 타입이라고 선언한다. `Cons` 객체는 두 개의 필드 `head`와 `tail`을 가지며, 각각의 타입은 `Int`와 `List`이다.

이제 코드상으로 다음과 같이 `List` 값들을 만들 수 있다.

```Scala
Nil
Cons(1, Nil)
Cons(1, Cons(2, Nil))
```

코드에서도 드러나듯, 함수형 리스트는 자료 구조 시각에서 보면 *단일* *연결* *리스트*(singly linked list)이다. 리스트의 첫 원소부터 차례대로 다음 원소에 접근할 수 있으며, 다음 원소로 가는 것은 가능하나 이전 원소로 가는 것은 불가능하다.

### 리스트를 다루는 함수

어떤 임의의 리스트가 존재할 때, 우리는 그 리스트가 빈 리스트인지 아닌지 알 수 없다. 따라서, Nil인 경우와 Cons인 경우로 나누어 처리할 방법이 필요하다. 이는 *패턴* *대조*(pattern matching)를 통해서 할 수 있다. 패턴 대조에 대해서도 함수형 프로그래밍에 대한 네 번째 글에서 자세히 다룰 것이기 때문에 이 글에서는 패턴 대조 자체에 대한 설명보다는 리스트를 패턴 대조하는 방법만 보여줄 것이다.

```scala
def f(l: List) = l match {
  case Nil => "The empty list"
  case Cons(h, t) => "A pair of an integer and a list"
}
```

패턴 대조는 `[식] match { case [패턴] => [식] ... }` 형태로 이루어지며, 처음에 등장하는 식을 계산한 결과를 위쪽에 나온 패턴부터 순서대로 대조하여 일치하는 패턴에 대응하는 식을 계산한다. 위 코드에서는 만약 `l`이 `Nil`이라면 함수의 결괏값은 `"The empty list"`가 된다. `Nil`이 아닌 경우, `l`은 무조건 `Cons` 객체이므로, 두 번째 패턴과 일치하여 함수의 결괏값은 `"A pair of an integer and a list"`가 된다. 이때, `h`와 `t`라는 변수가 각각 주어진 리스트의 `head`와 `tail`에 해당하는 값을 가리키게 되며, `h`와 `t`는 이 예시에서 드러나지는 않았지만, 두 번째 패턴에 대응하는 식을 작성할 때 사용할 수 있다.

아래의 `inc1`은 리스트 한 개를 인자로 받아, 인자로 받은 리스트보다 원소가 1씩 큰 리스트를 결과로 내는 함수이다.

```scala
def inc1(l: List): List = l match {
  case Nil => Nil
  case Cons(h, t) => Cons(h + 1, inc1(t))
}
```

주어진 리스트가 빈 리스트인 경우에는 당연히 결괏값도 빈 리스트이다. 주어진 리스트가 어떤 정수와 리스트의 순서쌍이라면, 해당 정수를 1 증가시킨 값을 머리로 하고 꼬리의 모든 원소를 1 증가시킨 리스트를 꼬리로 하는 리스트가 결괏값이다.

이제 리스트 한 개를 인자로 받아서, 인자로 받은 리스트의 원소를 제곱한 값을 원소로 가지는 리스트를 결과로 하는 함수 `square`를 정의해 보자. 위에서 정의한 `inc1`과 유사하므로, 스스로 생각해본 뒤 아래의 코드를 확인해보면 좋을 것이다.

<details><summary>`square` 코드 보기</summary>
```scala
def square(l: List): List = l match {
  case Nil => Nil
  case Cons(h, t) => Cons(h * h, square(t))
}
```
</details>

이번에는 리스트 한 개를 인자로 받아, 리스트의 원소 중 홀수만 포함하는 리스트를 결과로 하는 함수 `odd`를 정의할 것이다.

```scala
def odd(l: List): List = l match {
  case Nil => Nil
  case Cons(h, t) =>
    if (h % 2 != 0) Cons(h, odd(t))
    else odd(t)
}
```

인자가 빈 리스트인 경우, 홀수가 존재하지 않으므로 빈 리스트를 반환한다. 빈 리스트가 아니라면, 머리가 홀수인지 확인하여, 머리가 홀수이면 머리를 포함하고 꼬리에서 홀수만 남긴 리스트가 결과이고, 짝수라면 머리를 제외하고 꼬리에서 홀수만 남긴다.

다음으로는 인자로 받은 리스트에서 양의 정수만 원소로 남긴 리스트를 반환하는 함수 `positive`를 정의해 보자. `square`와 마찬가지로 스스로 생각해본 뒤 아래의 코드를 확인하면 좋을 것이다.

<details><summary>`positive` 코드 보기</summary>
```scala
def positive(l: List): List = l match {
  case Nil => Nil
  case Cons(h, t) =>
    if (h > 0) Cons(h, positive(t))
    else positive(t)
}
```
</details>

리스트의 길이를 구하는 함수 `length`는 다음과 같이 정의할 수 있다.

```scala
def length(l: List): Int = l match {
  case Nil => 0
  case Cons(h, t) => 1 + length(t)
}
```

빈 리스트의 길이는 0이고 비어있지 않은 리스트의 길이는 꼬리의 길이보다 1 크다.

위 코드를 바탕으로 리스트가 가진 원소들의 합과 곱을 구하는 `sum`, `product` 함수를 만들어 보자. 빈 리스트인 경우, 합은 0, 곱은 1이라 생각하자.

<details><summary>`sum` 코드 보기</summary>
```scala
def sum(l: List): Int = l match {
  case Nil => 0
  case Cons(h, t) => h + sum(t)
}
```
</details>

<details><summary>`product` 코드 보기</summary>
```scala
def product(l: List): Int = l match {
  case Nil => 1
  case Cons(h, t) => h * product(t)
}
```
</details>

리스트 하나와 정수 하나를 인자로 받아서 리스트의 제일 뒤에 해당 정수를 추가한 리스트를 만드는 함수 `addBack`도 비슷한 방법으로 정의할 수 있다.

<details><summary>`addBack` 코드 보기</summary>
```scala
def addBack(l: List, n: Int): List = l match {
  case Nil => Cons(n, Nil)
  case Cons(h, t) => Cons(h, addBack(t, n))
}
```
</details>

`addBack`을 제대로 구현하였다면 알 수 있듯이, 리스트의 뒤에 원소를 추가하기 위해서는 \(O(n)\)의 *시간* *복잡도*(time complexity)가 필요하다. 게다가, 전체 리스트를 새로 만들어야 하므로 메모리도 추가적으로 \(O(n)\)을 사용한다. 반대로, 리스트의 앞에 원소를 추가하는 것은 \(O(1)\) 시간에 가능하며, 기존 리스트를 그대로 참조하는 `Cons` 객체 하나만 만들면 되므로 \(O(1)\)의 추가 공간만을 필요로 한다. 따라서, 함수형 리스트를 사용할 때는 리스트의 뒤보다는 앞에 원소를 붙여나가는 것이 바람직하다.

## 꼬리 호출 최적화

어떤 함수가 마지막으로 하는 계산이 어떤 함수(자기 자신일 수도 있고 다른 함수일 수도 있다)를 호출하는 것인 경우를 꼬리 호출이라고 부른다. 꼬리 호출을 하는 경우, 호출 이후에는 모든 계산이 *불린* *함수*(callee)에서 이루어지므로, *부른* *함수*(caller)의 지역 변수는 더는 필요하지 않다. 따라서, 부른 함수의 *스택* *틀*(stack frame)을 계속 유지하고 있을 필요가 없다. 많은 함수형 언어에서는 컴파일 과정에서 함수가 꼬리 호출을 하는지 확인한 후, 꼬리 호출이라면 부른 함수의 스택 틀을 유지하는 일반적인 함수 호출 대신 부른 함수의 스택 틀을 없애고 불린 함수의 스택 틀을 만드는 최적화된 코드를 만들어낸다. 따라서, 모든 호출이 꼬리 호출로 이루어진다면, 함수 호출을 몇 번을 중첩하든 스택이 자라지 않기 때문에, 스택 넘침이 발생하지 않는다.

앞에서 정의한 `factorial` 함수를 꼬리 호출을 사용하는 형태로 바꾸어 보면서 꼬리 호출에 대해 조금 더 쉽게 이해해보자. 앞의 설명이 잘 와 닿지 않는다면, 지금부터의 예시를 잘 살펴보고 앞으로 돌아가서 다시 설명을 읽어보면 좋을 것이다.

```scala
def factorial(n: Int): Int = if (n <= 0) 1 else n * factorial(n - 1)
```

위에서 정의한 형태는 `factorial` 함수를 호출한 뒤 그 결과에 `n`을 곱하는 계산을 해야 하므로, 함수 호출이 마지막으로 하는 계산이 아니므로 꼬리 호출이 아니다. 그 말은 `factorial(n - 1)`이 계산되는 동안 나중에 사용될 `n`이 남아있어야 하므로 스택 틀을 없앨 수 없다는 뜻이다. 따라서 `factorial(3)`을 계산하는 과정은 다음과 같이 볼 수 있다.

* `factorial(3)`
* `3 * factorial(2)`
* `3 * (2 * factorial(1))`
* `3 * (2 * (1 * factorial(0)))`
* `3 * (2 * (1 * 1))`
* `3 * (2 * 1)`
* `3 * 2`
* `6`

`*`로 구분된 부분마다 서로 다른 함수의 지역 변수가 가리키는 값이므로, 최대 네 개의 함수를 위한 스택 틀이 동시에 존재함을 알 수 있다. 제일 처음에 주어진 인자의 값이 크다면 계속해서 스택이 자라나 결국은 스택 넘침이 일어날 것이다. 이는 REPL에서 확인해볼 수 있다.

```scala
scala> def factorial(n: Int): Int = if (n <= 0) 1 else n * factorial(n - 1)
factorial: (n: Int)Int

scala> factorial(10000)
java.lang.StackOverflowError
  at .factorial(<console>:12)
```

이 함수를 꼬리 호출 형태로 바꾸려면 어떻게 해야 할까? `factorial(n - 1)` 함수 호출이 끝나서 결과를 얻은 뒤 거기에 `n`을 곱하는 대신, `n`도 함께 인자로 넘겨서 불린 함수 쪽에서 계산이 끝난 뒤 인자로 받은 `n`을 곱해서 결과로 내게 만들면 된다. 중간 계산 결과를 계속 인자를 통해서 전달하는 방식이라고 생각할 수도 있다. 간단히 표현해보면 다음과 같다.

* `factorial(3)`
* `factorial(2, 중간 결과 = 3)`
* `factorial(1, 중간 결과 = 3 * 2)`
* `factorial(1, 중간 결과 = 6)`
* `factorial(0, 중간 결과 = 6 * 1)`
* `factorial(0, 중간 결과 = 6)`
* `6`

부른 함수로 돌아갈 필요가 없다는 것을 확인할 수 있다. 프로그램의 실행이 마치 반복문을 실행하는 것과 같아졌다. 코드로는 아래와 같이 구현할 수 있다. 중간 결과를 전달받기 위한 추가적인 매개변수가 필요하다. 이제 `factorial(n, i)`는 \(n!\times i\)를 계산한다.

```scala
def factorial(n: Int, inter: Int): Int =
  if (n <= 0) inter else factorial(n - 1, inter * n)
```

코드상으로도 꼬리 호출임을 확인할 수 있다. 조금 더 정확히는, 꼬리 호출의 특수한 경우인 꼬리 재귀(tail recursion)로, 함수의 마지막 계산이 언제나 자기 자신을 호출하는 것이다. 대부분 함수형 언어에서는 꼬리 재귀를 포함하는 모든 꼬리 호출을 최적화하므로 꼬리 재귀를 구분해서 생각할 필요가 없으나, Scala에서는 꼬리 호출이 아닌 꼬리 재귀만을 최적화하므로 꼬리 재귀가 중요하다.

Scala 코드가 컴파일되어 만들어진 Java 바이트코드가 실행되는 자바 가상 기계에서는 함수에서 반환하거나 다른 함수를 호출하는 것만을 허용하고, 스택 틀을 없애면서 다른 함수의 시작 부분으로 건너뛰는 것은 허용하지 않는다. 그렇기에 Scala 컴파일러는 다른 함수형 언어가 하는 것처럼 꼬리 호출 최적화를 할 수 없다. 그래서 Scala 컴파일러는 일반적인 꼬리 호출은 최적화를 하지 못하고, 꼬리 재귀만 코드를 반복문 형태로 변환하는 방식으로 최적화한다. 실제로, 위 코드를 컴파일한 뒤 `javap`를 사용해서 바이트코드를 확인해보면 다음과 같이 `factorial` 함수 내부에서 함수 호출 없이 함수 내부로의 *건너뜀*(jump)만을 사용하는 것을 볼 수 있다.

```java
public int factorial(int, int);
  Code:
     0: iload_1
     1: iconst_0
     2: if_icmpgt     9
     5: iload_2
     6: goto          20
     9: iload_1
    10: iconst_1
    11: isub
    12: iload_2
    13: iload_1
    14: imul
    15: istore_2
    16: istore_1
    17: goto          0
    20: ireturn
```

REPL에서 실행을 해보면 스택 넘침이 발생하지 않는 것도 확인할 수 있다.

```scala
scala> def factorial(n: Int, inter: Int): Int =
     |   if (n <= 0) inter else factorial(n - 1, inter * n)
factorial: (n: Int, inter: Int)Int

scala> factorial(10000, 1)
res0: Int = 0
```

(스택 넘침과는 별개로 *정수* *넘침*(integer overflow)으로 인해서 정상적인 결과가 나오지는 않는데, `Int`대신 `BigInt` 타입을 사용하면 해결할 수 있다.)

이렇게 반복문 형태로 만드는 최적화는 스택 넘침을 막는 것뿐 아니라 함수 호출의 비용도 없애준다는 장점이 있으나, 일반적인 꼬리 호출을 처리하지 못하므로, 다른 함수형 언어에서는 꼬리 호출 최적화를 할 아래와 같은 *상호* *재귀* *함수*(mutually recursive functions)를 최적화하지 못한다는 단점이 있다.

```scala
def even(n: Int): Boolean = if (n <= 0) true else odd(n - 1)
def odd(n: Int): Boolean = if (n == 1) true else even(n - 1)
```

함수가 복잡하다면 꼬리 재귀로 구현하려 했으나, 실수로 그렇게 하지 못할 수 있는데, Scala에서는 *주해*(annotation)를 통해서 어떤 함수가 꼬리 재귀인지를 검사해주도록 컴파일러에 요청할 수 있다.

```scala
import scala.annotation.tailrec
@tailrec def factorial(n: Int, inter: Int): Int =
  if (n <= 0) inter else factorial(n - 1, inter * n)
```

만약 `tailrec` 주해가 붙은 함수가 꼬리 재귀가 아니라면 컴파일 오류가 발생한다. `tailrec` 주해가 있든 없든 상관없이, 정상적으로 꼬리 재귀로 정의된 함수는 언제나 컴파일러가 반복문 형태로 최적화를 진행하므로 사용하지 않아도 문제는 없지만, 실수를 방지하기 위해서 꼬리 재귀를 의도한 함수에는 `tailrec` 주해를 달아주는 것이 좋다.

위와 같이 `factorial`의 구현을 수정하면 이제 스택 넘침이 발생하지는 않지만, 함수를 호출할 때 불필요하게 두 번째 인자로 `1`을 넘겨야 한다. 이 문제점은 아래와 같이 꼬리 재귀로 구현한 함수를 `factorial` 함수 내부의 지역 함수로 정의하고 `factorial`에서 해당 지역 함수를 호출하도록 고쳐서 해결할 수 있다.

```scala
def factorial(n: Int): Int = {
  @tailrec def aux(n: Int, inter: Int): Int =
    if (n <= 0) inter else aux(n - 1, inter * n)
  aux(n, 1)
}
```

이제 앞에서 구현한 리스트를 다루는 함수를 꼬리 재귀 형태로 바꾸어보자. 리스트의 길이를 구하는 함수 `length`는 다음과 같이 바꿀 수 있다.

```scala
def length(l: List): Int = {
  @tailrec def aux(l: List, inter: Int): Int = l match {
    case Nil => inter
    case Cons(h, t) => aux(t, inter + 1)
  }
  aux(l, 0)
}
```

마찬가지로 `sum`과 `product`도 바꿀 수 있으므로 스스로 해보자.

<details><summary>`sum` 코드 보기</summary>
```scala
def sum(l: List): Int = {
  @tailrec def aux(l: List, inter: Int): Int = l match {
    case Nil => inter
    case Cons(h, t) => aux(t, inter + h)
  }
  aux(l, 0)
}
```
</details>

<details><summary>`product` 코드 보기</summary>
```scala
def product(l: List): Int = l match {
  @tailrec def aux(l: List, inter: Int): Int = l match {
    case Nil => inter
    case Cons(h, t) => aux(t, inter * h)
  }
  aux(l, 1)
}
```
</details>

`inc1`은 아래와 같이 바꿀 수 있다.

```scala
def inc1(l: List): List = {
  @tailrec def aux(l: List, inter: List): List = l match {
    case Nil => inter
    case Cons(h, t) => aux(t, addBack(inter, h + 1))
  }
  aux(l, Nil)
}
```

위 코드는 올바르게 작동하지만 한 가지 문제가 있다. 새로운 중간 결과를 만들기 위해서는 계속해서 원소를 기존의 중간 결과 리스트의 뒤에 추가해야 하므로 기존에 시간 복잡도 \(O(n)\)에 구현한 함수를 \(O(n^2)\)에 구현하고 있다는 것이다. 이는 결괏값이 리스트인 다른 함수들도 마찬가지다.

꼬리 재귀로 구현하면서 시간 복잡도도 \(O(n)\)을 유지하고 싶다면 어떻게 해야 할까? 스스로 생각해보자. 답은 아래에 있다.

<details><summary>`inc1` 코드 보기</summary>
```scala
def reverse(l: List): List = {
  @tailrec def aux(l: List, inter: List): List = l match {
    case Nil => inter
    case Cons(h, t) => aux(t, Cons(h, inter))
  }
  aux(l, Nil)
}

def inc1(l: List): List = {
  @tailrec def aux(l: List, inter: List): List = l match {
    case Nil => inter
    case Cons(h, t) => aux(t, Cons(h + 1, inter))
  }
  reverse(aux(l, Nil))
}
```

이 코드는 \(O(n)\)의 시간 복잡도를 가지나 여전히 처음 작성한 함수보다 비효율적이기는 하다.
</details>

## 감사의 말

글을 확인하고 의견을 주신 류석영 교수님, Scala 세미나를 준비할 때 의견 주신 모든 분과 Scala 세미나에 참석하신 모든 분께 감사드립니다.
