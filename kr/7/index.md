## 프로그래밍 언어의 정의

프로그래밍 언어 수업에서는 여러 언어를 정의한다. 프로그래밍 언어를 정의하는 것은 그 언어의 *문법*(syntax)과 *의미*(semantic)를 정의하는 것이다. 이번 글에서는 언어의 문법이 무엇인지에 관해 다룬다. 문법에 대하여 알아보기 전에, 언어를 정의하는 것이 왜 중요한지 짚고 넘어가려고 한다.

앞으로 프로그래밍 언어 수업에서 정의할 모든 언어는 사람들이 실제로 사용하지 않는 작은 언어이다. 예를 들면, 사용자의 입력을 받을 수도 없고, 사용자에게 결과를 출력할 수도 없다. 또, 문자열이나 *부동소수점* *수*(floating point number)같이 흔히 사용되는 타입도 존재하지 않는다. 실제로 사용할 수도 없는 언어를 정의하는 것이 무의미해 보일 수 있다. 학부 수업이니까 내용을 쉽게 만들기 위해서 그런 것일까 싶을 수도 있지만, 프로그래밍 언어 수업뿐 아니라 많은 프로그래밍 언어 연구가 실제로 사용되지 않는 작은 언어를 대상으로 한다.

프로그래밍 언어 연구의 흔한 목표는 어떤 언어가 특정 성질을 만족한다는 것을 보이는 것이다. 어떤 언어는 이미 존재하는 사람들이 많이 사용하는 언어일 수도 있고, 아직 구현하지는 않았지만 이미 존재하는 언어를 개선하기 위해서 기능 추가를 제안한 것일 수도 있다. 여기서 언어의 성질이라는 것은 넓은 의미의 단어로, *타입* *안전성*(type soundness) 같은 언어에 내재한 성질부터, "이 언어의 코드에 어떤 알고리즘을 적용하면 특정 결과가 나온다" 같은 성질까지 모두 의미한다.

연구 과정에서 작은 언어를 정의하는 이유는 실제 사람들이 사용하는 언어가 연구 대상으로 삼기에 크기 때문이다. 많이 사용되는 언어는 프로그래머의 편의를 위해서 문법적 설탕 같은 기능을 가지고 있으며, 이런 기능을 모두 포함해서 어떤 성질을 증명하는 것은 시간이 많이 필요하다. 모든 기능이 보이고자 하는 성질과 직접적인 관련이 있다면 시간이 오래 걸리더라도 모두 포함해서 증명해야겠지만, 대부분의 기능은 연구자가 관심 있는 성질과는 관련이 없다. 따라서, 성질에 직접적으로 연관된 기능만을 찾은 다음, 그 기능을 잘 나타내는 작은 언어를 만들어 연구를 진행하는 것이 효율적이다. 

실존하는 특정 언어에 대한 연구는 비용이 많이 드는 것에 더해, 그 연구를 다른 언어에 적용하기 어렵다는 문제가 있다. 어떤 언어의 모든 기능을 반영하여 성질을 증명했다면, 그 기능 중 일부만 가지고 있는 다른 언어도 그 성질을 만족하는지 알 수 없다. 반면, 관심 있는 성질에 영향을 주는 기능만을 가진 작은 언어를 정의하여 연구를 수행하였다면, 그 기능을 가진 다른 모든 언어에도 큰 비용 없이 연구 결과를 적용할 수 있다.

