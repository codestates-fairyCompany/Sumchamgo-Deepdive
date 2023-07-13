### **내부 슬롯과 내부 메서드** 

**▶** 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드다.

ECMAScript 사양에 등장하는 이중 대괄호 (\[\[...\]\]) 로 감싼 이름들이 내부 슬롯과 내부 메서드이다.

JS엔진에서 실제로 동작하지만, 개발자가 직접 접근할 수 있도록 **외부로 공개된 객체의 프로퍼티는 아니다.**

단 일부 슬롯과 내부 슬롯에 한하여 간접적으로 접근할 수 있는 수단을 제공한다.

모든 객체는 \[\[Prototype\]\] 이라는 내부 슬롯을 갖는다.

내부 슬롯에 직접적으로 접근이 불가능하지만, \_\_proto\_\_ 를 통해 간접적으로 접근이 가능하다.

```
const o = {};

o.[[Prototype]]; // 접근불가

o.__proto__ // Object.prototype 접근가능
```

### **프로퍼티 어트리뷰트 & 프로퍼티 디스크립터 객체**

자바스크립트 엔진은 **프로퍼티를 생성할 때** **프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값**으로 자동 정의한다.

프로퍼티의 상태는 **프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)**를 말한다.

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯 \[\[Value\]\], \[\[Writable\]\], \[\[Enumerable\]\], \[\[Configurable\]\] 이다. 내부 슬롯으로 자바스크립트 내부 로직이므로 직접 접근이 안되지만,  
**Object.getOwnPropertyDescriptor** 메서드를 사용하여 간접적으로 확인할 수는 있다.  
이 메서드를 활용해 제공된 프로퍼티 어트리뷰트 정보 객체를 **프로퍼티 디스크립터 객체**라 한다.

```
const person = {
  name : 'Kyo',
  age : 28
};

console.log(Object.getOwnPropertyDescriptor(person, 'name'));
//{value: 'Kyo', writable: true, enumerable: true, configurable: true}

// ES8 에 추가된 메서드 Object.getOwnPropertyDescriptors 는 모든 프로퍼티 어트리뷰트 정보 제공
console.log(Object.getOwnPropertyDescriptors(person);
// {name: {…}, age: {…}}
// age:{value: 28, writable: true, enumerable: true, configurable: true}
// name:{value: 'Kyo', writable: true, enumerable: true, configurable: true}
```

**[※ Object.getOwnPropertyDescriptor(obj,props)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)**

obj : 속성을 찾을 대상 객체

props : 설명이 검색될 속성명

ES5에서, 이 메서드의 첫 번째 인수가 비객체(원시형)인 경우, 그러면 TypeError가 발생한다.  
ES6에서, 비객체 첫 번째 인수는 먼저 객체로 강제된다.

```
const arr = ['hello','안녕'];
Object.getOwnPropertyDescriptor(arr, 1);

// TypeError: "foo"는 객체가 아닙니다  // ES5 코드

Object.getOwnPropertyDescriptor(arr, 1);  // 두번째 인자는 객체의 키 대신 인덱스
// {value: '안녕', writable: true, enumerable: true, configurable: true}  // ES6 코드


const str = 'abcde';
console.log(Object.getOwnPropertyDescriptor(str, 3));
// {value: 'd', writable: false, enumerable: true, configurable: false}
```

### **데이터 프로퍼티와 접근자 프로퍼티**

▶ 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.

- 데이터 프로퍼티 :

키와 값으로 구성된 일반적인 프로퍼티

- 접근자 프로퍼티 :

자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

**▶ 데이터 프로퍼티**

1\. \[\[Value\]\]

프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값

프로퍼티 키를 통해 변경 시 값 재할당 가능, 프로퍼티가 없다면 동적 생성하고 생성된 프로퍼티의 \[\[Value\]\] 에 값 저장

2\. \[\[Writable\]\]

프로퍼티 값의 변경 가능 여부

false 값인 경우 \[\[Value\]\] 의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다.

3\. \[\[Enumerable\]\]

프로퍼티의 열거 가능 여부

false 값인 경우 for...in 문이나 Object.keys 메서드 등으로 열거할 수 없다  => 19장 ) 프로퍼티 열거

4\. \[\[Configurable\]\]

프로퍼티의 재정의 가능 여부

false 값인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경 금지

단, \[\[Writable\]\] 이 true 라면 \[\[Value\]\] 의 변경과 \[\[Writable\]\] 을 false 로 변경하는 것은 허용

**▶ 접근자 프로퍼티**

1\. \[\[Get\]\]

접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 **읽을 때** 호출되는 접근자 함수 => **getter 함수**

접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 \[\[Get\]\] 이 호출되고 그 결과가 프로퍼티 값으로 반환

2\. \[\[Set\]\]

접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 **저장할 때** 호출되는 접근자 함수 => **setter 함수**

접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 \[\[Set\]\] 이 호출되고 그 결과가 프로퍼티 값으로 저장

3\. \[\[Enumerable\]\]

4\. \[\[Configurable\]\]

