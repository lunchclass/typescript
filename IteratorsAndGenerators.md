# Iterators and Generators
***
## Iterables
* object가 Symbol.iterator 속성에 대한 구현이 있으면 itereable(순화가능)으로 간주
* Array, Map, Set, String, Int32Array, Uint32Array 등은 이미 구현됨
* Symbol.iterator 함수는 값 list를 리턴

### for..of statements
returns a list of keys on the object

### for ..in statements
returns a list of values of the numeric properties of the object

```js
let list = [4, 5, 6];

for (let i in list) {
   console.log(i); // "0", "1", "2",
}

for (let i of list) {
   console.log(i); // "4", "5", "6"
}
```

```js
let pets = new Set(["Cat", "Dog", "Hamster"]);
pets["species"] = "mammals";

for (let pet in pets) {
   console.log(pet); // "species"
}

for (let pet of pets) {
    console.log(pet); // "Cat", "Dog", "Hamster"
}
```
* 이 코드 좀 이상함.

### Code generation
* ES5와 ES3가 target일 때 Array type만 순회 가능
  * for..of loop은 for loop으로 바뀜
* ECMAScript 2015 이상 taget의 경우 for..of loop 코드 생성
