## Namespaces and Modules

### Introduce
이 글에서는 TypeScript에서 네임 스페이스와 모듈을 사용하여 코드를 구성하는 다양한 방법을 설명합니다.

### Using Namespaces
네임 스페이스는 글로벌 네임 스페이스에서 JavaScript 객체로 명명됩니다. 
여러 파일로 확장 할 수 있으며 --outFile 사용하여 연결할 수 있습니다.
(자세한 설명은 Namespace편 참조)

### Using Modules
네임 스페이스와 마찬가지로 모듈에는 코드와 선언이 모두 포함될 수 있습니다. 
주요 차이점은 모듈이 종속성을 선언 한다는 것입니다.

모듈은 모듈 로더 (예 : CommonJs / Require.js)에 종속됩니다. 
작은 JS 응용 프로그램의 경우 이는 최적이 아닐 수도 있지만 대규모 응용 프로그램의 경우 장기적인 모듈성 및 유지 관리 이점이 있습니다. 
(자세한 설명은 Module편 참조)

### Pitfalls of Namespaces and Modules
Namespace와 Module을 사용하는데 있어서 피해야할 것들에 대해 설명합니다.

1. /// <reference> - module 사용하기
/// <reference ... /> 구문 대신 import를 사용하자
ex) import x from "...";
ex) import x = require("...");

.myModules.d.ts
```js
// In a .d.ts file or .ts file that is not a module:
declare module "SomeModule" {
    export function fn(): string;
}
```
.myOtherModule.ts
```js
/// <reference path="myModules.d.ts" />
import * as m from "SomeModule";
```

### Needless Namespacing
```js
.shapes.ts
export namespace Shapes {
    export class Triangle { /* ... */ }
    export class Square { /* ... */ }
}
```

.shapeConsumer.ts
```js
import * as shapes from "./shapes";
let t = new shapes.Shapes.Triangle(); // shapes.Shapes?
```
모듈 파일 자체에서 가져오는 코드에 의해 정의 되므로 추가로 모듈레이어를 사용할 필요가 없다.

수정한다면...



.shapes.ts
```js
export class Triangle { /* ... */ }
export class Square { /* ... */ }
```

.shapeConsumer.ts
```js
import * as shapes from "./shapes";
let t = new shapes.Triangle();
```


