# null과 undefined
- 값이 대입되지 않은 변수 혹은 속성을 사용하려고 하면 undefined를 반환
- null은 '객체가 없음'을 나타낸다.

```js
typeof null // 'object'
typeof undefined // 'undefined'
```

- 변수를 선언한 적이 있는지 확인하고 싶을 때에도 typeof 연산자를 사용하고,
- 이 때, 변수를 선언한 적이 없다면 'undefined'가 반환된다.


- 저장을 한 적이 없는지와 내가 '없음'이라는 사실을 저장했는지를 구분하기 위해서는 undefined를 저장하면 구분이 안되므로, <br>**명시적으로(확 드러나게) '없음'을 나타내고 싶다면 항상 null을 사용하는 것**이 좋다. (법칙은 아니고 관례임. )

- 다만, 객체를 사용할 때 어떤 속성의 부재를 null을 통해서 나타내는 쪽보다는, 그냥 그 속성을 정의하지 않는 방식이 간편하므로 더 널리 사용된다. 

```js
// 이렇게 하는 경우는 많지 않습니다.
{
  name: 'Seungha',
  address: null
}

// 그냥 이렇게 하는 경우가 많습니다.
{
  name: 'Seungha'
}

// 어쨌든 이렇게 하지는 말아주세요.
{
  name: 'Seungha',
  address: undefined
}
```

## Null Check
strict equality`===` 엄밀한 비교. 
abstract equality`==` 추상적인 비교.

*  null check를 할 때는 `==`를 쓰는 것이 편리하다. 
```js
null === undefined; // false

null == undefined;  // true
null == null; // true

undefined == null // true
undefined == undefined // true


null == 1       // false
null == 'hello' // false
null == false   // false

undefined == 1       // false
undefined == 'hello' // false
undefined == false   // false
```
- 즉, == 연산자는 한 쪽 피연산자에 null 혹은 undefined가 오면, 다른 쪽 피연산자에 null 혹은 undefined가 왔을 때만 true를 반환하고, 다른 모든 경우에 false를 반환한다.

- 따라서 null check를 할 때 만큼은 ==를 사용하는 것이 편합니다. 다른 모든 경우에는 ===를 사용하는 것이 좋다.


# 함수


## 함수의 구성 요소
### 매개변수와 인수
- 함수를 호출할 때 인수 자리에 변수를 써주면, 이 변수가 넘어가는 게 아니라 값이 넘어가는 것임. 이 표현식의 값이 넘어가는 것이지, 변수 자체가 넘어가는 게 X.

- 매개변수에는 재대입이 가능하다.(let으로 선언한 변수와는 미묘하게 다른 점이 있음.)

### 반환값
- return 키워드 바로 다음에 오는 값이 함수 호출의 결과값으로 반환되며, 반환되는 즉시 함수 실행이 종료된다.
- 즉, return 다음에 오는 코드는 실행되지 않는다.

- return 뒤에 아무 값도 써주지 않거나, 아예 return 구문을 쓰지 않으면 함수는 undefined를 반환한다.


## 스코프 (Scope)
- 매개변수와 같이 함수 안에서 정의된 변수는 함수 바깥에서는 접근할 수 없기 때문에 함수 안에서만 사용할 수 있다. 
- 즉, 변수는 코드의 일정 범위 안에서만 유효하다는 성질이 있다.
- **특정 변수가 유효한 코드 상의 유효 범위를 가지고 `스코프(scope)`라고 합니다.**

- 매개변수는  함수 스코프를 갖습니다. 즉, 함수의 중괄호 안에서만 유효하다.


### 스코프 연쇄 (Scope Chain)
- 중첩된 스코프 안에서는 바깥 스코프의 변수를 가져다 쓸 수 있다. 
- 코드의 실행 흐름이 변수 이름에 도달하면, 그 변수와 같은 이름을 갖는 변수를 현재 스포크에서 찾아보고, 만약 없으면 바로 바깥쪽, 없으면 바같쪽 스코프로 올라가서 계속 찾아보는 과정이 되풀이 된다.

