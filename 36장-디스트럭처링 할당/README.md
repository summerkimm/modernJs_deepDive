# 36장 디스트럭처링 할당

구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

## 배열 디스트럭처링 할당

```js
// ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3
```

```js
// ES6
const arr = [1, 2, 3];

// 할당 연산자 왼쪽에 값을 할당받을 변수를 선언해야 함
// 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 함
// 할당 기준은 배열의 인덱스
const [one, two, three] = arr;

console.log(one, two, three); // 1 2 3
```

우변에 이러터블을 할당하지 않으면 에러가 발생한다.

```js
const [x, y]; // SyntaxError...

const [a, b] = {}; // TypeError...
```

```js
// 변수의 개수와 이터러블 요소 개수가 반드시 일치할 필요는 없음
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3
```

```js
// 변수에 기본값을 설정할 수 있다.
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

// 기본값보다 할당된 값이 우선한다.
const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); // 1 2 3
```

변수에 Rest 파라미터와 유사하게 **Rest 요소** `...`을 사용할 수 있다.  
Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 한다.

```js
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [2, 3]
```

## 객체 디스트럭처링 할당

```js
// ES5
var user = { firstName: "Soeun", lastName: "Kim" };

// 프로퍼티 키를 사용해 변수에 할당함
var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); // Soeun Kim
```

```js
// ES6
const user = { firstName: "Soeun", lastName: "Kim" };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서 의미 X
// 할당 연산자 왼쪽에 프로퍼티 값을 할당받을 변수를 선언해야 한다.
// 변수는 객체 리터럴 형태로 선언한다
const { lastName, firstName } = user;
console.log(firstName, lastName); // Soeun Kim
```

```js
// 우변에 객체 또는 객체로 평가될 수 있는 표현식(문자열, 숫자, 배열 등)을 할당하지 않으면 에러가 발생한다.
const { lastName, firstName}; // SyntaxError...

const { lastName, firstName} = null;
```

객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.

```js
const user = { firstName: "Soeun", lastName: "Kim" };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어짐
// 프로퍼티 키가 lastName인 프로퍼티 값을 ln에 할당함
// 프로퍼티 키가 firstName인 프로퍼티 값을 fn에 할당함
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); // Soeun Kim
```

```js
// 변수에 기본값을 설정할 수 있다.
const { firstName = "Soeun", lastName } = { lastName: "Kim" };
console.log(firstName, lastName); // Soeun Kim

const { firstName: fn = "Soeun", lastName: ln } = { lastName: "Kim" };
console.log(fn, ln); // Soeun Kim
```

객체 디스트럭처링 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.

```js
const str = "Hello";
// String 래퍼 객체로부터 length 프로퍼티만 추출함
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: "HTML", completed: true };
// todo 객체로부터 id 프로퍼티만 추출함
const { id } = todo;
console.log(id); // 1
```

객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.

```js
function printTodo(todo) {
  console.log(
    `할일 ${todo.content}은 ${todo.completed ? "완료" : "비완료"} 상태입니다.`
  );
}
printTodo({ id: 1, content: "HTML", completed: true });
// 할일 HTML은 완료 상태입니다.

// 매개변수 todo에 객체 디스트럭처링 할당을 사용할 수 있다.
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`);
}
printTodo({ id: 1, content: "HTML", completed: true });
// 할일 HTML은 완료 상태입니다.
```

```js
// 배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당 혼용 가능한다.

const todos = [
  { id: 1, content: "HTML", completed: true },
  { id: 2, content: "CSS", completed: false },
  { id: 3, content: "JS", completed: false },
];

// todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
const [, { id }] = todos;
console.log(id); // 2
```

```js
// 중첩 객체인 경우
const user = {
  name: "Kim",
  address: {
    zipCode: "03068",
    city: "Jeju",
  },
};

// address 프로퍼티 키로 객체 추출 -> 이 객체의 city 프로퍼티 키로 값을 추출
const {
  address: { city },
} = user;
console.log(city); // Jeju
```

#### Rest 프로퍼티

Rest 파라미터나 Rest 요소와 유사하게 **Rest 프로퍼티** `...`를 사용할 수 있다.  
마찬가지로 반드시 마지막에 위치해야 한다.

```js
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1, { y: 2, z: 3};
```
