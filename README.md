# RxJs?

ReactiveX 시리즈의 JavaScript 구현체 라이브러리가 RxJs

그럼 ReactiveX란 무엇인가?

그 정보는 [나프콘 2016 김훈민님의 발표내용](https://www.youtube.com/watch?v=3FKlYO4okts) 또는 [김훈민님 블로그](http://huns.me/development/2051)에서 자세히 확인이 가능하니 여기서는 간략하게만 설명하자

공식홈페이지에는 아래와 같이 설명되어있다.
>ReactiveX is a library for composing asynchronous and event-based programs by using observable sequences.

이 내용을 해석하면
>ReactiveX는 observable 시퀀스를 사용하여 비동기 및 이벤트 기반 프로그램을 작성하기위한 라이브러리입니다.

처음 ReactivX는 .NET용 라이브러리로 출시되었으며, 그이후 RxJS / RxJava / Rx++ 등 추가로 출시하면서 오픈소스화 하였다고한다

ReactivX는 비동기의 문제를 해결하기 위해 만들어졌는데, 도대체 어떠한 방향으로 해결할려고 했을까?

# Reactive? 도대체 요즘 왜이리 많이 나오는건데

요즘 함수형 프로그래밍 혹은 리액티브 프로그래밍라는 용어가 자주 보인다. 
사실 너무 깊게들어가면 너무 어렵다. 그래서 천천히 작성하자...

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












 