구체적인 예시로 Scala에 관한 연구를 이야기해 보겠다. Scala의 특징은 객체가 *타입* *멤버*(type member)를 가진다는 것이다. (타입 멤버가 무엇인지 몰라도 된다.) Scala 이전에 널리 사용된 언어에서는 객체가 타입 멤버를 가지지 않았기에, 과거에는 객체가 타입 멤버를 가지는 타입 체계가 타입 안전한지 알 수 없었다. (타입 안전성이 무엇인지 몰라도 된다.) 연구자들은 Scala의 타입 안전성을 보이기 위해서, 객체가 타입 멤버를 가지는 작은 언어인 DOT(Dependent Object Type; ‘닷’이라고 읽는다)을 정의한 뒤, DOT의 타입 안전성을 증명하였다. 만약 완전한 Scala의 타입 안전성을 증명하려 하였다면, 현재의 기술로는 수십 년이 걸릴 수도 있으며, 연구자들의 관심사인 타입 멤버를 가진 객체와는 상관없는 기능을 위해 불필요한 노력을 기울였을 것이다. 그러나, DOT의 타입 안전성을 증명함으로써, 시간을 많이 들이지도 않았고, 같은 기능을 제공하는 Wyvern 같은 다른 언어에도 연구 결과를 적용할 수 있다. 물론, 아무리 특징을 잘 나타낸다고 해도, DOT의 타입 안전성이 Scala의 타입 안전성을 의미하는 것은 아니다. 그렇지만, 언어의 핵심 부분의 타입 안전성이 증명된 것과 그렇지 않은 것은 Scala를 믿고 사용하고 싶은 사용자들에게 큰 차이이다. 또한, 타입 멤버를 가지는 객체 이외의 기능은 이미 다른 연구에서 검증되었기에, DOT을 검증하는 것만으로도 Scala의 타입 안전성에 대해 큰 확신이 생긴다.

지금까지 한 이야기를 요약하여, 많은 프로그래밍 언어 연구의 흐름을 아래와 같이 정리할 수 있다.

1. 언어 A의 기능 B가 성질 C를 만족함을 증명하고 싶다.
2. B를 잘 나타내는 작은 언어 a를 정의한다.
3. C를 언어 a의 정의에 맞게 성질 c로 정의한다.
4. a가 c를 만족함을 증명한다.
5. A가 C를 만족한다는 강력한 이론적 배경이 생기며, 언어 D도 기능 B를 제공할 때, D가 C를 만족할 것이라 기대할 수 있다.

물론, 이런 흐름을 따르지 않는 프로그래밍 언어 연구도 많으며, 프로그래밍 언어 분야는 탁상공론이 아니다. 실제 언어, 실제 프로그램, 실제 시스템을 만들고 증명하고 검증하는 연구, 실제 프로젝트에서 사용 가능한 도구 제작으로 이어지는 연구 모두 프로그래밍 언어 연구자들이 하는 일이다. 대표적으로, Facebook에서 개발한 정적 분석기인 Infer는 Faceook, Amazon을 포함한 많은 회사에서 사용하고 있으며, 프로그래밍 언어 전공자들에 의해 개발되고 있다.

