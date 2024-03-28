# 25장

- 자바스크립트는 프로토타입 기반 객체지향 언어다.    
- 프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어다.    
- ES5에서는 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다. 
- 그러나 클래스 기반 언어에 익숙한 프로그래머들은 자바스크립트를 어렵게 느꼈고,   
ES6에 클래스를 도입해 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 객체 생성 메커니즘을 제시했다. 

#### 클래스 vs 생성자 함수     
- 유사점 : 프로토타입 기반의 객체지향을 구현함   
- 차이점 
  1. 클래스를 new 연산자 없이 호출하면 에러가 발생한다.   
     생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
  2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공하지만, 생성자 함수는 제공하지 않는다. 
  3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다.   
    함수 선언문으로 정의된 생성자 함수는 함수 호이스팅, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.   
  4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 해제할 수 없다.   
     생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
  5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 


## 25.2 클래스 정의
일반적으로 파스칼 케이스를 사용한다. 
```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

클래스는 함수로 평가되며, 일급 객체다.    

클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드 세 가지가 있다. 

```js
// 클래스 선언문 
class Person {
    // 생성자 
    constructor(name) {
      // 인스턴스 생성 및 초기화 
        this.name = name;
    }

    // 프로토타입 메서드
    sayHi(){
        console.log(`Hi! My name is ${this.name}`);
    }

    // 정적 메서드 
    static sayHello() {
        console.log('Hello!');
    }
}

// 인스턴스 생성 
const me = new Person('Kim');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Kim
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Kim
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

## 25.3 클래스 호이스팅 
```js
// 클래스 선언문
class Person {}

console.log(typeof Person); // function 
```
클래스 선언문으로 정의한 클래스는 런타임 이전에 먼저 평가되어 함수 객체를 평가한다.   
이때 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor다.   

클래스는 클래스 정의 이전에 참조할 수 없다. 
```js
console.log(Person); // ReferenceError..
class Person {};
```

클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않다.
```js
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다. 
  console.log(Person); // ReferenceError: Cannot access 'Person' before initialization ...

  // 클래스 선언문
  class Person {}
}
```
클래스는 let, const 키워드로 선언한 변수처럼 호이스팅된다. 따라서 클래스 선언문 이전에 TDZ에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다. (p.213 참고)

## 25.4 인스턴스 생성 

클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.   
```js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```
클래스는 인스턴스를 생성하기 위해 존재하는 것이기 때문에 반드시 new 연산자와 함께 호출해야 한다.
```js
class Person {}

// 인스턴스 생성
const me = Person(); // TypeError...
```
클래스 표현식으로 정의된 클래스의 경우 클래스를 가리키는 식별자를 사용해 인스턴스를 생성한다.
```js
// 클래스를 가리키는 식별자: Person
const Person = class MyClass {};
const me = new Person(); // Correct! 

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자
console.log(MyClass);

const you = new MyClass(); // ReferenceError...
```
클래스 표현식에서 사용한 이름은 외부 코드에서 접근 불가능하다.    

## 25.5 메서드
클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세 가지가 있다.

### constructor 
인스턴스를 생성하고 초기화하기 위한 특수한 메서드.   
constructor는 이름을 변경할 수 없다. 
```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화 
    this.name = name;
  }
}
```
```js
// 클래스는 함수다.
console.log(typeof Person); // function
console.dir(Person);
```

클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있다. 함수처럼 프로토타입과 연결되어 있으며 자신의 스코프 체인을 구성한다. 


모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있다. 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미한다. 

```js
const me = new Person('Kim');
console.log(me);
```
Person 클래스의 constructor 내부에서 this에 추가한 name 프로퍼티가/ 클래스가 생성한 인스턴스의 프로퍼티로 추가된 것을 확인할 수 있다. constructor 내부에서 this에 추가한 프로퍼티는/ 인스턴스 프로퍼티가 된다. constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다.   


constructor는 클래스 내에 최대 한 개만 존재할 수 있다.
```js
class Person {
  constructor() {}
  constructor() {}
}
// SyntaxError...
```

constructor는 생략할 수 있다.
```js
class Person {
  // constructor를 생략하면 클래스에 빈 constructor가 암묵적으로 정의된다. 
}

// constructor를 생략한 클래스는 빈 constructor에 의해 빈 객체를 생성한다. 
const me = new Person();
console.log(me); // Person {}
```
프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.
```js
class Person {
    constructor() {
        this.name = 'Lee';
        this.address = 'Seoul';
    }
}
const me = new Person();
console.log(me);
```
인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다. 이때 초기값은 constructor의 매개변수에게 전달된다.
```js
class Person {
    constructor(name, address) {
        // 인수로 인스턴스 초기화 
        this.name = name;
        this.address = address;
    }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person('Kim', 'Jeju');
console.log(me); // Person {name: 'Kim', address: 'Jeju'}
```

constructor는 별도의 반환문을 갖지 않아야 한다. new 연산자와 함께 클래스가 호출되면 암묵적으로 this, 즉 인스턴스를 반환하기 때문이다. 


### 프로토타입 메서드   
- 프로토타입 메서드를 생성하기 위해서는 명시적으로 프로토타입에 메서드를 추가해야 함 
  ```js
  // 생성자 함수
  function Person() {
    this.name = name;
  }

  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  const me = new Person('Kim');
  me.sayHi();
  ```

- 클래스 몸체에서 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다. 
  ```js
  class Person {
    constructor(name) {
      this.name = name;
    }

    sayHi() {
      console.log(`Hi! My name is ${this.name}`);
    };
  }

  const me = new Person('Kim');
  me.sayHi();
  ```

### 정적 메서드
인스턴스를 생성하지 않아도 호출할 수 있는 메서드 
- 생성자 함수의 경우 명시적으로 생성자 함수에 메서드를 추가해야 함
  ```js
  function Person() {
      this.name = name;
  }

  // 정적 메서드
  Person.sayHi = function () {
    console.log('Hi');
  };

  // 정적 메서드 호출
  Person.sayHi(); // Hi!
  ```
- 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드가 된다.
  ```js
  class Person {
    constructor(name) {
      this.name = name;
    }

    // 정적 메서드 
    static sayHi() {
      console.log('Hi!');
    };
  }
  ```

### 클래스의 인스턴스 생성 과정
1. 인스턴스 생성과 this 바인딩   
  클래스를 호출하면 constructor가 실행되기 전 암묵적으로 빈 객체 생성됨   
  -> 클래스가 생성한 인스턴스 

2. 인스턴스 초기화   
  this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고,   
  constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다. 

3. 인스턴스 반환
클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다. 

```js
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;
  }

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환한다.
}

const me = new Person('Kim');
console.log(me);
```
