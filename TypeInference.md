
# Type Inference
### 기본
Typescript는 지정된 타입이 없을때 자동으로 타입을 유추해주는 ‘type inference’기능이 있습니다.

```
let x = 3;
```

위 코드에서 x는 number로 타입이 유추되어 let x:number로 자동설정 됩니다. 
이러한 추론은 변수나 속성을 초기화하고, 매개 변수 기본값을 설정하고, 함수 반환 유형을 결정할 때 (return) 발생합니다.

### Best Common Type
배열과 같이 여러가지 값의 타입에서 타입유추는 값마다의 타입중 모든 값에 맞는 (super type) 타입을 결정합니다. 
아래 한 배열의 예시코드의 Best common type 을 찾습니다. 

```
let x = [0, 1, null];
```

1) 맴버별 type 판단
  0: number, 1: number, null: null
2) 맴버별 type중 모든 값에 맞는 타입 판단
  number 타입과 null 타입중 0, 1, null 모든 값에 맞는 타입은 number!!

그러므로 Best common type 은 number

다음은 Best Common Type을 찾지 못하는 경우입니다.

```
class Animal{
    getCategory() {
        return 'Animal'
    }
}

class Rhino extends Animal{
    getName() {
        return 'Animal'
    }
    run() {

    }
}

class Elephant extends Animal{
    getName() {
        return 'Animal'
    }
    jump() {

    }
}

class Snake extends Animal{
    getName() {
        return 'Animal'
    }
    slide() {

    }
}

let zoo = [new Rhino(), new Elephant(), new Snake()];
```

이상적으로 우리는 zoo변수가 Animal[]로 추정되기를 원할수도 있지만 strictly Animal타입인 값이 없으므로 Animal[]로 추정되지 않습니다. 
이 문제를 해결하려면 Animal[] 타입을 명시적으로 제공 해야 합니다.

```
let zoo: Animal[] = [new Rhino(), new Elephant(), new Snake()];
```

Best common type을 찾지 못하면 타입은 union 타입((Rhino | Elephant | Snake)[])으로 설정됩니다.

```
let zoo = [new Rhino(), new Elephant(), new Snake(), new Animal()];
```

위와 같이 되어있을 경우에는 zoo변수는 Animal[]으로 타입이 지정됩니다.

### Contextual Type
타입 유추는 “the other direction”에서도 작동합니다. 즉, 좌항에 의해 우항의 타입이 유추될수 있습니다.

```
window.onmousedown = function(mouseEvent) {
    console.log(mouseEvent.buton);  //<- Error
};
```

위의 코드에서 Typescript type검사기는 Window.onmousedown함수 타입을 사용하여 우항의 함수식 타입을 추론하였습니다. 
그래서 mouseEvent매개변수 타입을 추론할수 있었고 오류를 발생시킬수 있었습니다.

### 플로우 & 타입 스크립트
자바스크립트의 정적 분석을 위해 사용되는 코드

플로우에서는 코드를 사용하는 부분에서 타입 에러를 검사한다. 
따라서 length 함수의 인자로 number나 boolean을 넣으면, number나 boolean이 x가 되기 적절한지, 
다시 말해 length 프로퍼티를 가지는지를 확인하기 때문에 잘못된 타입을 사용했다는 것을 알 수 있다.

function length(x) {
  let l;
  l = x.length;
  return l;
}

하지만 타입스크립트는 length 함수를 선언하는 순간 이 함수의 타입이 any -> any로 결정되기 때문에 
length 프로퍼티가 없는 값을 넘겨도 타입 에러인지 알 수 없다. 
이런 경우를 막기 위해 타입스크립트에서는 함수를 선언할 때 타입 어노테이션을 붙이는 것을 추천한다. 
즉, 위의 length 함수는 아래와 같이 선언하여 사용하는 것이 좋다.

function length(x: { length: number }): number {
  let l;
  l = x.length;
  return l;
}