```
const person = {
  // 데이터 프로퍼티
  firstName : 'Sangkyo',
  lastName : 'Kim',

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티
  // getter 함수
  get fullName(){
     return `${this.firstName} ${this.lastName}`;
  },

  // setter 함수
  set fullName(name){
     [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + ' ' + person.lastName); // Sangkyo Kim

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName 값을 저장하면 setter 함수 호출
person.fullName = 'Rany Kyo';

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수 호출
console.log(person.fullName); // Rani Kyo

// firstName 은 데이터 프로퍼티
let descriptor = Object.getOwnPropertyDescriptor(person,'fisrtName');
console.log(descriptor);
// {value: 'Rani', writable: true, enumerable: true, configurable: true}

// fullName 은 접근자 프로퍼티
descriptor = Object.getOwnPropertyDescriptor(person,'fullName');
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

접근자 프로퍼티와 데이터 프로퍼티를 구별하는 방법

```
// 일반 객체의 __proto__ 는 접근자 프로퍼티이다.
Object.getOwnPropertyDescriptor(Object.prototype,'__proto__');
// {enumerable: false, configurable: true, get: ƒ, set: ƒ}

// 함수 객체의 prototype 은 데이터 프로퍼티이다
Object.getOwnPropertyDescriptor(function(){},'prototype');
// {value: {…}, writable: true, enumerable: false, configurable: false}
```

### **프로퍼티 정의**

프로퍼티 어트리뷰트를 정의 또는 재정의 하는 것

Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다.

인자는 (객체의 참조, 데이터 프로퍼티 키 문자열, 프로퍼티 디스크립터 객체) 를 전달한다.

```
const person = {} ;

// 데이터 프로퍼티 정의
Object.defineProperty(person,'firstName', {
  value: 'Sangkyo',
  writable: true,
  enumerable: true,
  configurable: true,
});

// 디스크립트 객체의 프로퍼티를 일부 생략 가능하다
Object.defineProperty(person,'lastName', {
  value: 'Kim'
});

let descriptor = Object.getOwnPropertyDescriptor(person,'firstName');
console.log(descriptor);
// {value: 'Sangkyo', writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person,'lastName');
console.log(descriptor);
// {value: 'Kim', writable: false, enumerable: false, configurable: false}
// 이와같이 생략한 프로퍼티들은 기본 false 로 설정되어
// 지금 상태에선 수정, 삭제, 열거가 불가능하다 // 또한 프로퍼티도 재정의 불가 (configurable)


// 접근자 프로퍼티 정의
Object.defineProperty(person,'fullName',{
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  set(name) {
    [this.firstName,this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true,
});


// 프로퍼티 디스크립터 객체의 프로퍼티 생략 시 기본값
// value: undefined
// get: undefined
// set: undefined
// writable: false
// enumerable: false
// configurable: false
```

Object.defineProperties 메서드로 여러 개의 프로퍼티 한 번에 정의 가능

```
Object.defineProperties(person, {
  firstName: {
  value: 'Sangkyo',
  writable: true,
  enumerable: true,
  configurable: true,
  },
  lastName: {
  value: 'Kim',
  writable: true,
  enumerable: true,
  configurable: true,
  }
  // 접근자 프로퍼티도 함께 가능
});
```

### **객체 변경 방지** 

객체는 변경 가능한 값으로 재할당 없이 직접 변경할 수 있다.

프로퍼티 추가, 삭제, 갱신, 프로퍼티 어트리뷰트도 위의 메서드를 활용해 재정의 가능

이러한 객체의 변경을 방지하는 메서드들을 제공하는데, 금지하는 강도가 각각 다르다

| 구분            | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의  |
| --------------- | ------------------------ | ------------- | ------------- | ---------------- | ---------------- | --------------------------- |
| 객체 확장 금지  | Object.preventExtensions | X             | O             | O                | O                | O                           |
| 객체 밀봉       | Object.seal              | X             | X             | O                | O                | X                           |
| 객체 동결       | Object.freeze            | X             | X             | O                | X                | X                           |

▶ **객체 확장 금지 Object.preventExtensions**

프로퍼티의 추가만 금지된다 ( 동적 추가, defineProperty 추가 금지)

Object.isExtensible 메서드를 통해 확장 가능한지 boolean 을 확인할 수 있다.

▶ **객체 밀봉 Object.seal**

프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의를 금지한다. 밀봉되면 읽기와 쓰기, 프로퍼티 값 갱신만 가능하다.

Object.isSealed 메서드로 밀봉되었는지 확인 가능하다. 밀봉되면 configurable 이 false 가 된다.

▶ **객체 동결 Object.freeze**

객체를 동결 시켜, 프로퍼티 추가 및 삭제, 값 갱신, 어트리뷰트 재정의 전부 금지하여 읽기만 가능한 상태이다.

Object.isFrozen 메서드로 동결된 객체인지 확인 가능하다. 동결되면 writable, configurable 이 false 이다.

### **불변 객체** 

위에 정리한 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고, 중첩 객체까지는 영향을 주지못한다.

```
const person = {
  name: 'Kyo',
  address: {city: 'Seoul'}
};

// 얕은 객체 동결
Object.freeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩객체는 동결안됨
console.log(Object.isFrozen(address)); // false

person.address.city = 'Suwon';
console.log(person); // {name:'Kyo',address:{city:'Suwon'}};

// 따라서 중첩객체 동결을 위해 재귀적 객체동결이 필요하다

function deepFreeze(target){
  if(target && typeof target === 'object' && !Object.isFrozen(target)){
    Object.freeze(target);

    Object.keys(target).forEach(key => deepFreeze(target[key]));
    }
    return target;
}

// 이제 이 함수에 객체를 넣으면 중첩객체까지 동결가능하다.
```
