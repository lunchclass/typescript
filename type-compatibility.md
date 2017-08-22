## Type Compatibility 타입 호환성
같은 선언문에 있거나 같은 타입 이름을 갖는 것

### Introduce

Java, C#과는 달리 멤버에게만 의존하여 유형을 연관시킨다.
(Person 클래스가 자체를 Named 인터페이스의 구현 자로 명시 적으로 설명하지 않으므로 오류가되지만 js는 가능.)
```js
interface Named {
    name: string;
}

class Person {
    name: string;
}

let p: Named;
// OK, because of structural typing
p = new Person();
```

### Starting out
```js
interface Named {
    name: string;
}

let x: Named;
// y's inferred type is { name: string; location: string; }
let y = { name: "Alice", location: "Seattle" };
x = y;
```
y가 x에 할당 될 수 있는지 없는지는 각 속성을 검사해서 호환 가능 여부 확인.
name이라는 멤버 변수가 있기 때문에 할당이 허용. location은 고려되지 않음.

### Comparing two functions
함수 또한 동일하다. 안의 매개 변수를 확인해서 할당 여부 체크.
```js
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // OK
x = y; // Error
```
매개 변수의 이름은 고려되지 않고 유형 만 고려해야 한다. 이 경우 x 모든 매개 변수는 y 상응하는 호환 가능한 매개 변수를 가지므로 할당이 허용.
두 번째 할당은 'x'에없는 필수 두 번째 매개 변수가 있으므로 할당이 허용되지 않으므로 오류임.

```js
let x = () => ({name: "Alice"});
let y = () => ({name: "Alice", location: "Seattle"});

x = y; // OK
y = x; // Error because x() lacks a location property
```
위 샘플은 반환 유형에 따라 다른 두 함수를 사용하여 반환 유형을 처리하는 방법.

### 왜 이런걸 허용할까?
JavaScript에서 추가 함수 매개 변수를 무시하는 것이 실제로 매우 일반적이기 때문이다. 
예를 들어 아래예제, Array#forEach 는 콜백 함수에 배열 요소, 인덱스 및 배열의 세 가지 매개 변수를 제공한다. 
그럼에도 불구하고 첫 번째 매개 변수 만 사용하는 콜백을 제공하는 것은 매우 유용하다.
```js
let items = [1, 2, 3];

// Don't force these extra parameters
items.forEach((item, index, array) => console.log(item));

// Should be OK!
items.forEach(item => console.log(item));
```