그러나, 그런 실용적인 연구가 있으려면 핵심적인 성질이나 알고리즘에 대한 엄밀한 이론적 뒷받침이 존재해야 하며, 작은 언어에 대한 연구가 그 역할을 한다. Infer 역시 몇 논문에 [이론적 배경](https://fbinfer.com/docs/separation-logic-and-bi-abduction.html)을 두고 있으며, 그 논문들은 실제 언어를 직접 다루지 않는다. 오히려, 작지만 일반적인 연구이기에, Infer는 한 언어에 대한 정적 분석기가 아닌 Java, C, C++, Objective-C에 모두 사용 가능한 정적 분석기로 만들어졌다.

프로그래밍 언어 연구에서 가장 중요한 것은 관심 있는 문제를 잘 표현하는 작고 엄밀한 문제를 정의하고 그 문제를 푸는 것이다. 프로그래밍 언어 수업도 같다. 프로그래밍 언어 수업에서는 거의 모든 언어가 제공하는 핵심적인 기능을 다루며, 그런 기능을 잘 나타내는 작은 언어를 엄밀하게 정의한다. 이는 프로그래밍 언어 연구의 기초이다. 동시에, 프로그래밍 언어에 관심이 없는 학생에게도 새로운 언어를 쉽게 이해하고 사용할 수 있게 하는 밑거름이 된다.

## 문법

어떤 언어의 문법은 무엇이 그 언어의 코드에 해당하는지를 규정한다.

```scala
class A
```

```scala
class A {
```

첫 코드는 Scala 코드이다. 그러나, 두 번째 코드는 Scala 코드가 아니다. 이는 Scala의 문법에 따라 결정된 것이다.

조금 더 수학적으로 표현하면, 작성 가능한 모든 코드의 집합이 존재할 때, 언어 A의 코드의 집합은 모든 코드의 집합의 부분 집합이다. 이 부분 집합을 결정하는 것이 언어 A의 문법이다.

문법은 *구체적* *문법*(concrete syntax)과 *요약* *문법*(abstract syntax)으로 나눌 수 있다. 구체적 문법과 요약 문법의 엄밀한 수학적 정의가 존재하는 것은 아니지만, 분명히 서로 다른 성질을 가지고 있기 때문에, 둘을 구분하는 것은 어렵지 않다. 프로그래밍 언어 강의에서는 둘을 간단하게 설명할 때, 구체적 문법은 사람을 위한 문법이고 요약 문법은 컴퓨터를 위한 문법이라고 이야기한다. 엄밀한 표현은 아니지만, 두 문법이 각각 무엇인지 보여주는 직관적인 설명이다.

### 구체적 문법

구체적 문법은 사람을 위한 문법이라고 표현했듯이, 구체적 문법은 실제로 사람이 작성한 코드 중  무엇이 어떤 언어의 코드인지 결정한다. 따라서, 문자열에 대한 규칙이다. 문자열에 대한 규칙이기에, 띄어쓰기나 줄 바꿈을 포함한 모든 문자가 중요하다. 구체적 문법은 "문자열은 큰따옴표로 시작하고 끝난다", "주석은 연속된 *역슬래시*(backslash) 두 개로 시작한다", "모든 연산자는 중위 연산자이다" 등 실제 코드를 작성하기 위한 규칙을 정한다. 대부분의 프로그래밍 언어 *명세*(specification)는 언어의 문법을 구체적 문법으로 기술한다. 사용자들이 명세에 따라 코드를 작성해야 하므로 당연한 선택이다.

문법을 정의할 때 가장 널리 사용되는 방법은 *Backus-Naur* *형식*(Backus-Naur form; BNF)이다. BNF는 여러 개의 규칙으로 구성되며, 각 규칙은 `<이름> ::= 식 | 식 ...` 형태이다. *홑화살괄호*(angle bracket)로 둘러싸인 이름은 *메타변수*(metavariable)로, 문자열의 집합을 나타낸다. 식은 메타변수와 문자열의 나열이며, 우변의 식 중 하나를 선택하여, 그 식의 모든 메타변수를 해당 메타변수의 원소로 치환하여 얻은 문자열이 좌변의 메타변수가 나타내는 집합의 원소이다. 문자열은 큰따옴표로 감싸서 표기한다.

이 글에서는 산술식을 나타내는 언어인 AE의 문법을 정의한다. AE의 식은 정수이거나, 두 식의 합이거나, 두 식의 차이다. 아래는 BNF를 사용하여 정의한 AE의 구체적 문법이다.

\[
\begin{array}{rcl}
\langle digit \rangle & ::= & "0"\ |\ "1"\ |\ "2"\ |\ "3"\ |\ "4"\\
& | &  "5"\ |\ "6"\ |\ "7"\ |\ "8"\ |\ "9" \\
\langle nat \rangle & ::= & \langle digit \rangle\ |\ \langle digit \rangle \ \langle nat \rangle \\
\langle num \rangle & ::= & \langle nat \rangle\ |\ "-"\ \langle nat \rangle \\
\langle AE \rangle & ::= & \langle num \rangle \\
& | & "\{+"\ \langle AE \rangle\ "\ {"}\ \langle AE \rangle \ "\}" \\
& | & "\{-"\ \langle AE \rangle\ "\ {"}\ \langle AE \rangle \ "\}"
\end{array}
\]

