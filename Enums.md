# Enums
***
## Enums
* 명명 상수 집합(a set of named numeric constants) 정의
* enum 키워드로 정의
```js
enum Direction {
    Up = 1,
    Down,  //2,   Up + 1
    Left,  //3, Down + 1
    Right  //4, Left + 1
}
```

* 0 이상의 member로 구성
```js
enum Direction {

} // no problem
console.log(Direction); // ???
```

* member는 constant(상수) or computed(계산식)
* constant(상수)인 경우
  * 초기값 없을 경우 이전 member 값 + 1
  * 초기값 없이 첫 member
  ```js
  enum Direction {
    Up // 0
  }
  ```
* a constant enum expression(상수 열거식)인 경우
  * 숫자 리터럴
  * member간 참조
  * 괄호로 묶인 경우
  * +, -, ~ 단항 연산자 사용
  * +, -, *, /, %, <<, >>, >>> 이항 연산자 사용 (NaN or Inifinity로 계산시 compile error)


* 그 외에는 computed
```js
enum FileAccess {
    // constant members
    None,
    Read    = 1 << 1,
    Write   = 1 << 2,
    ReadWrite  = Read | Write,
    // computed member
    G = "123".length
}
```

* Enums are real objects for a reverse mapping
```js
enum Enum {
    A
}
let a = Enum.A;
let nameOfA = Enum[Enum.A]; // "A"
```

* never inlined, so 추가 생성 코드 및 indirect 비용을 줄이려면 enum 키워드 앞에 const를 붙여라
```js
const enum Enum {
    A
}
let a = Enum.A;
let nameOfA = Enum[Enum.A]; // Error : A const enum member can only be accessed using a string literal.
```
* const enum은 compile시 완전히 제거 됨 

```js
const enum Enum {
    A = 1,
    B = A * 2
}
let a = Enum.A;
//var a = 1 /* A */;

```
