# 22. this
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