BNF를 사용하여 정의한 문법은 아래처럼 해석할 수 있다. \(\langle digit \rangle\)이 나타내는 집합은 \(Digit\), \(\langle nat \rangle\)이 나타내는 집합은 \(Nat\), \(\langle num \rangle\)이 나타내는 집합은 \(Num\), \(\langle AE \rangle\)가 나타내는 집합은 \(AE\)라고 표기하겠다.

\(Digit\)은 \(\{"0","1","2","3","4","5","6","7","8","9"\}\)이다. 십진수의 숫자를 나타낸다.

\(Nat\)은 다음 조건 1과 2를 모두 만족하는 집합 중 가장 작은 집합으로, 자연수를 나타낸다. \("0"\), \("42"\), \("00100"\) 등을 원소로 가진다.

1. \(\forall d\in Digit.d\in Nat\)
2. \(\forall d\in Digit.\forall n\in Nat.d+n\in Nat\)

(\(+\) 연산자는 문자열 *접합*(concatenation)을 나타낸다.)

\(Num\)은 다음 조건 1과 2를 모두 만족하는 집합 중 가장 작은 집합으로, 정수를 나타낸다. \("-0"\), \("1"\), \("-42"\) 등을 원소로 가진다.

1. \(\forall n\in Nat.n\in Num\)
2. \(\forall n\in Nat."-"+n\in Num\)

\(AE\)는 다음 조건 1, 2, 3을 모두 만족하는 집합 중 가장 작은 집합으로, 산술식을 나타낸다.

1. \(\forall n\in Num.n\in AE\)
2. \(\forall e_1\in AE.\forall e_2\in AE."\{+"+e_1+"\ {"}+e_2+"\}"\in AE\)
3. \(\forall e_1\in AE.\forall e_2\in AE."\{-"+e_1+"\ {"}+e_2+"\}"\in AE\)

\("\{+1\ 2\}"\)는 \(AE\)의 원소이다. 그러나, \("\{+\ 1\ 2\}"\)는 \("+"\)와 \("1"\) 사이에 공백이 있으므로 \(AE\)의 원소가 아니다.

### 요약 문법

대부분의 프로그래밍 언어 연구는 과하게 구체적인 구체적 문법 대신 요약 문법을 사용한다. 이전 절에서 보았듯이 구체적 문법은 대개의 연구에서 상관하지 않는 사소한 것들을 신경 쓴다.

요약 문법은 코드를 표현하는 추상적인 자료 구조이다. 구체적 문법이 실제 문자열을 다루는 것과 달리, 요약 문법은 추상적인 대상을 다룬다. 물론, 문자열은 사람이 어떤 정보를 표현하고 전달하기 위해 가장 많이 사용하는 수단이므로, 요약 문법 역시 문자열로 표현될 때가 많다. 그러나, 요약 문법의 본질은 문자열이 아니라는 것이다. 1이라는 값이 “1”이나 “일” 등의 문자열로 표현된다고 해서 그 값의 본질이 문자열에 있지는 않다. 1은 0 다음의 자연수라는 것이 본질이다. 마찬가지로, 요약 문법을 문자열로 기술하더라도 이는 표현 방법일 뿐이며, 요약 문법이 다루는 대상은 문자열이 아니다.

아래는 BNF로 정의한 AE의 요약 문법이다. \(n\)은 정수를 나타내는 메타변수, \(e\)는 식을 나타내는 메타변수이다.

\[
\begin{array}{rcl}
n & \in & \mathbb{Z} \\
e & ::= & n \\
& | & \{+\ e\ e\} \\
& | & \{-\ e\ e\} \\
\end{array}
\]

구체적 문법과 마찬가지로 BNF를 집합으로 해석할 수 있다. \(e\)가 나타내는 집합을 \(\mathcal{A}\)이라고 하자. \(\mathcal{A}\)는 1, 2, 3을 모두 만족하는 가장 작은 집합이다.

