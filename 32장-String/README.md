# 32장 String

## String 생성자 함수

String 객체는 생성자 함수 객체로, new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.  
String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면, 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String();
console.log(strObj); // String {''}
```

String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면, 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String("Kim");
console.log(strObj); // String {'Kim'}
```

String 래퍼 객체는 length 프로퍼티와, 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다. -> 인덱스를 사용하여 각 문자에 접근할 수 있다.

```js
const strObj = new String("Kim");
console.log(strObj); // String {0: "K", 1: "i", 2: "m", length: 3}
console.log(strObj[0]); // K

// 문자열은 원시 값이므로 변경할 수 없다. (에러 발생 X)
strObj[0] = "S";
console.log(strObj); // Kim
```

String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
let strObj = new String(null);
console.log(strObj); // String {'null'}
```

new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다.

```js
// 숫자 타입 => 문자열 타입
String(1); // "1"

// 불리언 타입 => 문자열 타입
String(true); // "true"
```

## String 메서드

String 객체의 메서드는 언제나 새로운 문자열을 생성하여 반환한다. (문자열은 변경 불가능한 원시 값이기 때문)

### indexOf

대상 문자열에서 인수를 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.  
검색에 실패하면 -1을 반환한다.  
indexOf 메서드는 대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용하다.

```js
const str = "Hello World";

str.indexOf("l"); // 2
str.indexOf("or"); // 7
str.indexOf("x"); // -1
```

indexOf 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello World";

str.indexOf("l", 3); // 3
```

### search

대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.  
검색에 실패하면 -1 반환한다.

```js
const str = "Hello World";
str.search(/o/); // 4
```

### includes

대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인해 그 결과를 true 또는 false로 반환한다.

```js
const str = "Hello World";

str.includes("Hello"); // true
str.includes(""); // true
str.includes("x"); // true
str.includes(); // false
```

2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello World";

str.includes("l", 3); // true
```

### startsWith

대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 true 또는 false로 반환한다.  
2번째 인수로 검색을 시작할 인덱스를 전달한다.

```js
const str = "Hello World";

str.startsWith("He"); // true
str.startsWith("x"); // false
str.startsWith(" ", 5); // true
```

### endsWith

대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true 또는 false로 반환한다.  
2번째 인수로 검색할 문자열의 길이를 전달한다.

```js
const str = "Hello World";

str.endsWith("ld"); // true
str.endsWith("x"); // false
str.endsWith("lo", 5); // true
```

### charAt

대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.  
인덱스는 문자열의 범위(0 ~ 문자열 길이-1) 사이의 정수여야 한다. 범위를 벗어난 경우 빈 문자열을 반환한다.  
charAt 메서드와 유사한 문자열 메서드 : charCodeAt, codePointAt

```js
const str = "Hello World";

for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i)); // H e l l o  W o r l d
}

str.charAt(5); // ''
```

### substring

대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.  
두 번째 인수는 생략할 수 있다. 이때 첫 번째 인수로 전달한 인덱스에 위치하는 문자부터 마지막 문자까지 반환한다.

```js
const str = "Hello World";

str.substring(1, 4); // ell
str.substring(1); // ello World

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1); // ell

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-2); // Hello World

// 인수 > 문자열의 길이인 경우 인수는 문자열의 길이로 취급된다.
str.substring(1, 100); // ello World
str.substring(20); // ''
```

indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득한다.

```js
// 스페이스를 기준으로 앞에 있는 부분 문자열 취득
str.substring(0, str.indexOf(" ")); // Hello
// 스페이스를 기준으로 뒤에 있는 부분 문자열 취득
str.substring(str.indexOf(" ") + 1, str.length); // World
```

### slice

substring 메서드와 동일하게 동작한다.  
음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```js
const str = "Hello world";

str.substring(0, 5); // 'Hello'
str.slice(0, 5); // 'Hello'

str.substring(2); // 'llo world'
str.slice(2); // 'llo world'

str.substring(-5); // 'Hello world'
str.slice(-5); // 'world'
```

### toUpperCase

대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

```js
const str = "Hello world";

str.toUpperCase(); // 'HELLO WORLD'
```

### toLowerCase

대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

```js
str.toLowerCase(); // 'hello world'
```

### trim

대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```js
const str = "       foo         ";

str.trim(); // 'foo'
```

trimStart, trimEnd를 사용하면 대상 문자열 앞 or 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```js
const str = "       foo         ";

str.trimStart(); // 'foo         '
str.trimEnd(); // '       foo'
```

### repeat

대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.  
인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError 발생, 인수를 생략하면 기본값 0이 설정된다.

```js
const str = "abc";

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'abc'
str.repeat(2); // 'abcabc'
str.repeat(-1); // RangeError...
```

### replace

대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```js
const str = "Hello world";

str.replace("world", "Kim"); // Hello Kim
```

검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환한다.

```js
const str = "Hello world world";

str.replace("world", "Kim"); // Hello Kim World
```

특수한 교체 패턴을 사용할 수 있다.

```js
const str = "Hello world";

// $& => 검색된 문자열을 의미함
str.replace("world", "<strong>$&</strong>"); // 'Hello <strong>world</strong>'
```

첫 번째 인수로 정규 표현식을 전달할 수 있다.

```js
const str = "Hello Hello";

// 'hello'를 대소문자 구별하지 않고 전역 검색한다.
str.replace(/hello/gi, "Kim"); // 'Kim Kim'
```

두 번째 인수로 치환 함수를 전달할 수 있다. 첫 번째 인수로 전달한 문자열 또는 정규 표현식에 매치한 결과를 두 번째 인수로 전달한 치환 함수의 인수로 전달하면서 호출하고 치환 함수가 반환한 결과와 매치 결과를 반환한다.

```js
// 카멜 케이스를 스네이크 케이스로 변환하는 함수
function camelToSnake(camelCase) {
  // /.[A-Z]/g는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
  return camelCase.replace(/.[A-Z]/g, (match) => {
    console.log(match); // 'oW'
    return match[0] + "_" + match[1].toLowerCase();
  });
}

const camelCase = "helloWorld";
camelToSnake(camelCase); // 'hello_world'
```

### split

대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.  
인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

```js
const str = "How are you doing?";

// 공백으로 구분(단어로 구분)하여 배열로 반환한다.
str.split(" "); // ["How", "are", "you", "doing?"]

// \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다.
str.split(/\s/); // ["How", "are", "you", "doing?"]

// 인수로 빈 문자열을 전달하면 각 문자를 모두 분리한다.
str.split(""); // "H", "o", "w", " ", "a", "r", "e", " ", ...

// 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
str.split(); // ["How are you doing?"]
```

두 번째 인수로 배열의 길이를 지정할 수 있다.

```js
str.split(" ", 3); // ["How", "are", "you"]
```

reverse, join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

```js
function reverseString(str) {
  return str.split("").reverse("").join("");
}
reverseString("Hello World!"); // '!dlroW olleH'
```
