# 20. strict mode

## 20.1 strict mode 란?
```
function foo() {
  x = 10;
}
foo()
console.log(x) // 10
```
ReferenceError를 발생 시킬 것 같지만 자바스크립트는 암묵적으로 전역 객체에 x프로퍼티를 동적 생성한다. 이때 전역 객체의 x프로퍼티는 마치 전역 번수처럼 사용할 수 있다. 이러한 현상을 암묵적 전역(implicit global)이라 한다.
암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 크고, 오류를 줄여 안정적인 코드를 생산하기 위해 ES5부터 strict mode(엄격 모드)가 추가되었다.

## 20.2 strict mode 적용
스크립트 전체에 strict mode 적용.
```
'use strict';

function foo() {
  // ...
}
```

특정함수에 strict mode 적용
```
function foo() {
'use strict';
  // ...
}
```

코드 선두에서 'use strict';을 선언하지 않으면 제대로 동작❌
```
function foo() {
  // ...
'use strict';
  // ...
}
```

## 20.3 전역에 strict mode 적용은 피하자
strict mode 스크립트와 non-strict mode를 혼용하는것은 오류를 발생시킬 수 있다.
외부 서드파티 라이브러리가 non-strict mode인 경우도 있기 때문에 전역으로 사용하는 것은 바람직하지 않다!

이런경우 즉시 실행함수로 감싸서 스코프를 구분하여 사용.
```
(function () {
  'use strict';

  // ...
}())
```

## 20.4 함수단위로 strict mode를 적용하는 것도 피하자
모든 함수에 일일이 strict mode를 적용하거나, 외부의 컨텍스트에 strict mode가 적용되지 않았다면 문제가 발생할 수 있다.
strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## 20.5 strict mode가 발생하는 에러
### 20.5.1 암묵적 전역
```
(function(){
  'use strict';

  x = 1;
  console.log(x) // ReferenceError: x is not defined
}())
```
선언하지 않은 변수를 참조하면 ReferenceError 발생.

### 20.5.2 변수, 함수, 매개변수의 삭제
```
(function(){
  'use strict';

  var x = 1;
  delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
  }

  delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
}())
```
delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생.

### 20.5.3 매개변수 이름의 중복
```
(function(){
  'use strict';

  function foo(x,x) { // SyntaxError: Duplicate parameter name not allowed in this context
    return x+x;
  }
}())
```
중복된 매개변수 이름을 사용하면 SyntaxError 발생.

### 20.5.3 with 문의 사용
`with` 문을 사용하면 SyntaxError 발생.
```
(function(){
  'use strict';

  with({ x: 1}) { // SyntaxError: Strict mode code may not include a with statement
    console.log(x)
  }
}())
```
`with` 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있음.<br>
but, 성능과 가독성이 나빠지기 때문에 사용❌

## 20.6 strict mode 적용에 의한 변화
### 20.6.1 일반함수의 this
함수를 일반 함수로서 호출하면 this에 `undefined`가 바인딩된다.
```
(function(){
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
}())
```
생성자 함수가 아닌 일반 함수 내부에서는 `this`를 사용할 필요가 없기 때문.

### 20.6.2 arguments 객체
매개변수에 전달 된 인수를 재할당해도 arguments 객체에 반영되지 않는다.
```
(function(x){
  'use strict';

  // 매개변수에 전달된 인수를 재 할당
  x = 10;

  // 변경된 인수가 arguments 객체에 반영 되지 않음
  console.log(arguments) // {0: 7, lenght: 1}
}(7))
```