1. \(\forall n\in\mathbb{Z}.n\in \mathcal{A}\)
2. \(\forall e_1\in\mathcal{A}.\forall e_2\in\mathcal{A}.\{+\ e_1 \ e_2\}\in\mathcal{A}\)
3. \(\forall e_1\in\mathcal{A}.\forall e_2\in\mathcal{A}.\{-\ e_1 \ e_2\}\in\mathcal{A}\)

BNF 대신 *추론* *규칙*(inference rule) 형태로 문법을 정의할 수도 있다. 추론 규칙은 언어의 의미를 정의하기 위해서 주로 사용되지만, 미리 익숙해질 수 있도록 요약 문법을 추론 규칙으로 정의해 보겠다. 구체적 문법도 추론 규칙으로 정의할 수 있지만, 불필요한 반복이라 생각되기 때문에, 요약 문법만 정의하려 한다.

추론 규칙을 통해서 명제로부터 다른 명제를 유도할 수 있다. 한 추론 규칙은 가로선 위에 있는 여러 개의 *명제*(proposition)와 가로선 밑에 있는 한 명제로 구성된다. 가로선 위에는 명제가 없거나 한 개만 있을 수도 있으며, 명제가 없으면 가로선을 생략해도 된다. 가로선 위의 명제는 가정이고 가로선 아래의 명제는 결론이다. 모든 명제는 메타변수를 가질 수 있다. 추론 규칙의 대표적인 예시로는 *전건* *긍정*(modus ponens)이 있다. 전건 긍정은 임의의 명제 \(P\), \(Q\)에 대해서 \(P\rightarrow Q\)이고 \(P\)이면 \(Q\)라는 규칙이다. 전건 긍정은 아래처럼 표기할 수 있다. \(p\), \(q\)는 명제에 대한 메타변수이다.

\[
\frac
{ p\rightarrow q \quad p }
{ q }
\]

추론 규칙에 존재하는 모든 메타변수를 메타변수가 의미하는 집합의 원소로 치환했을 때, 가정이 모두 참이라면 결론도 참이다. 만약 어떤 명제 \(P\)와 \(Q\)가 존재하고 \(Q\rightarrow P\)와 \(Q\)가 참이라는 사실을 알고 있다면, 전건 긍정 규칙에서 \(p\)를 \(Q\)로, \(q\)를 \(P\)로 치환했을 때 가정이 모두 참이므로, \(P\)를 증명할 수 있다. 증명은 아래와 같이 *증명* *나무*(proof tree) 형태로 표기할 수 있다.

\[
\frac
{ Q\rightarrow P \quad Q }
{ P }
\]

추론 규칙을 여러 번 사용하여 한 명제를 증명할 수 있다. 어떤 명제 \(P\), \(Q\), \(R\)에 대해 \(P\rightarrow(Q\rightarrow R)\), \(P\), \(Q\)가 참이라고 가정하자. 전건 긍정 규칙에서 \(p\)를 \(P\)로, \(q\)를 \(Q\rightarrow R\)로 치환하여 \(Q\rightarrow R\)이 참임을 얻을 수 있다. 다시 전건 긍정 규칙에서 \(p\)를 \(Q\)로, \(q\)를 \(R\)로 치환하면 \(R\)이 증명된다. 이는 아래의 증명 나무로 나타낼 수 있다.

\[
\frac
{ 
{\Large\frac
  {\large P\rightarrow(Q\rightarrow R) \quad P }
  { Q\rightarrow R } }\quad
  Q }
{ R }
\]

다음은 AE의 요약 문법을 추론 규칙으로 정의한 것이다.

\[
\frac
{ n\in\mathbb{Z} }
{ n\in\mathcal{A} }
\quad
\frac
{ e_1\in\mathcal{A} \quad e_2\in\mathcal{A} }
{ \{+\ e_1 \ e_2\}\in\mathcal{A} }
\quad
\frac
{ e_1\in\mathcal{A} \quad e_2\in\mathcal{A} }
{ \{-\ e_1 \ e_2\}\in\mathcal{A} }
\]

