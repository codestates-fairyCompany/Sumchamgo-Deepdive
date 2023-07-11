### **심벌이란?**

심벌은 ES6 에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다. 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서, 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

### **심벌 값의 생성** 

▶ Symbol 함수   
심벌 값은 Symbol 함수를 호출하여 생성한다. 이 때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.

```
const mySymbol = Symbol();

// 심볼 값은 외부로 노출되지 않는다
console.log(mySymbol); // Symbol()
```

다른 생성자 함수 Object(), Array(), Date() 등 과 다르게 new 연산자와 함께 호출하지 않는다.

선택적으로 문자열을 인수로 전달하지만, 심벌 값에 대한 설명(description) 이나 디버깅 용도로만 사용한다. => 설명 인자가 같더라도,생성된 심볼은 유일무이한 값이다.

```
const s1 = Symbol('mySymbol');
const s2 = Symbol('mySymbol');

console.log(s1 === s2); // false
```

심볼 값도 객체처럼 접근 시, 암묵적으로 래퍼 객체를 생성한다.

```
const symbol = Symbol('key');
console.log(typeof symbol); // symbol

// symbol의 래퍼 객체 생성
const wrapper = Object(symbol);
console.log(typeof wrapper); // object

// 심벌의 래퍼객체인지 확인
console.log(wrapper instanceof Symbol); // true
console.log(wrapper.valueOf() === symbol); //true

// 래퍼 객체에 키값 생성
wrapper.name = 'kyo';
console.log(wrapper.name);  // kyo
console.log(symbol.name); // undefined
// 심볼 값 자체에 영향을 주지 않는다
```

**_☆ 래퍼 객체 ?_**

원시 데이터 타입을 객체처럼 동작하도록 래핑하는 객체 => 해당 원시 값에 대한 추가적 기능 제공 목적

```
const str = 'Hello';
const wrapper = new String(str);
console.log(typeof wrapper); // object
console.log(wrapper.valueOf()); // 'Hello'
// valueOf() 메서드는 특정 객체의 원시 값을 반환

console.log(wrapper); // String{'Hello'} 0:'H' 1:'e' 2:'l' ...
```

_**☆ ' new ' 키워드 ?**_

자바스크립트에서 생성자 함수를 호출하여 새로운 객체를 생성하는데 사용되는 키워드 => 생성자 함수를 객체 생성자로 동작하게 하여 새로운 객체를 반환

new 로 생성자 함수 호출 시

1\. 빈 객체 생성 : 이 새 객체는 생성자 함수에서 this 로 참조 가능

2\. 프로토타입 연결 : 새로 생성된 객체는 생성자 함수의 프로토타입에 정의된 속성과 메서드를 상속 받는다.

3\. 생성자 함수 호출 : 생성자 함수 실행, 생성자 함수 안의 this 키워드는 새로 생성된 객체를 가리킨다.

4\. 객체 반환 : 생성자 함수 내에서 명시적으로 다른 객체를 반환하는게 아니라면, new 로 생성된 객체 반환

```
function Person(name,age) {
 this.name = name;
 this.age = age;
};

const kyo = new Person('Kyo', 28);
console.log(kyo.name); // 'Kyo'
console.log(kyo.age); // 28
```

### **Symbol.for / Symbol.keyFor 메서드** 

▶ Symbol.for

동일한 키에 대해서 항상 동일한 심벌 값을 반환한다. => 동일한 심벌 값을 사용하고 싶을 때 활용!

```
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

▶ Symbol.keyFor

전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출한다.

```
const s1 = Symbol.for('mySymbol');
symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('newSymbol');
symbol.keyFor(s2); // undefined
```

### **심벌과 상수**

```
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있다.
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

이 경우 변경/중복 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```
/ 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

자바스크립트에서는 다른 언어의 enum 을 흉내내어 사용하려면 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용한다. => 16장 객체 동결

```
// JavaScript enum
// Direction 객체는 불변 객체이며 프로퍼티 값은 유일무이한 값이다.
const Direction = Object.freeze({
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
});

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

### **심벌과 프로퍼티 키**

객체의 프로퍼티 키는 빈 문자열을 포함해 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성할 수도 있다.

심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다. 프로퍼티에 접근할 때도 대괄호를 사용한다.

```
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // 1
```

### **심벌과 프로퍼티 은닉** 

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다. 따라서 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

하지만 ES6에 추가된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]
```

**_☆ Object.keys() vs Object.getOwnPropertyNames()_**

Object.keys() 는 객체의 열거가능한 속성들의 이름으로 구성된 배열 반환 => \`for...in\` 루프를 통해 열거될 수 있는 속성

객체 자체에 속한 속성들만 반환 // 프로토타입 체인 상의 속성은 포함 x

Object.getOwnPropertyNames() 는 객체의 모든 속성들의 이름으로 구성된 배열 반환 // 열거 가능 속성, 열거 불가능 속성 모두 포함

```
const obj = {
  a: 1,
  b: 2
};

// 객체에 c 키값을 추가해 열거 불가 속성 지정
Object.defineProperty(obj, 'c', {
  value: 3,
  enumerable: false
});

console.log(Object.keys(obj)); // ['a', 'b']
console.log(Object.getOwnPropertyNames(obj)); // ['a', 'b', 'c']
```

### **심벌과 표준 빌트인 객체 확장** 

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.

```
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].sum(); // 3
```

하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 앞으로도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

```
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // 3
```

### **Well-known Symbol**

자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.

[##_Image|kage@sq9yP/btsm8MS1h7x/56tvL2tUVmzigPgMZyMG4k/img.png|CDM|1.3|{"originWidth":834,"originHeight":416,"style":"alignCenter"}_##]

이와 같이 기본 제공하는 빌트인 심벌 값을  ECMAScript 사양에서는 Well-known Symbol 이라 부른다.

예를 들어 Array, String, Map, Set, TypedArray, NodeList, HTMLCollection 과 같이 for ... of 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol 인 Symbol.iterator를 키로 갖는 메서드를 가지며 Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.

```
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      }
    };
  }
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```