- 바깥 스코프에서 찾을 때는 부모-부모-부모 ...를 찾는 것이지, 다른 함수 안에서만 쓰인 변수를 가져다 쓸 수는 X.
- 가장 바깥에 있는 스코프를 최상위 스코프(top-level scope) 혹은 전역 스코프(global scope)라고 부른다.
```js
const five = 5;
function add5(x) {
  function add(y) {
    return x + y;
  }
  return add(five);
}
add5(3); // 8
```
- reference Error같은 게 뜨면, 내 변수가 스코프 안에서 잘 들어있는지 확인할 것!

### 변수 가리기 (Variable Shadowing)
- 안쪽 스코프의 변수가 바깥쪽 스코프에 있는 변수를 가리는 현상을 말한다.
- 바깥 스코프와 상관없이 매개변수를 자유롭게 사용할 수 있게끔 하는 성질이다.
- 바깥 스코프에 있는 변수를 일일히 기억하지 않고 사용하고 싶은 매개변수를 자유롭게 사용할 수 있도록 이런 성질이 존재한다. 
```js
const x = 3;

function add5(x) { // `x`라는 변수가 다시 정의됨
  function add(x, y) { // `x`라는 변수가 다시 정의됨
    return x + y;
  }
  return add(x, 5);
}

add5(x);
```

### 어휘적 스코핑 (Lexical Scoping)
- 스코프는 **코드가 작성된 구조**에 의해서 결정되는 것이지, **함수 호출의 형태**에 의해 결정되는 것이 아니다. 
```js
function add5(x) {
  const five = 5;
  return add(x);
}

add5(3); // 8

function add(x) {
  return five + x; // ReferenceError: five is not defined
}
```
- add라는 함수가 add5라는 함수 안에서 호출되었다고 해서, add 함수 내부에서 add5 함수의 스코프 안에 있는 변수에 접근할 수 있는 것은 아니다.
- 스코프는 코드가 작성된 구조에 의해 결정되는 성질이다. 위 코드를 동작시키려면, 아래와 같이 작성해야 한다.
```js
function add5(x) {
  const five = 5;
  function add(x) {
    return five + x;
  }
  return add(x);
}
```

## 값으로서의 함수
- JavaScript에서는 함수도 값이다.
- cf) filter는 함수를 인수로 넘겨줘야 사용할 수 있는 메소드.
    - 원본 배열을 변경시키지 X. 새 배열을 반환함.
- '일급 객체'라는 용어는 면접에 종종 나옴
- 자바스크립트의 객체는 일급 객체다. 
    - **조건: 변수나 데이터 구조안에 담을 수 있다.**
    - **파라미터로 전달 할 수 있다.**
    - **반환값(return value)으로 사용할 수 있다.**
     
    - 할당에 사용된 이름과 관계없이 구별이 가능하다
    - 동적으로 프로퍼티 할당이 가능하다
    cf) c나 java의 함수는 일급 함수가 아니다.


- url(https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)

## 익명 함수 (Anonymous Function)
- JavaScript에서 함수를 선언할 때 꼭 이름을 붙여주어야 하는 것은 아니다.
- 이름을 붙이지 않은 함수를 가지고 익명 함수(anonymous function)라고 한다.
- 익명 함수는 함수를 만든 쪽이 아니라 다른 쪽에서 그 함수를 호출할 때 많이 사용된다.


## 화살표 함수 (Arrow Function)
- function keyword 함수는 값을 반환하려면 반드시 return을 사용해야 한다. 
- 화살표 함수의 경우 중괄호가 없으면 바로 반환이 된다. 
- 코드의 길이나 표기법이 굉장히 간단해진다.
- 화살표 함수는 익명 함수 밖에 없다. 이름이 있는 화살표 함수는 없다.

```js
// 바로 반환시키지 않고 function 키워드를 통한 함수 정의처럼 여러 구문을 사용하려면 curly braces({...}) 로 둘러싸주어야 한다. 
//  `=>` 다음 부분을 **중괄호로 둘러싸면**, 명시적으로 `return` 하지 않는 한 아무것도 반환되지 않습니다.
const add = (x, y) => {
  const result = x + y;
  return result;
}

//매개변수가 하나밖에 없다면, 매개변수 부분의 괄호를 쓰지 않아도 된다. 
const negate = x => !x;
```

cf) function keyword 함수 vs 화살표 함수 비교
```js
[1, 2, 3, 4, 5].filter(function (x) {
  return x % 2 === 0;
}); // [2, 4]
```
```js
[1, 2, 3, 4, 5].filter(x => x % 2 === 0);
```

# 제어 구문

