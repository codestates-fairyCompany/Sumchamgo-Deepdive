ES6에서 도입된 스프레드 문법 ( 전개 문법 ) 은 ' ... ' 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

스프레드 문법 사용 대상은 Array, String, Map, Set, DOM 컬렉션, arguments 와 같이 **for...of 문으로 순회할 수 있는 이터러블에 한정**된다.

```
console.log(...[1,2,3]); // 1 2 3
console.log(...'Hello'); // H e l l o

console.log(...new Map([['a','1'],['b','2']])); // ['a','1'] ['b','2']
console.log(...new Set([1,2,3])); // 1 2 3 // 인자로 이터러블만 가능

// 이터러블이 아닌 일반 객체는 스프레드 문법이 안됨
console.log(...{a:1,b:2});
// TypeError: Spread syntax requires ...iterable[Symbol.iterator] to be a function
```

스프레드 문법의 결과물은 값으로 사용할 수 없다.

why? 결과물이 순회하는 값들의 목록이기 때문

```
const list = ...[1,2,3]; // Uncaught SyntaxError: Unexpected token '...'
```

다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용 가능하다.

1\. 함수 호출문의 인수 목록

2\. 배열 리터럴의 요소 목록

3\. 객체 리터럴의 프로퍼티 목록

### **함수 호출문의 인수 목록에서 사용하는 경우** 

```
const arr = [1,2,3];

const max = Math.max(arr); // NaN

// Math.max 는 매개변수를 개수를 확정할 수 없는 가변 인자 함수
// 여러 개의 숫자를 인수로 받아야함

Math.max(1,2,3); // 3
const max = Math.max(...[1,2,3]); // 3
const max = Math.max(...arr) // 3
```

Rest 파라미터와 형태가 동일하지만, Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위함이다.

\=> 26장 Rest 파라미터

```
function qwer (...rest) {
  console.log(rest); // [1,2,3]
}
// 1,2,3 => [1,2,3];

qwer(...[1,2,3]);
// [1,2,3] => 1,2,3

// Rest <=> 스프레드
```

### **배열 리터럴 내부에서 사용하는 경우** 

**▶ concat**

ES5 에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우 배열 리터럴만으로는 해결할 수 없고, concat 메서드를 사용해야한다.

스프레드를 사용하면 메서드없이 배열 리터럴만으로 결합 가능하다.

```
const arr = [1,2].concat([3,4]);
console.log(arr); // [1,2,3,4]

const nArr = [...[1,2], ...[3,4]];
console.log(nArr); // [1,2,3,4]
```

**▶ splice**

ES5 에서 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면 splice 메서드를 사용한다.

Array.prototype.splice(변경 시작위치, 제거 요소 개수, 배열에 추가할 요소)

이 때 splice 메서드의 세 번째 인자로 배열을 전달하면 배열 자체가 추가된다.

```
const arr1 = [1,4];
const arr2 = [2,3];

arr1.splice(1,0,arr2);

console.log(arr1); // [1,[2,3],4]
// [2,3] 을 해제해서 전달해야한다.
```

스프레드 문법을 사용하면 간단하다.

```
const arr1 = [1,4];
const arr2 = [2,3];

arr1.splice(1,0,...arr2);
console.log(arr1); // [1,2,3,4]
```

**▶ slice**

ES5 에서 배열을 복사하려면 slice 메서드를 사용한다.

```
const origin = [1,2];
const copy = origin.slice();

console.log(copy); //[1,2]
console.log(origin === copy); //false
```

```
const origin = [1,2];
const copy = [...origin];

console.log(copy); // [1,2]
console.log(origin === copy); // false
```

**▶ 이터러블을 배열로 변환**

```
// arguments 객체는 이터러블이면서 유사 배열 객체
function sum() {
  return [...arguments].reduce((pre,cur) => pre + cur, 0);
}

console.log(sum(1,2,3)); //6
```

```
// Rest 파라미터를 사용하는 것이 더 간결하다
const sum = (...args) => args.reduce((pre,cur) => pre + cur,0);

console.log(sum(1,2,3)); // 6
```

단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다.

```
const arrayLike = {
 0: 1,
 1: 2,
 2: 3,
 length : 3
};

const arr = [...arrayLike];
// Uncaught TypeError: arrayLike is not iterable

// 이 경우 Array.from 메서드를 사용해 이터러블로 바꿔보자
const iterA = Array.from(arrayLike)
console.log(iterA); // [1, 2, 3]
```

### **객체 리터럴 내부에서 사용하는 경우**

객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

대상은 이터러블이어야 하지만 스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다.

```
// 스프레드 프로퍼티
// 객체 복사 ( 얕은 복사 )
const obj = {x:1,y:2};
const copy = {...obj};

console.log(copy); // {x: 1, y: 2}
console.log(obj === copy); // false


// 객체 병합
const merged = {x:1,y:2,...{a:3,b:4}};
console.log(merged); // {x: 1, y: 2, a: 3, b: 4}
```
