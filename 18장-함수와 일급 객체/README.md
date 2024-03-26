# 18장 함수와 일급 객체

## 18.1 일급 객체

#### 일급 객체의 조건

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

***자바스크립트 함수도 일급 객체에 해당한다.***
<details>
  <summary>예제</summary>
  
```js
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다. 
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.  
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2

```

</details>

#### 함수 vs 일반 객체
일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다. 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.


## 18.2 함수 객체의 프로퍼티
함수도 객체이므로 프로퍼티를 갖는다.

<details>
  <summary>함수 객체의 프로퍼티</summary>

  ![함수 객체의 프로퍼티](./함수%20객체의%20프로퍼티.png)

</details>

square 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트를 `Object.getOwnPropertyDescriptors` 메서드로 확인 가능

![함수 객체 데이터 프로퍼티](./함수%20객체%20데이터%20프로퍼티.png)

- `arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티
일반 객체에는 없는 **함수 객체 고유의 프로퍼티**


### arguments 프로퍼티
함수 객체의 `arguments` 프로퍼티 값은 `arguments` 객체다.   
`arguments` 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용되므로 함수 외부에서 참조할 수 없다.

```js
function multiply(x, y) {
    console.log(arguments);
    return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); // 2
console.log(multiply(1, 2, 3)); // 2
```

1. 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않기 때문에, 함수 호출 시 매개변수와 인수의 개수가 달라도 에러가 발생하지 않는다.

2. 함수를 정의할 때 선언한 매개변수는 함수 내부에서 변수와 동일하게 취급된다.   
함수가 호출되면 함수 내부에서 암묵적으로 매개변수가 선언되고 `undefined`로 초기화된 이후 인수가 할당됨.

3. `매개변수의 개수 > 전달된 인수` : 인수가 전달되지 않은 매개변수는 `undefined` 상태 유지한다.  
   `매개변수의 개수 < 전달된 인수` : 초과된 인수는 무시된다.

4. 이때 모든 인수는 암묵적으로 `arguments` 객체의 프로퍼티로 보관된다.     


![arguments 객체의 프로퍼티](./arguments%20객체의%20프로퍼티.png)
- arguments 객체의 프로퍼티 키는 인수의 순서, 프로퍼티 값은 인수.  
- arguments 객체의 `callee` 프로퍼티는 함수 자신(=호출되어 arguments 객체를 생성한 함수)  
- arguments 객체의 `length` 프로퍼티는 인수의 개수
- arguments 객체의 `Symbol(Symbol.iterator)` 프로퍼티는 arguments 객체를 순회 가능한 자료구조인 이터러블(iterable)로 만들기 위한 프로퍼티(34장 이터러블 참고)

#### arguments 객체는 매개변수 개수를 확정할 수 없는 `가변 인자 함수`를 구현할 때 유용

```js
function sum() {
  let res = 0;

  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }
  return res;
}
console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

arguments 객체는 유사 배열 객체.  
>유사 배열 객체 : length 프로퍼티를 가진 객체, for문으로 순회할 수 있는 객체

### caller 프로퍼티

함수 자신을 호출한 함수를 가리킴  
ECMAScript 사양에 포함되지 않은 비표준 프로퍼티

```js
fuction foo(func) {
  return func();
}

function bar() {
  return bar.caller;
}

console.log(foo(bar)); // function foo(func) {...}
console.log(bar()); // null
```

### length 프로퍼티

함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

```js
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x + y;
}
console.log(baz.length); // 2
```

- `arguments` 객체의 `length` 프로퍼티와 함수 객체의 `length` 프로퍼티 값은 다를 수 있음  
- `arguments` 객체의 `length` 프로퍼티는 인자 개수를 가리키고,  
함수 객체의 `length` 프로퍼티는 매개변수의 개수를 가리킴

### name 프로퍼티

함수 객체의 `name` 프로퍼티는 함수 이름을 나타낸다.  
name 프로퍼티는 ES5와 ES6에서 동작을 달리한다.  
`익명 함수 표현식`의 경우 ES5에서 `name` 프로퍼티는 **빈 문자열**을 값으로 갖는다.  
ES6에서는 **함수 객체를 가리키는 식별자**를 값으로 갖는다.

```js
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

var anonymousFunc = function () {};
console.log(anonymousFunc.name); // anonymousFunc

function bar() {
  console.log(bar.name); // bar
}
```


### `__proto__` 접근자 프로퍼티

`__proto__` 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티.  
내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.  
[[Prototype]] 내부 슬롯에도 직접 접근할 수 없으며 `__proto__` 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있다.

```js
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnPropery 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false

console.log(Object.prototype.hasOwnProperty("__proto__")); // true
```

**`hasOwnProperty` 메서드**  
인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 `true`,  
상속받은 프로토타입의 프로퍼티 키인 경우 `false` 반환

### `prototype` 프로퍼티

생성자 함수로 호출할 수 있는 함수 객체, 즉 `constructor`만이 소유하는 프로퍼티다.  
일반 객체와 생성자 함수로 호출할 수 없는 `non-constructor`에는 `prototype` 프로퍼티가 없다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnPrototype("prototype"); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnPrototype("prototype"); // false
```

`prototype` 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.
