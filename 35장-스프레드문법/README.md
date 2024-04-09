# 35장 스프레드 문법

ES6에서 도입된 스프레드 문법 `...`은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.  
스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, DOM 컬렉션(NodeList, HTMLCollection, arguments)와 같이 for...of 문으로 순회할 수 있는 이터러블에 한정된다.

```js
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다.
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(..."Hello"); // H e l l o

// Map과 Set은 이터러블이다.
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // ['a', '1'] ['b', '2']
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 }); // TypeError...
```

스프레드 문법의 결과물은 값들의 목록이므로, 값으로 사용할 수 없고 변수에 할당할 수 없다.

```js
const list = ...[1, 2, 3]; // SyntaxError...
```

다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

## 함수 호출문의 인수 목록에서 사용하는 경우

```js
const arr = [1, 2, 3];

// 배열 arr의 요소 중에서 최대값을 구하기 위해 Math.max를 사용한다.
// Math.max 메서드에 숫자가 아닌 배열을 인수로 전달하면 NaN을 반환한다.
const max = Math.max(arr); // -> NaN
```

```js
// 스프레드 문법 이전
var max = Math.max.apply(null, arr); // 3

// 스프레드 문법 사용
const max = Math.max(...arr); // 3
```

#### 스프레드 문법 vs Rest 파라미터

- 스프레드 문법 : 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것
- Rest 파라미터 : 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ...을 붙이는 것
- 스프레드 문법과 Rest 파라미터는 서로 반대의 개념

```js
// Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
  console.log(rest); // 1, 2, 3 -> [1, 2, 3]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

## 배열 리터럴 내부에서 사용하는 경우

### concat

2개의 배열을 1개의 배열로 결합하고 싶은 경우

```js
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

### splice

어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하고 싶은 경우

```js
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

// 세 번째 인수 arr2를 해체하여 전달해야 한다.
// 그렇지 않으면 arr1에 arr2 배열 자체가 추가된다.
arr1.splice(1, 0, arr2);
console.log(arr1); // [1, [2, 3], 4]

Array.prototype.splice.apply(arr, [1, 0].concat(arr2));
console.log(arg1); // [1, 2, 3, 4]

// ES6
arr1.splice(1, 0. ...arr2);
console.log(arr1); // [1, 2, 3, 4];
```

### 배열 복사

```js
// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(origin); // [1, 2]
console.log(copy === origin); // false
```

```js
// ES6
const origin = [1, 2];
const copy = [...origin];

console.log(origin); // [1, 2]
console.log(copy === origin); // false
```

### 이터러블을 배열로 변환

```js
// ES5
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.call(arguments);

  return args.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6

// ES6
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6

// Rest 파라미터 args는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

console.log(sum(1, 2, 3)); // 6
```

### 객체 리터럴 내부에서 사용하는 경우

스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

```js
// 스프레드 프로퍼티
// 객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

스프레드 프로퍼티가 제안되기 이전에는 Object.assign 메서드를 사용하여 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```js
// 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x: 1, y: 10, z: 3 }

const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // { x: 1, y: 100 }
```

스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있다.

```js
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
console.log(chaged); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, z: 0 };
console.log(added); // { x: 1, y: 2, z: 0 }
```
