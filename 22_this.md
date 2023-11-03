# 22. this
> [화살표 함수와 this 바인딩](https://velog.io/@padoling/JavaScript-화살표-함수와-this-바인딩)


## 22.1 this 키워드
this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**이다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다. this가 가리키는 값, this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.<br>
자바스크립트의 this는 함수가 호출되는 방식에 따라 this 바인딩이 동적으로 결정된다.
```
const person = {
  name: 'kim',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가르킨다
    console.log(this) // {name: 'kim', getName: ƒ}
    return this.name
  }
}

function Person(name) {
  this.name = name
  // 생성자 함수에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this) // Person {name: 'kim'}
}
const me = new Person('kim');
```

## 22.2 함수 호출 방식과 this 바인딩
this 바인딩은 함수가 어떻게 호출되었는지에 따라 동적으로 결정됨.<br>
> **렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.**<br>
함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 **함수 객체가 생성되는 시점에 상위 스코프를 결정.**<br>
this 바인등은 함수 호출 시점에 결정됨

**함수 호출 방식과 this 바인딩**

|함수 호출 방식|this 바인딩|
|---|---|
|일반 함수 호출|전역 객체|
|메서드 호출|메서드를 호출한 객체|
|생성자 함수 호출|생성자 함수가 생성할 인스턴스|
|Function.prototype.apply/call/bind 메서드에 의한 <br>간접 호출|Function.prototype.apply/call/bind 메서드의<br> 첫 번째 인수로 전달할 객체|

### 22.2.1 일반 함수 호출
기본적으로 전역 객체가 바인딩됨.
일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this는 전역 객체가 바인딩 된다. strict mode가 적용된 일반 함수 내부에 있는 this는 undefined가 바인딩.<br>

### 22.2.2 메서드 호출
메서드 내부의 this에는 메서드 호툴한 객체, 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.
```
const person = {
  name: 'kim',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    return this.name
  }
}

// 메서드 getName을 호출한 객체는 person
console.log(person.getName()) // kim
```
### 22.2.3 생성자 함수 호출
생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩 된다.

### 22.2.4 Function.prototype,apply/call/bind 메서드에 의한 간접 호출
`apply` `call` `bind` 메서드는 `Function.prototype`의 메서드이다. 이 메서드들은 모든 함수가 사용할 수 있으며 첫 번째 인수로 전달한 객체가 this 바인딩된다.<br>

`apply` `call` 메서드의 본질적인 기능은 함수를 호출하는것.
`apply`와 `call`의 차이점은 호출할 함수에 인수를 전달하는 방식만 다를 뿐 this로 사용할 객체를 전달하는 것은 동일.

`bind` 메서드는 `apply` `call` 와 달리 함수를 호출하지 않음.
첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성하여 반환한다.
```
const person = {
  name: 'kim',
  foo(callback) {
    setTimeout(callback, 100)
  }
}

person.foo(function () {
  // 일반 함수로 호출된 콜백 함수 내부의 this는 window를 가리킨다.
  console.log(`my name is ${this.name}.`) // my name is .
})


// bind() 적용
const person = {
  name: 'kim',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100)
  }
}

person.foo(function () {
  console.log(`my name is ${this.name}.`) // my name is kim.
})
```
