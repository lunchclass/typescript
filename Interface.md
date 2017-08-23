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
const b: number = 1;//errorreadonly a: number = 1;
```

