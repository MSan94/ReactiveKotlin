# 리액티브 프로그래밍이란?
- 데이터 스트림과 변경 사항 전파를 중심으로 하는 비동기 프로그래밍 패러다임
- 즉, 데이터와 데이터 스트림에 영향을 미치는 모든 변경사항을 관련된 모든 당사자들을 리액티브 프로그래밍이라 한다.

```
fun main(args : Array<String>){
    var number = 4
    var isEven = isEven(number)
    println("The number is " + (if(isEven) "Even" else "Odd"))
    number = 9
    println("The number is " + (if(isEven) "Even" else "Odd"))
}
fun isEven(n:Int) : Boolean = ((n % 2 ) == 0)
```
- number에 새로운 값이 할당되었지만 isEven은 여전히 참이다.
- 하지만, isEven이 number 변수의 변경 사항을 추적하도록 설정되었다면 false가 되었을 것이다. -> 리액티브 프로그래밍 동작

# 함수형 리액티브 프로그래밍을 적용해야 하는 이유
- 콜백 지옥의 제거
  - 콜백은 미리 정의된 이벤트가 발생할 때 호출되는 메서드다.
  - 인터페이스를 콜백 메서드와 함게 전달하는 메커니즘을 콜백 메커니즘이라고 부른다.
  - 이 메커니즘에는 인터페이스와 그 구현 등을 비록해 많은 코드가 필요하다.
- 오류 처리를 위한 표준 메커니즘
  - 일반적으로 복잡한 작업과 HTTP를 사용해 작업한느 동안 발생하는 오류의 처리는 주요 관심사
  - 표준 메커니즘이 없을 경우 많은 어려움이 발생
- 간결해진 스레드 사용
  - 스레드를 코틀린에서 자바에 비해 더 쉽게 사용할 수 있지만, 여전히 복잡하다.
  - 리액티브 프로그래밍을 통해 한층 더 쉽게 사용 가능
- 간단한 비동기 연산
  - 스레드와 비동기 작업은 서로 연관돼 있다.
  - 스레드 사용이 쉬워질수록 비동기 연산도 쉬워진다.
- 전체를 위한 하나, 모든 작업에 대해 동일한 API
  - 리액티브 프로그래밍, 특히 RxKotlin은 간단하고 직관적인 API를 제공한다.
  - 네트워크 호출, 데이터베이스 접근, 계산 또는 UI 연산 등 어느 곳에나 어떤 것을 대상으로 사용 가능
- 함수형 접근
  - 리액티브 프로그맹일을 통해 함수형 접근 방법을 취해서 가독성이 좋은 선언적 코드를 작성할 수 있다.
- 유지보수와 테스트 코드
  - 유지보수와 테스트 코드가 한층 쉬워진다.

# 리액티브 선언
- 리액티브 선언이란 4가지 리액티브 원리는 정의해놓은 문서이다. ( http://www.reactivemanifesto.org )
- 응답성
  - 시스템은 즉각 응답해야 한다. 응답성 있는 시스템은 신속하고 일관성 있는 응답시간을 유지해 일관된 서비스 품질 제공
- 탄력성
  - 시스템에 장애가 발생하더라도 응답성을 유지해야 한다. 
  - 탄력성은 복제, 봉쇄, 격리, 위임에 의해 이루어진다.
  - 장애는 각 컴포넌트 내부로 억제돼 각 컴포넌트들을 서로 격리시키기에 하나의 컴포넌트에 장애가 발생해도 전체 시스템에 영향을 끼치지 못한다.
- 유연성
  - 리액티브 시스템은 작업량이 변하더라도 그 변화에 대응하고 응답성을 유지해야 한다.
- 메시지 기반
  - 탄력성의 원칙을 지키려면 리액티브 시스템은 비동기적인 메시지 전달에 의존해 컴포넌트들 간의 경계를 형성해야 한다.

# 리액티브 스트림 표준 사양
- http://www.reactive-streams.org/

# 코틀린을 위한 리액티브 프레임워크
- 도움되는 라이브러리
  - RxKotlin
  - Reactor-Kotlin
  - Redux-Kotlin
  - FunKTionale

# RxJava의 푸시 메커니즘과 풀 매커니즘 비교
- Rxkotlin은 전통적인 프로그램에서 사용되는 반복자 패턴의 풀 메커니즘 대신 푸시 메커니즘의 데이터/이벤트 시스템으로 대표되는 옵저버블 패턴을 중심으로 작동
- 그렇기 때문에 지연 평가가 일어나면 동기식 또는 비동기식으로 모두 이용 가능
```
fun main(args: Array<String>){
    var list:List<Any> = listOf("One",2,"Three","Four",4.5,"Five",6.0f)
    var iterator = list.iterator()
    while(iterator.hasNext()){
        println(iterator.next())
    }
}
```
```
fun main(args:Array<String>){
    var list:List<Any> = listOf("One",2,"Three","Four",4.5,"Five",6.0f)
    var observable: Observable<Any> = list.toObservable();
    
    observable.subscribeBy(
        onNext = { println(it) },
        onError = { it.printStackTrace() },
        onComplete = { println("Done!") }
    }
}

// 리스트 생성 -> 리스트에 대한 observable 인스턴스 생성 -> observable 인스턴스를 구독
// observable을 구독했기 때문에 모든 변경 사항은 onNext로 푸시괼 것이고, 모든 데이터가 푸시되면 onComplete, 에러발생시 onError 호출
```


# ReactiveEvenOdd 프로그램
```
fun main(args : Array<String>) {
    var subject:Subject<Int> = PublishSubject.create()
    subject.map( { isEven(it) }).subscribe({println("The number is ${(if (it) "Even" else "Odd" )}" )})
    subject.onNext(4)
    subject.onNext(9)
}
// the number is Even
// the number is Odd
```
