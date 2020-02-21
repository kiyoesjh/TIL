# 📌

## Promise

```js
class Promise {
  constructor(fn) {
    fn();
  }
}
```

과 비슷하다 볼수 있다.

```js
function asyncWorkre(resolve, reject) {
  if(true) {
    resolve("...");
  }else {
		reject("...");
  }
}

const promise = new Promise(asyncWork);
//비동기 코드가 실행되고 나서 이후에 뭔가 해야하는 상황
//비동기 코드 : asyncWork
//이후에 해야 하는 일 : then 에 등록

```

```js
function asyncWorkre(resolve, reject) {
  console.log(0);
  if(true) {
    resolve("...");
  }else {
		reject("...");
  }
}

const promise = new Promise(asyncWork);
console.log(1);

promise.then(function(result) {
  console.log(result);
}, function(err) {
  console.log(err);
});

console.log(2);

//실행 순서
//0 , 1, then, 2, result msg
```

 

`new Promise` : 객체를 만든다, 비동기 처리를 도와주는 물건

객체라는 게 만들어 졌다, 실물로 갖고 있을 수 있게 된것.

**비동기 처리를 객체로 다룰 수 있게 된 것이 핵심이다.**

```js 
var somePromise = new Promise(function (resolve, reject)) {
  resolve(data); //해결된 것

	reject(error); //실패된 것
}
```

`resolve` 와 `reject` 함수를 인자로 받는다.

```js
somePromise.then(function(data) {
  console.log('Success: ', data);
}).catch(function(error) {
  console.error('Error: ', error);
})
```

`then` , `catch` 이라는 메소드는 인스턴스 레벨에 있는 메소드이다.

* `then` : 인자로 받는 콜백은 `resolve` 되었을 때 호출 된다.
  `resolve(data);` 할때 넘겨주는 인자(data) 가 `then(function(data){});` 의 콜백의 인자로 들어간다.
* `catch`  : 인자로 받은 콜백은 `reject` 되었을 때 호출 된다. 
  `reject(error);` 할때 넘겨주는 인자(error) 가 `catch(function(error){});` 의 콜백의 인자로 들어간다.
* `Promise.all()` 이라는 메서드도 있다. 모든 비동기가 완료된 뒤에 사용하는.

