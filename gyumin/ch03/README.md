# 3장 함수 정의하기

## 3.1 함수

### 3.1.1 코틀린 함수의 구조

```Kotlin
fun circleAraea(radius: Double): Double{
    return PI*radius*radius
}

fun main(){
    print("Enter radius: ")
    val radius = readLine()!!.toDouble()
    println("Circle area: ${circleArea(radius)}"
}
```

* 함수의 구성요소는 다음과 같이 정리할 수 있다.
    * fun 키워드를 통해 컴파일러에게 함수 정의가 뒤따라온다는 사실을 알린다.
    * 변수 이름과 마찬가지로 아무 식별자나 함수 일므으로 쓸 수 있다.
    * 다음에는 괄호로 둘러싸여 있는 콤마(,)로 분리한 파라미터 목록이 온다
    * 반환 타입은 함수를 호출한 쪽에 돌려줄 반환값의 타입이다.
    * 함수 본문은 {}로 감싼 블록이며, 함수의 구현을 기술한다.
* 코틀린 함수의 파라미터는 무조건 불변입니다.
* 코틀린은 자바와 마찬가지로 call by value 입니다. 즉, 파라미터 값에 호출하는 쪽의 인자를 복사한다는 뜻입니다. 하지만 배열의 경우 배열 내부의 원소들을 변경하면, 원본 원소들도 변경됩니다.
* 파라미터의 타입은 항상 지정해줘야 합니다. 컴파일러는 함수 정의에서 파라미터 타입을 추론하지 못합니다.
* 함수의 반환 타입 또한 함수 파라미터에서 추론이 가능함에도 명시해줘야 합니다. 이는 return을 반환하는 지점이 여러군대 일 수 있어, 이 모든 반환 위치에서의 타입을 추론할 수 없기 떄문입니다.
* 함수의 반환 타입을 생략할 수 있을 때는 두 가지가 존재합니다.
    * 유닛(unit, 자바에서의 void)를 반환하는 경우
    * 식이 본문인 경우, 어떤 함수가 단일 식으로만 구성된 경우 return과 중괄호를 생략하고 = 를 통해 함수를 정의할 수 있습니다. 추가로 반환타입을 생략할 수 있습니다.
        * ```fun circleArea(radius: Double) : Double = PI*radius*radius```
        * ```fun circleArea(radius: Double)  = PI*radius*radius```
* 블록이 본문인 함수를 정의할 때 {} 앞에 '='를 추가하면 익명 함수를 기술하는 람다로 해석되어 원하는 결과를 얻을 수 없다.
    * 블록 정의 안에 return 문이 금지이기 때문입니다.
```
fun circleArea(radius: Double) = {PI*radius*radius} // 가능
  
fun circleArea(radius: Double) = {
    return PI*radius*radius
} // 예외 발생
```

### 3.1.2 위치 기반 인자와 이름 붙은 인자

* 함수 호출 인자는 순서대로 파라미터에 전달됩니다.
* 코틀린에서는 이름 붙은 인자(named argument)를 통해 변수를 위치가 아닌 이름을 통해 지정해줄 수 있습니다.

```Kotlin
fun rectangleArea(width: Double, height: Double): Double{
    return width*height
}

rectangleArea(width = w, height = h)
```

### 3.1.3 오버로딩과 디폴트 값

* 자바 메소드와 마찬가지로 코틀린 함수도 오버로딩 할 수 있습니다.
* 코틀린에서 주어진 호출 식에 대해 실제 호출할 함수를 결정할 때 컴파일러는 자바 오버로딩 해소 규칙과 비슷한 규칙을 따릅니다.
  * 파라미터의 개수와 타입을 기준으로 호출할 수 있는 모든 함수를 찾는다.
  * 덜 구체적인 함수를 제외시킨다. 파라미터 타입이 상위 타입일수록 덜 구체적인 함수입니다.
  * 후보가 하나로 압축되면 이 함수가 호출할 함수다. 후보가 둘 이상이면 캄파일 오류가 발생한다.
* 코틀린은 경우에 따라 함수 오버로딩을 쓰지 않아도 됩니다. 이는 코틀린이 디폴트 파라미터를 제공하기 때문입니다.
  * 디폴트 파라미터의 경우 함수 파라미터 목록에서 뒤쪽에 위치하는게 좋은 코딩 스타일입니다.