아래의 증명 나무는 \(\{+\ 4\ \{-\ 2\ 1\}\}\)이 \(\mathcal{A}\)의 원소라는 것을 증명한다.

\[
\frac
{ 
{\Large
\frac
  { 4\in\mathbb{Z} }
  { 4\in\mathcal{A} } \quad
  \frac
  { \frac
    { 2\in\mathbb{Z} }
    { 2\in\mathcal{A} }
    \frac
    { 1\in\mathbb{Z} }
    { 1\in\mathcal{A} }
  }
  { \{-\ 2\ 1\}\in\mathcal{A} }
}
}
{ \{+\ 4\ \{-\ 2\ 1\}\}\in\mathcal{A} }
\]

AE의 요약 문법은 Scala 코드로도 작성할 수 있다. AE의 식은 전형적인 대수적 데이터 타입이므로, 지난 글에서 다룬 것처럼 봉인된 트레잇과 경우 클래스를 사용하여 정의할 수 있다.

```scala
sealed trait AE
case class Num(n: Int) extends AE
case class Add(l: AE, r: AE) extends AE
case class Sub(l: AE, r: AE) extends AE
```

\(\{+\ 4\ \{-2\ 1\}\}\)를 Scala 코드로는 아래처럼 쓸 수 있다.

```scala
Add(Num(4), Sub(Num(2), Num(1)))
```

대부분의 요약 문법이 정의한 대상은 나무 모양이다. 요약 문법에 따라 만들어진 나무를 *요약* *문법* *나무*(abstract syntax tree; AST)라고 부른다. 아래 나무는 \(\{+\ 4\ \{-2\ 1\}\}\)을 시각적으로 표현한 것이다. 위 Scala 코드가 정의하고 있는 객체도 같은 구조로 되어 있다.

<div class="chart" id="tree-ae-0"></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/treant-js/1.0/Treant.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/raphael/2.2.8/raphael.js"></script>
<script src="./7_0_treant.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/treant-js/1.0/Treant.css">
<link rel="stylesheet" href="./7_0_treant.css">
<script>new Treant( simple_chart_config );</script>

### 파싱

*파싱*(parsing)은 구체적 문법을 따르는 문자열을 AST로 변환하고, 구체적 문법을 따르지 않는 문자열은 거부하는 과정이다. *파서*(parser)는 파싱을 하는 프로그램이다. 프로그래밍 언어 수업에서 파싱에 대해서는 자세히 다루지 않으므로, 이 글에서도 이 이상으로 설명하지는 않겠다.

Scala 표준 라이브러리는 *파서* *조합기*(parser combinator)를 제공하기 때문에, 사용자는 파싱에 대한 구체적인 지식 없이도 파서를 구현할 수 있다. 아래는 AE의 파서를 구현한 것으로, 입력은 문자열, 출력은 AE의 AST이다. 만약 문자열이 구체적 문법을 따르지 않는다면 예외가 발생한다. 단, 위에서 정의한 AE의 구체적 문법과는 달리 공백을 자유롭게 사용할 수 있다.

```scala
import scala.util.parsing.combinator._

object AE extends RegexParsers {
  def wrap[T](e: Parser[T]): Parser[T] = "{" ~> e <~ "}"
  lazy val n: Parser[Int] = "-?\\d+".r ^^ (_.toInt)
  lazy val e: Parser[AE] =
    n                  ^^ Num                         |
    wrap("+" ~> e ~ e) ^^ { case l ~ r => Add(l, r) } |
    wrap("-" ~> e ~ e) ^^ { case l ~ r => Sub(l, r) }

  def parse(s: String): AE =
    parseAll(e, s).getOrElse(throw new Exception)
}

AE.parse("1") 
// Num(1)

AE.parse("{+ 4 {- 2 1}}")
// Add(Num(4),Sub(Num(2),Num(1)))

AE.parse("{+ 1 2 3}")
// java.lang.Exception
```

## 감사의 말

글을 확인하고 의견을 주신 류석영 교수님께 감사드립니다.