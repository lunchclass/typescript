
# Classes

## 타입스크립트의 Class 와 Javascript(ES6) 의 클래스의 다른점
타입스크립트에서의 클래스는 ES6 부터 적용된 Class 와 많은 부분에서는 동일하나 선언적인 문법을 사용한다는 점과 접근제한자를 제공하는 면에서 다른 부분들이 존재한다.

먼저 선언적인 문법의 차이를 살펴보자.
```TypeScript
class C {
  public mNumber: number;
  public mString: string;
}

let c: C = new C();
c.number = 100;
c.name = "michael"; // <== 타입스크립트 컴파일 에러 발생
```
타입스크립트는 클래스선언시 정의되지 않은 프로퍼티나 맴버변수에 대해서는 컴파일에러를 발생시키나 Javascript 에서는 선언시 정의되지 않은 프로퍼티들에 대해서도 추가가 가능하여 위와 타입스크립트와 같은 에러가  발생하지 않는다.
