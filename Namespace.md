## Namespace
(*typescript 1.5까지 사용하던 "External modules"(내부모듈)은 이제 namespace입니다)
(외부모듈은 이제 ECMAScript 2015 의 용어에 맞게 간단하게 "모듈"입니다.)
(즉, module X { 는 현재 선호되는 namespace X { 와 동등합니다)

### Validators in a single file

아래 예제는 단순한 문자열 유효성 검사기 입니다.
```js
interface StringValidator {
    isAcceptable(s: string): boolean;
}

let lettersRegexp = /^[A-Za-z]+$/;
let numberRegexp = /^[0-9]+$/;

class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
        return lettersRegexp.test(s);
    }
}

class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: StringValidator; } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters only"] = new LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        let isMatch = validators[name].isAcceptable(s);
        console.log(`'${ s }' ${ isMatch ? "matches" : "does not match" } '${ name }'.`);
    }
}
```
(* test(String) 검색한 문자열에 패턴이 있는지 여부)

해당 코드의 문제점이라고 한다면,
단순 패턴 확인을 위한 class와 interface가 나열되어 있고,
Reg. exp가 밖으로 노출되어 있다는 점입니다.

----------
우리가 더 많은 검증자를 추가함에 따라, 
우리는 우리의 유형을 추적하고 다른 객체와의 이름 충돌에 대해 걱정할 필요가 없도록 일종의 조직 체계를 원할 것입니다. 
전역 이름 공간에 다른 이름을 많이 쓰는 대신 개체를 namespace로 꾸며봅시다.

### Namespaced Validators
```js
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```
모든 유효성 검사기 관련 엔터티를 Validation 라는 namespace로 이동합니다. 
우리가 여기서 interface와 class를 namespace 외부에서 볼 수 있기를 원하기 때문에 export를 붙이고
반대로, lettersRegexp 및 numberRegexp 변수는 구현 세부 사항이므로 안보이는 상태로 유지되며 namespace 외부에 표시되지 않습니다. 
파일의 맨 아래에 있는 테스트 코드에서 namespace 외부에서 사용할 때 유형의 이름을 한정해야합니다 (예 : Validation.LettersOnlyValidator .

### Splitting Across Files

파일은 별개이지만 각각이 동일한 네임 스페이스에 기여할 수 있으며 모든 파일이 한 곳에서 정의 된 것처럼 사용할 수 있습니다. 

Validation.ts
```js
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }
}
```
LettersOnlyValidator.ts
```js
/// <reference path="Validation.ts" />
namespace Validation {
    const lettersRegexp = /^[A-Za-z]+$/;
    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }
}
```
ZipCodeValidator.ts
```js
/// <reference path="Validation.ts" />
namespace Validation {
    const numberRegexp = /^[0-9]+$/;
    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}
```
Test.ts
```js
/// <reference path="Validation.ts" />
/// <reference path="LettersOnlyValidator.ts" />
/// <reference path="ZipCodeValidator.ts" />

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(""" + s + "" " + (validators[name].isAcceptable(s) ? " matches " : " does not match ") + name);
    }
}
```

### Aliases
네임 스페이스 작업을 단순화 할 수있는 또 다른 방법은 import q = x.y.z 를 사용하여 일반적으로 사용되는 개체의 더 짧은 이름을 만드는 것입니다. 
```js
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons;
let sq = new polygons.Square(); // Same as 'new Shapes.Polygons.Square()'
```
