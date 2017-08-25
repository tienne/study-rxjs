# ReactiveX? 리액티브? 요즘 왜이리 자주 나오는건데?

요즘 함수형 프로그래밍 혹은 리액티브 프로그래밍라는 용어가 자주 보인다. 

이 문서에서는 Reactive 혹은 함수형 프로그래밍에 대한 설명을 깊게 하진 않는다.

왜냐면 설명하자니 너무 어렵다. 그치만 중요하다 개인발전을 좋아하는 개발자라면 꼭 한번 찾아서 읽어보자  

ReactiveX 또는 리액티브 프로그래밍 대한 정보는 [나프콘 2016 김훈민님의 발표내용](https://www.youtube.com/watch?v=3FKlYO4okts) 또는 [김훈민님 블로그](http://huns.me/development/2051)에서 자세히 확인이 가능하니 여기서는 ReactiveX에 대해서만 간략하게 알아보자

### ReactiveX란 무엇인가?

공식홈페이지에는 아래와 같이 설명되어있다.
>ReactiveX is a library for composing asynchronous and event-based programs by using observable sequences.

이 내용을 해석하면
>ReactiveX는 observable 시퀀스를 사용하여 비동기 및 이벤트 기반 프로그램을 작성하기위한 라이브러리입니다.

처음 ReactivX는 .NET용 라이브러리로 출시되었으며, 그이후 RxJS / RxJava / Rx++ 등 추가로 출시하면서 오픈소스화 하였다고한다

# RxJs?

ReactiveX 시리즈의 JavaScript 구현체 라이브러리가 RxJs

Angular 2(이하 Angular)에서 비동기 처리등에 적극 도입했으며, Angular의 필수 라이브러리 중 하나이다. 

ReactiveX는 비동기의 문제를 해결하기 위해 만들어졌는데, promise / await / generator 등과 무엇이 다르며, 도대체 어떠한 방향으로 해결할려고 했을지 알아보자

# Your mouse is a database

위에 문장은 Rx의 창시자인 Erik Meijer가 한 얘기로 Rx에서 비동기 처리를 위해 `시간`이라는 개념을 도입했다.

아래의 그림을 보면 마우스로 발생되는 이벤트들을 시간이라는 개념으로 표시해두었다.
![Image](https://github.com/tienne/lean-rxjs/blob/master/images/your_mouse_is_database.jpg?raw=true)
[그림출저 Erik Meijer의 Twitter]

우리가 일반적으로 알고있는 데이터베이스는 메모리 혹은 디스크 공간에 저장되는 데이터 집합이다.
근데 마우스가 데이터베이스라면 데이터에 해당하는 것은 무엇일까? 바로 이벤트다. Rx는 이 이벤트조차 데이터로 취급하는거다.
그 이벤트를 시간이라는 배열(컬렉션)에 담아, 아래와 같은 형태로 관리를 한다.

Mouse[click, click, move, move, move]

이것을 `스트림(Stream)`라고 표현한다.

Rx에는 이러한 개념에 LINQ(Language Intergrated Query)를 도입했다.[LINQ도 에릭 마이어가 만들었다.] LINQ는 통합 질의 언어로 코드에서 데이터를 가져올때 SQL문처럼 표현할 수 있도록 도와주는 일종의 언어 확장이다.

제공하는 방식으로는 SQL 방식과 메서드 방식이 있다. Rx에서는 메서드 방식을 도입했다.

간단한 예시로 아래를 보자

먼저 SQL 방식이다.
```csharp
var productName = from product in products
           where product.id == 1
           select product.name;
```

그 다음은 메서드 방식이다.
```csharp
var productName = products
    .Where(product => product.id == 1)
    .Select(product => product.name);
```

그럼 Rx에서는 LINQ를 어떻게 활용하고 있는지 보자.

```javascript 1.7
var button = document.getElementById('submitBtn');
var clickStream = Rx.Observable.fromEvent(button, 'click');

clickStream
  .filter(e => {
    return e.altKey;
  })
  .subscribe(e => {
    console.log(e.clientY);
  });
```

쿼리로 표현하면 이런식 아닐까 싶다.
```SQL
SELECT clientY FROM clicks where altKey == true;
```

# RxJs의 구성요소 및 키워드

- Observable: 미래의 값(value)나 이벤트의 호출 가능한 수집의 개념을 나타냄.
- Observer: Observable에 의해 전달 된 값을 처리하는 콜백 콜렉션.
- Subscription: Observable의 실행을 나타냄. 주로 실행 취소에 유용함.
- Operators: map, filter, concat, flatMap 등과 같은 method를 사용하여 컬렉션을 다루는 함수 프로그래밍 스타일을 가능하게하는 순수(pure) 함수.
- Subject: EventEmitter와 동일하며 여러 Observers에 값 또는 이벤트를 멀티 캐스팅함.
- Schedulers: 동시 처리를 제어하는 중앙 집중식 디스패처로서 계산이 언제 발생하는지 조정할 수 있다. setTimeout or requestAnimationFrame or others.

각각의 요소들에 대해 알아보자

### Observable

Observable은 관찰할 수 있는 대상 혹은 데이터를 지칭합니다. 

이 데이터는 ['Rxjs', 'RxJava'] 같은 배열 데이터도 될 수 있고 Ajax 비동기 통신 결과 혹은 클릭 이벤트 등 Observable로 만들 수 있습니다.

간단한 Observable 객체를 만들어보도록 하겠습니다.

```typescript
const observable$ = Rx.Observable.create(function(observer) {
    observer.onNext(1);
    observer.onNext(2); 
    observer.onNext(3); 

    observer.onCompleted();
});
observable$.subscribe(observer);
```
이 observable은 [1, 2, 3] 같이 표현할 수 있으며, 이러한 Observable를 관찰(구독)하는 대상을 Observer라고 합니다.

이 observable를 subscribe 메서드를 이용해서 observer에 구독할 수 있습니다. 

### Observer

이 Observer는 아래처럼 정의되어있습니다.

```typescript
interface Observer<T> {
  closed?: boolean;
  next: (value: T) => void;
  error: (err: any) => void;
  complete: () => void;
}
```

- next: 다음 데이터를 불러올때 발생되는 메서드입니다.
- error: 데이터를 불러오는 과정에서 오류가 발생되었을때 발생되는 메서드입니다.
- complete: 모든데이터를 다 불러왔을때 발생되는 메서드입니다.

위에서 선언했던 observable$를 구독하여 observer$를 처리 해보도록 하겠습니다.
```typescript
const observable$ = Rx.Observable.create(function(observer) {
    observer.onNext(1);
    observer.onNext(2); 
    observer.onNext(3); 
    
    observer.onCompleted();
});

const subscription = observable$.subscribe(
  (value) => console.log(`값: ${value}`),
  (error) => console.log(`에러: ${error}`),
  () => console.log(`완료`)
);

//구독 취소
//subscription.unsubscribe();

//결과 console
//"값: 1"
//"값: 2"
//"값: 3"
//"완료"
```

여기서 변수명 `observable$` 변수 뒤에 $는 해당 변수가 `스트림(stream)` 혹은 `Observable`임을 나타내는 의미입니다.

observable를 구독하면 리턴값으로 `Subscription`이 생성되는데 위에 구성요소에 설명한거와 같이 `unsubscribe()` 메서드를 이용하여 구독을 취소에 사용됩니다.

### Subscription


 












 