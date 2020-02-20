# 📌

## Async (비동기)

*'지금' 과 '나중' 사이의 간극.
간극의유형 : 사용자 입력 대기, 데이터베이스 / 파일 시스템의 정보조회, 네트워크를 경유한 데이터 송신 후 응답 대기, 일정 시간동안 반복적인 작업 (Task) 수행 (ex. 애니메이션) 등*

네트워크 관련 모듈이 따로 있을 수 있다. (node js 에 fetch API를 가지고 있지 않는 것과 같다.)

브라우저 안에 자바스크립트 엔진, 네트워크 모듈, 렌더링 엔진, UI 제어하는 모듈, 브라우저의 저장 공간, 등 많은 걸 가지고 있다.

프로그램에서 '지금' 에 해당하는 부분 그리고 '나중' 에 해당하는 부분 사이의 관계가 바로 **비동기 프로그램의 핵심**이다.

'나중은 '지금의 직후가 아니다. ⇒ '지금' 당장 끝낼수 없는 작업은
비동기적으로 처리되므로 직관적으로도 알 수 있듯이 프로그램을 중단 (Blocking) 하지 않는다.

```js
var data = ajax('http://some.url.1');
console.log(data);
```

위와 같은 코드에서는 표준 Ajax 요청은 동기적으로 작동하지 않아 `ajax()` 결과값을 data 에 할당 할 수 없다. Ajax는 '지금' 요청하고 '나중에' 결과를 받는다.

'지금' 부터 '나중' 까지 '기다리는' 가장 간단한(사실상 최적의) 방법은 **콜백함수** 라는 장치를 이용하는 것이다.

```js
ajax('http://some.url.1', function myCallbackFunction(data) {
  console.log(data);
}) 
```

---



자바스크립트 엔진은 크게 `call stack` , `heap` 으로 나뉜다.

### call stack

*호출 스택*

call stack은 데이터 스트럭처로 실행되는 순서를 기억하고 있다. 

작업이 요청되면(함수를 실행하려면) 요청된 작업은 순차적으로 스택에 해당하는 함수를 집어 넣게 되는데, 그렇게 되면 call stack에 하나씩 쌓이게 되고 가장 위에 쌓인 순서대로 실행된다. 

함수에서 리턴이 일어나면 스택의 가장 위쪽에서 해당 함수를 꺼내게 된다. 

이게 call stack의 하는 일의 전부이다.

자바스크립트는 단 하나의 call stack을 사용하기 때문에 해당 task가 종료하기 전까지 다른 어떤 task도 수행될 수 없다.

### Heap

동적으로 생성된 객체 인스턴스가 할당되는 영역이다.



---

### callback queue(event queue, ...)

환경마다 다르게 부른다. (Node js 에서는 webAPI도 다르게 부른다 web 환경이 아니기때문에)

비동기 처리 함수의 콜백 함수, 비동기식 이벤트 핸들러, Time 함수(setTimeout, setInterval) 의 콜백 함수가 보관되는 영역으로 이벤트 루프에 의해 특정 시점(call stack이 비어있을 때) 순차적으로 call stack으로 이동되어 실행한다.

### event loop

> *여러 프로그램 덩이를 시간에 따라 매 순간 한번씩 엔진을 실행 시키는 "이벤트 루프"*
>
> *자바스크립트 엔진은 애당초 시간이라는 관념 따윈 없었고 임의의 자바스크립트 코드 조각을 시시각각 주는대로 받아 처리하는 실행기 일뿐, '이벤트'를 스케줄링 하는 일은 언제나 엔진을 감싸고 있던 주위 환경의 몫*
>
> *- You Don't Know JS-*

call stack내에 현재 실행중인 task가 있는지, callback queue에 대기 하고 있는 task가 있는지 반복해서 확인한다. call stack이 비어있다면 callback queue내에 task가 call stack으로 이동하고 실행한다.

`setTimeout()` 타이머가 항상 완벽하게 정확한 타이밍으로 동작하지 않는 이유는 이벤트 루프가 20개의 원소로 가득 차 있을 때 콜백은 새치기 하지 않고 줄 맨 끝에서 대기하기 때문이다.

적어도 지정한 시간 이전에는 콜백이 실행 되지 않을 것이라는 사실은 보장 할 수 있지만 정확히 언제, 혹은 좀 더 시간이 경과한 이후에 실행 될 지는 이벤트 루프 큐의 상황에 따라 달라진다.



```js
function fn1() {
  console.log('fn1');
  fn2();
}

function fn2() {
  setTimeout(function () {
    console.log('fn2');
  }, 0);
	console.log('fn2 end!');
}

fn1();
```

위와 같은 코드에서 어떻게 동작하는 지 확인해보자.

`fn1` 함수가 호출되면 call stack에 `fn1` 이 쌓이고 그 위에 `console.log('fn1')` 은 call stack에 쌓이고 콘솔에 'fn1' 이 찍히면서 사라진다.

 `fn2` 함수가 호출되어 call stack에 쌓인다. `setTimeout` 함수가 실행되고 나면 `setTimeout`함수는 webAPI 로 넘어가고 call stack에서 비워진다. 그 동안 webAPI에서는 `setTimeout` 시간을 재고 있게된다. 그 다음 `console.log('fn2 end!')` 가 실행되고 call stack에서 비워진다.

 마지막으로 남아있던 `fn1` 함수도 call stack에서 비워지게 되고, 그 사이 `setTimeout`은 시간이 다 되어 callback queue 에서 `callback` 함수로 넘어가 기다리게 되고, call stack이 다 비워짐을 event loop 이 확인 하고 call stack으로 `callback` 함수를 올려보내게 된다. 마지막으로 `console.log('fn2')` 가 찍히고 함수는 종료된다.



[출처] : 한빛미디어책 You Don't Know JS (this와 객체 프로토타입, 비동기와 성능) - kyle simpson

[출처] : [poiemaweb.com]([https://poiemaweb.com/js-event#2-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84event-loop%EC%99%80-%EB%8F%99%EC%8B%9C%EC%84%B1concurrency](https://poiemaweb.com/js-event#2-이벤트-루프event-loop와-동시성concurrency))