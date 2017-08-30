# Interface

## Optional Properties
?는 optional 변수를 의미한다. 선언되지 않아도 문제없다.

```Typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
    let newSquare = {color: "white", area: 100};
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}

let mySquare = createSquare({color: "black"});
```

## Readonly properties
readonly는 읽기전용 변수이며 최초 선언 시 정의되고 interface member에만 사용 가능하다. 변수에는 const를 사용해야 한다.

```Typescript
interface Point {
  readonly x: number;
  readonly y: number;
  //error
  const z: number;
}

//ok
let point: Point = { x: 5, y: 2 };
//error
point.x = 1;
//ok
const b: number = 1;
//error
readonly a: number = 1;
```

## Excess Property Checks

Interface object 정의 시 타입을 체크한다. 이 때 정의되지 않는 타입을 선언하면 에러가 발생하지만, type assertion을 사용하거나 string index signature를 추가하면 에러를 방지할 수 있다.

```Typescript
interface LabelledValue {
    label: string;
    //[propName: string]: any;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
// ok
printLabel(myObj);
//error, if has "[propName: string]: any;" to LabelledValue, it's ok.
printLabel({ size: 10, label: "Size 10 Object" });
//ok
printLabel({ size: 10, label: "Size 10 Object" } as LabelledValue);
```

## Function Types

Interface 에 function을 선언 할 수 있는데, function의 parameter의 이름이나 타입은 변경/생략해도 상관없다.

```Typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;

// ok
mySearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
}

// ok
mySearch = function(src: string, sub: string): boolean {
    let result = src.search(sub);
    return result > -1;
}

// ok
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```
## Indexable Types

index signature를 사용하여 interface를 indexable type으로 만들 수 있습니다.
https://basarat.gitbooks.io/typescript/docs/types/index-signatures.html

```Typescript
////////////////////////////////
interface StringArray {
    [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
////////////////////////////////

////////////////////////////////
class Animal {
    name: string;
}
class Dog extends Animal {
    breed: string;
}

interface Okay {
    [x: string]: Dog;
}

let test: Okay = {
    'index': {
        name: "aa",
        breed: "aa"
    },
    'index2': {
        name: "bb",
        breed: "bb"
    }
}

console.log(test['index']);
// name: "aa", breed: "aa"
////////////////////////////////

////////////////////////////////
interface NumberDictionary {
    [index: string]: number;
    length: number;    // ok, length is a number
    name: string;      // error, the type of 'name' is not a subtype of the indexer
}
////////////////////////////////
```

## Class Types

생성자를 강제하고 싶을 때 객체를 생성하는 function을 제공하여 생성자를 강제 할 수 있다.

```Typescript
interface ClockConstructor {
    new (hour: number, minute: number);
}

class Clock implements ClockConstructor {
    currentTime: Date;
    // error. constructor is static side
    constructor(h: number, m: number) { }
}
/////////////////////////////////////////////

interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick();
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}
class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

## Extending Interfaces

Interface를 상속하여 확장할 수 있습니다.

```Typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

## Hybrid Types

Interface를 casting하여 사용할 수 있고, Interface를 마치 함수나 object처럼 사용할 수 있습니다.

```Typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

## Interfaces Extending Classes


interface가 class를 상속할 경우 class의 구현부를 제외한 member를 상속받을 수 있다.
즉, 아래 Button과 TextBox는 SelectableControl로 casting이 가능하면서 state 멤버를 사용할 수 있게된다.
Image와 Location은 Control의 state를 사용할 수 없다.

```Typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control {
    select() { }
}

class TextBox extends Control {
    select() { }
}

class Image {
    select() { }
}

class Location {
    select() { }
}
```