- `if...else` 구문에서 중괄호 내부에 있는 구문이 하나라면, 중괄호를 생략할 수 있으나 나중에 문장을 추가할 수도 있으니 항상 중괄호를 쓰는 습관을 들이자. 

- switch 바로 뒤의 괄호의 값: '코드 실행 여부를 판별할 기준이 되는 값', 
- 이 기준이 되는 값과 case 바로 뒤에 오는 값이 일치하면 콜론(:) 뒤의 코드 영역이 실행된다.
-**case쪽의 코드 영역 마지막에 break를 써주지 않으면, 해당 case가 실행될 때 바로 뒤의 case 코드 영역이 뒤이어 실행되게 된다. -> case문에 break를 꼭 써줘야 한다!!**
```js
function translateColor(english) {
  let result;
  switch (english) {
    case 'red':
      result = '빨강색';
      break;
    //  이 break를 생략할 경우, 'red'를 넣었을 때 ,result = '빨강색';후에  result = '파랑색';으로 실행흐름이 넘어가서 '파랑색'이 결과값으로 나온다. 
    case 'blue':
      result = '파랑색';
      break;
    case 'purple':
    case 'violet':
      // 이 코드 영역은 english 변수의 값이 'purple'일 때와 'violet'일 때 모두 실행됩니다.
      result = '보라색';
      break;
    default:
      result = '일치하는 색깔이 없습니다.';
  }
  return result;
}
```
## do...while 구문
- do...while 구문은 while 구문과 사용법은 크게 다르지 않으나, 내부 코드를 **무조건 한 번은 실행**시킨다는 차이점이 있다.
- 절대 true가 될 수 없는 구문을 무조건 한 번은 실행시킬 수 있다. 

### 배열의 순회
- 배열의 각 항목을 방문하면서 차례대로 도는 것을 말한다. 

* ` forEach`메소드 - 배열의 각 항목을 차례대로 실행시키고 싶을 때 사용하는 메소드(ES5에 추가된 문법임)
```js
const arr = [1, 2, 3, 4, 5];

arr.forEach((item, index) => {
  console.log(`배열의 ${index + 1} 번째 요소는 ${item} 입니다.`);
})
```
* for 구문의 종류
    - for
    - for...in 구문
    - for...of 구문


```js
const arr = [1, 2, 3, 4, 5];

for (let item of arr) {
  console.log(`현재 요소는 ${item} 입니다.`);
}
// 현재 요소는 1 입니다.
// 현재 요소는 2 입니다.
// 현재 요소는 3 입니다.
// 현재 요소는 4 입니다.
// 현재 요소는 5 입니다.

```
- arr의 요소(item)들이 순서대로 item이라는 변수에 들어가면서 차례대로 실행됨.

## `break`, `continue`
```js
alert('퀴즈를 시작합니다.');
while (true) {
  const answer = prompt('빨강의 보색은 무엇일까요?');
  if (answer === '초록') {
    alert('정답입니다! 🎉');
    break; // 루프를 종료하고 다음 코드로 넘어감
    // break를 만나면 while 구문 아예 바깥으로 빠져나오게 됨. 따라서 정답인
    // 초록을 입력했을 때는 정답입니다!를 출력하고 while문을 빠져나가서 alert('퀴즈가 끝났습니다.')로 실행흐름이 옮겨간다. 
  } else {
    alert('틀렸습니다! 다시 시도해보세요.');
  }
}
alert('퀴즈가 끝났습니다.');
```
- `continue`를 만나면, 나머지 코드를 실행하지 않고 루프의 처음으로 되돌아가는 효과가 있다.
- 반복문의 시작(조건문)으로 돌아가게 된다. 

- `break`는 `break`를 둘러싸고 있는 가장 가까운 루프만 종료시킨다.
- 가장 바깥의 루프까지 다 빠져나오는 게 X.

### 함수를 즉시 종료하기
- `return`과 `throw`는 **함수의 나머지 코드를 건너뛰고 함수를 즉시 종료시킨다**는 걸 기억하라!
- switch구문을 쓸 때는 break를 써야 하지만, return이 있으면 함수의 나머지 코드를 건너뛰고 함수를 즉시 종료시킨다.
- 함수의 나머지 코드를 건너뛰고 즉시 종료시키는 결과를 낳기 때문에 break를 더 추가해서 쓸 필요가 X. 

