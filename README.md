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

# Your mouse is a database

위에 문장은 Rx의 창시자인 Erik Meijer가 한 얘기로 Rx에서 비동기 처리를 위해 `시간`이라는 개념을 도입했다.

아래의 그림을 보면 마우스로 발생되는 이벤트들을 시간이라는 개념으로 표시해두었다.
![Image](https://github.com/tienne/lean-rxjs/blob/master/images/your_mouse_is_database.jpg?raw=true)
[그림출저 Erik Meijer의 Twitter]

우리가 일반적으로 알고있는 데이터베이스는 메모리 혹은 디스크 공간에 저장되는 데이터 집합이다.
근데 마우스가 데이터베이스라면 데이터에 해당하는 것은 무엇일까? 바로 이벤트다. Rx는 이 이벤트조차 데이터로 취급하는거다.
그 이벤트를 시간이라는 배열(컬렉션)에 담아. 아래와 같은 형태로 관리를 한다.

Mouse[click, click, move, move, move]

이것을 `스트림(Stream)`라고 표현한다.



 