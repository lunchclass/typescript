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
  * +, -, *, /, %, <<, >>, >>> 이항 연산자 사용


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