### 3.1.4 vararg

* 인자의 개수가 정해지지 않은 함수를 정의할 때, vararg를 통해 해결할 수 있습니다.

```Kotlin
fun printSorted(vararg items: Int){
    items.sort()
    println(items.contentToString())
}
```

* vararg를 사용하면 해당 파라미터를 배열 타입으로 사용할 수 있습니다.
* 스프레드 연산자인 *를 사용하면 배열을 가변 인자 대신 넘길 수 있습니다.

```Kotlin
val numbers = intArrayOf(6,2,10,1)
printSorted(*numbers)
printSorted(numbers) // 에러 발생
```

* 스프레드는 배열을 복사합니다. 따라서 배열 내용을 바꾸더라도 원본 배열에 영향을 미치지 않습니다.
* 스레레드를 사용하면 얕은 복사가 이뤄집니다. 즉, 배열 내부에 참조가 있는 경우 배열 내부의 원소가 공유될 수 있습니다.
* vararg 파라미터 이후 파라미터가 존재하면 해당 파라미터는 이름을 통해 값을 전달해줘야 합니다.
* vararg는 오버로딩 해소에도 영향을 미칩니다.
  * vararg 파라미터가 있는 함수는 동일한 타입의 파라미터 수가 고정돼 있는 함수보다 덜 구체적인 함수로 간주됩니다.

### 3.1.5 함수의 영역과 가시성

* 코틀린 함수는 정의된 위치에 따라 세 가지로 구분할 수 있습니다.
  * 파일에 직접 선언된 최상위 함수
  * 어떤 타입 내부에 선언된 멤버 함수
  * 다른 함수 안에 선언된 지역 함수
* 디폴트로 최상위 함수는 공개 함수입니다. 즉, 최상위 함수는 함수가 정의된 파일 내부뿐 아니라 프로젝트 어디에서나 사용할 수 있습니다.
* 최상위 함수 정의 앞에 private 또는 internal을 통해 가시성을 변경할 수 있습니다.
  * internal은 함수가 적용된 모듈 내부에서만 사용할 수 있습니다.
* 코틀린에서는 지역 변수처럼 지역 함수를 정의할 수 있습니다.
* 지역 함수는 자신을 둘러싼 함수, 블록에 선언된 변수나 함수에 접근할 수 있습니다.
* 지역 함수와 변수에는 가시성을 변경자를 붙일 수 없습니다.


## 3.3 조건문

### 3.3.1 if 문으로 선택하기

* 코틀린의 if 문은 자바와 다르게 식으로 사용할 수 있습니다.

```fun max(a: Int, b: Int) = if(a > b) a else b```
* if 문을 식으로 사용할때는 if와 else 모두 존재해야 합니다.

> 코틀린에는 3항 연산자가 존재하지 않습니다.

* if 식에서 return을 사용하면 편리한 경우가 있습니다.
  * return 문은 존재하지 않는 값을 뜻하는 Nothing이라는 특별한 타입의 값으로 간주됩니다.
  * Nothing 타입은 프로그램의 순차적 제어 흐름이 그 부분에서 끝나되 어떤 잘정의된 값에 도달하지 못한다는 듯입니다.
  * Nothing 타입은 모든 코틀린 타입의 하위 타입으로 간주되기 때문에 식이 필요한 위치에 return을 사용해도 타입 오류가 발생하지 않습니다.

### 3.3.2 범위, 진행, 연산

* 코틀린은 순서가 정해진 값 사이의 수열을 표현하는 몇 가지 타입을 제공합니다.
* 범위를 만드는 가장 간단한 방법은 수 값에 대해 .. 연산자를 사용하는 것입니다.
  * ```val chars = 'a'..'h'```
  * ```val twoDigits = 10..99```
  * ```val zeroo2One = 0.0..1.0```
* in 연산을 사용하면 어떤 값이 범위 안에 들어있는지 알 수 있습니다.
  * ```println(num in 10.99) // num >= 10 && num <= 99```
* downTo 연산을 통해서 아래로 내려가는 진행을 만들 수 있습니다.
* step 을 통해서 진행의 간격을 지정할 수 있습니다.
  * 진행 간격은 양수여야 합니다.
