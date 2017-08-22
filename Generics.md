# Generics

## 제네릭이란?
제네릭은 함수나 클래스등을 선언할 떄 타입을 확정짓지 않고 함수를 호출하거나 인스턴스를 생성하는 시점에서 타입을 확정짓는 기능인다.

아래 함수는 string 타입의 매개변수를 받아 string 타입을 리턴한다.
```TypeScript
function f(arg: string): string {
  return arg;
}
```

이를 제네릭 함수로 다음과 같이 표현할 수 있다.
```TypeScript
function f<T>(arg: T): T {
  return arg;
}
```

위에서 보면 함수 이름 뒤에 <T> 가 추가된 것을 볼수 있다. <T> 를 타입매개변수라고 한다.
함수이름 뒤에 타입변수가 추가되면 함수안에서는 타입매개변수를 타입대신에 사용할 수가 있다.

타입매개변수는 제네릭 함수가 호출될 때 실제 타입으로 바꾸어 줘야 한다.
```TypeScript
let ret: string = f<string>("hello world");
```

제네릭을 사용하게 되면 함수를 호출할 때 원하는 타입으로 바꿀 수 있는 유연함을 제공한다.
```TypeScript
let ret: number = f<number>(100);
```
## any 와 제네릭이 다른점
그런데 any 타입을 쓰면 제네릭과 마찬가지로 어떤 타입이던지 사용할 수 있기때문에 둘이 유사해 보인다.
```TypeScript
function f(arg: any): any {
    return arg;
}

//any 타입이기 때문에 어떤 타입이던 사용가능하다.
let retNum: number = f(100);
let retString: string = f("hello world");
```

제네릭과 any가 다른 부분은 제네릭이 좀 더 타입을 엄격하게 적용한다는 것이다.
예를 들면 any를 사용한 아래 코드는 아무 이상없이 컴파일 된다.
```TypeScript
function f(arg: any): any {
  return arg;
}

let ret: number = f("hello world");
```
하지만 함수의 리턴값을 할당받는 변수 ret 은 number 를 기대하고 있으나 실제 할당받게 되는 것은 문자열이다.

제네릭을 사용할 경우 위와 같은 사용은 컴파일시 에러를 발생시킨다.
```TypeScript
function f<T>(arg: T): T {
  return arg;
}

let ret: number = f<number>("hello world"); //<== 컴파일 에러 발생!!
```
결국 제네릭은 any와는 다르게 함수 호출시 타입을 확정함으로써 잘못 사용된 타입 오류를 컴파일에러로 막아준다.

## 제네릭은 언제 필요할까?
제네릭은 타입에 대한 유연성을 제공하여 코드의 재사용성을 높여준다. 
예를 들어 두 배열을 매개변수로 받은 뒤, 배열들의 길이를 합하여 반환하는 함수 f1이 있다.
```TypeScript
function f1(arg1: number[], arg2: number[]): number {
  return (arg1.length + arg2.length);
}
```
만약 숫자를 담고 있는 배열이 아닌 문자열을 담고 있는 배열의 합을 구하고 싶다면 함수 f2를 새로 선언해 주어야 한다.
```TypeScript
function f2(arg1: string[], arg2: string[]): number {
  return (arg1.length + arg2.length);
}
```
숫자가 담긴 배열과 문자열이 담긴 배열의 합을 구하고 싶은 경우에도 함수 f3를 새로 선언해 주어야 한다.
```TypeScript
function f3(arg1: number[], arg2: string[]): number {
  return (arg1.length + arg2.length);
}
```
하지만 f1, f2, f3 모두 매개변수의 타입을 제외하면 배열의 길이의 합을 반환하는 기능은 동일하다. 
제네릭을 사용할 경우 세개의 함수를 각각 따로 선언할 필요없이 하나의 함수선언으로 모두 사용가능하다.

```TypeScript
function f<T, E>(arg1: Array<T>, arg2: Array<E>): number {
  return (arg1.length + arg2.length);
}

let len: number = f<number, number>([1,2,3,4,5], [6,7,8]);
let len: number = f<string, string>(['a','b','c','d','e'], ['f','g','h']);
let len: number = f<number, string>([1,2,3,4,5], ['f','g','h']);
```
## 제네릭 사용법

제네릭은 "함수/클래스/인터페이스" 에 사용할 수 있다. 기본적으로 타입매개변수(ie. <T>)를 선언부에 추가함으로 사용가능하며 호출하거나 인스턴스생성시에 타입매개변수를 실제 타입으로 바꿔줘야 한다.

### 제네릭 함수의 선언과 사용
```TypeScript
function f<T>(arg: T) {
  return arg;
}

let a: number = f<number>(100);
```

제네릭함수 호출시 타입을 명시하지 않아도 컴파일러가 사용된 값을 보고 추론해서 자동으로 적용해 주기도 한다.

```TypeScript
let a = f(100);
```

### 제네릭 클래스 선언과 사용
```TypeScript
class C<T> {
  private mVar: T;
  constructor(arg: T) {this.mVar = arg};
}

let c: C<string> = new C<string>("hello world"); 
```

### 제네릭 인터페이스 선언과 사용
```TypeScript
interface I<T> {
  member: T;
}

class intfImpl implements I<number> {
  public member: number;
  constructor(arg: number) {this.member = arg};
}

or

let c: I<number> = {member: number = 100;};
```

## 타입매개변수에 제약조건을 추가하는 방법
타입매개변수에 특정 조건들을 추가해야 할 필요가 있을 수도 있다. 예를 들면 타입매개변수가 치환될 타입은 반드시 length 를 프로퍼티로 포함하고 있어야 한다고 가정하자.
이 경우 extends 키워드를 이용해서 제약 조건을 interface에 추가한 뒤 타입매개변수가 이를 상속받도록 하면 된다.
```TypeScript
interface I{
  length: number;
}

function f<T>(arg: T extends I): number{
  return arg.length;
}

let param = {
  length: number = 10; 
  value: string = 'hello world';
};

f(param);

```
위와 같이 타입매개변수가 인터페이스를 상속받을 경우 제네릭함수 f의 매개변수는 반드시 length 를 프로퍼티로 포함하고 있어야 한다.
이렇게 특정 조건을 추가하고 싶을때는 인터페이스를 이용한다.

## 제네릭을 이용해서 타입체크하기

extends 키워드를 이용한 제약조건은 제네릭을 이용해서 타입체크를 할 수 있는 좋은 아이디어를 제공해 준다.

예를 들면 함수의 매개변수가 주어진 MAP 의 키값이어야만 한다는 조건을 체크하고 싶을 경우 제네릭을 이용해서 다음과 같이 구현할 수 있다.
```TypeScript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // a 는 x 의 키값이므로 문제가 없으나
getProperty(x, "m"); // m 은 x 의 키값이 아니므로 컴파일타임에서 오류가 발생한다.
```
위 경우 타입 매개변수 K 가 MAP 의 key 값이라는 조건을 상속받았기 때문에 K 는 반드시 T의 Key 값이어야만 한다.

특정 클래스를 상속받는 클래스만 인스턴스로 생성하는 함수를 구현하고 싶다면 다음과 같이 구현할 수 있다
```TypeScript
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}
createInstance(Lion).keeper.nametag;
```

위 예제는  Animal 클래스를 상속받는 클래스만 인스턴스로 생성하는 createInstance 함수를 구현한 예이다.타입매개변수 A 는 Animal 을 상속받도록 선언했기 때문에 createInstance 는 Bee 와 Lion 만 인스턴스로 생성한다.