* 범위와 진행 타입은 코틀린 표준 라이브러리에 IntRange, FloatRange, CharProgression, IntProgression 등으로 정의돼 있습니다.
* 일반적으로 범위는 동적으로 할당되는 객체이기 때문에 비교 대신 범위를 사용하면 약간의 부가 비용이 발생합니다. 히자만 컴파일러는 꼭 필요할 때만 실제 객체를 만들어내기 위해 노력합니다.

### 3.3.3 when 문과 여럿 중에 하나 선택하기

```Kotlin
fun hexDigit(n: Int) : Char{
    when {
        n in 0..9 -> return '0'+n
        n in 10.15 -> return 'A' + n-10
        else -> return '?'
    }
}
```

* 기본적으로 when 문은 when 키워드 다음에 블록이 옵니다.
* 블록 안에는 조건 -> 문 형태로된 여러 개의 가지와 else -> 문 형태로 된 한 가지가 있을 수 있습니다.
* 코틀린의 when은 자바의 switch와 비슷하지만 자바는 break를 만날 때까지 진행하지만 코틀린은 조건을 만족하는 식이 발견되면 바로 종료합니다.
* when 문을 실행하는 방법은 앞에서 본 첫 번째 형식의 when과 비슷하게 다음과 같습니다.
  * 대상 식을 평가합니다. 계산한 값을 subj라고 합니다.
  * 프로그램은 최초로 참인 조건을 찾을 때까지 각 가지의 조건을 코드에 나온 순서대로 평가합니다.
  * 참인 조건을 찾으면 그 가지의 문장을 실행합니다. 참인 조건이 없다면 else 가지의 문장을 실행합니다.
* when은 if와 마찬가지로 식으로 사용할 수 있습니다.
  * 식으로 사용할 때, 정의한 변수는 when 블록 내부에서만 사용할 수 있으며, var로 선언할 수 없습니다.

```Kotlin
fun readHexDigit() = when(val n = readLine()!!.toInt()) {
    in 0..9 -> '0'+n
    in 10..15 -> 'A'+n-10
    else -> '?'
}
```


## 3.4 루프

### 3.4.4 내포된 루프와 레이블

* 코틀린에서는 자바의 임의 레이블과 비슷한 문법을 제공합니다.

```Kotlin
fun indexOf(subArray: IntArray, array: IntArray): Int{
  outerLoop@ for (i in array.indices){
    for(j in subArray.indices){
      if(subarray[j] !=  array[i+j]) continue@outerLoop
    }
    return i
  return -1
}
```

* 코틀린은 어느 문장 앞에든 레이블을 붙일 수 있지만 break와 continue에는 구체적으로 루프 앞에 붙은 레이블만 사용할 수 있습니다.

### 3.4.5 꼬리 재귀 함수

* 코틀린은 꼬리 재귀(tail recursive) 함수에 대한 최적화 컴파일을 제공합니다.

```Kotlin
tailrec fun binIndexOf(
  x: Int,
  array: IntArray,
  from: Int=0,
  to: Int = array.size
){
  if(from==to) return -1
  val midIndex = (from+to-1)
  val mid = array[midIndex]
  return when{
    mid < x -> binIndexOf(x, array, midIndex + 1, to)
    mid > x -> binIndexOf(x, array, from, midIndex)
    else -> midIndex
  }
}
```

* 비재귀와 비교해서 재귀는 성능 차원에서 약간의 부가 비용과 스택 오버플로 발생할 가능성이 있습니다.
* 코틀린에서는 tailrec을 통해 재귀 함수를 비재귀적인 코드로 자동으로 변환해줍니다. 이를 통해 재귀 함수의 간결함과 비재귀 루프의 성능만을 취할 수 있습니다.

```Kotlin
tailrec fun binIndexOf(
  x: Int,
  array: IntArray,
  from: Int=0,
  to: Int = array.size
){
  var fromIndex = from
  var toIndex = to
  
  while(true){
    if(fromIndex==toIndex) return -1
    val midIndex = (fromIndex+toIndex-1)
    val mid = array[midIndex]
    
    when{
      mid < x -> fromIndex = midIndex+1
      mid > x -> toIndex = midIndex
      else -> return midIndex
    }
  }
}
```

* 재귀함수에서 비재귀함수로 변환하기 위해서는 재귀 호출 다음에 아무 동작도 수행하지 말아야합니다. 즉, 재귀 호출과 함께 추가적인 연산이 있으면 안됩니다.