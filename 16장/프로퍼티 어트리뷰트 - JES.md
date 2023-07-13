### 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드다.

ECMAScript 사양에 등장하는 이중 대괄호 ( \[\[...\]\] )로 감싼 이름들이 내부 슬롯과 내부 메서드다.

예를 들어, 모든 객체는 \[\[Prototype\]\]이라는 내부 슬롯을 갖는다.

내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 원칙적으로 직접 접근할 수 없지만 \[\[Prototype\]\] 내부 슬롯의 경우, \_\_proto\_\_를 통해 간접적으로 접근할 수 있다.

```
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // -> Uncaught SyntaxError: Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
o.__proto__ // -> Object.prototype
```

---

### 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.

프로퍼티의 상태란 **프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumberable), 재정의 가능 여부(configurable)**를 말한다.

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯 \[\[Value\]\], \[\[Writable\]\], \[\[Enumberable\]\], \[\[Configurable\]\]이다. 따라서 프로퍼티 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDesciptor 메서드를 사용하여 간접적으로 확인할 수는 있다.

```
const person = {
	name: 'Jang'
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// { value: 'Jang', writable: true, enumerable: true, configurable: true }
```

Object.getOwnPropertyDescriptor 메서드는 호출할 때 첫 번째 매개변수에는 객체의 참조를 전달하고, 두 번째 매개변수에는 프로퍼티 키를 문자열로 전달한다.

이때 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.

\-> 원래 하나의 프로퍼티에 대해 프로퍼티 디스크립터 객체를 반환하지만 ES8에서 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.

```
const person = {
	name: 'Jang'
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{ name:
   { value: 'Jang',
     writable: true,
     enumerable: true,
     configurable: true },
  age:
   { value: 20,
     writable: true,
     enumerable: true,
     configurable: true } }
*/
```

---

### 데이터 프로퍼티와 접근자 프로퍼티

- **데이터 프로퍼티** : 키와 값으로 구성된 일반적인 프로퍼티.
- **접근자 프로퍼티** : 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티.

#### 데이터 프로퍼티

데이터 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

이 포르퍼티 어트리 뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

| 프로퍼티 어트리뷰트  | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                                                                                                                                                                                                                                                   |
| -------------------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[\[Value\]\]        | value                               | \- 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다. \- 프로퍼티 키를 통해 프로퍼티 값을 변경하면 \[\[Value\]\]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 \[\[Value\]\]에 값을 저장한다.                                           |
| \[\[Writable\]\]     | writable                            | \- 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다. \- \[\[Writable\]\]의 값이 false인 경우 해당 프로퍼티의 \[\[Value\]\]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다.                                                                                                  |
| \[\[Enumberable\]\]  | enumberable                         | \- 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다. \- \[\[Enumberable\]\]의 값이 false인 경우 해당 프로퍼티는 for ... in문이나 Object.keys 메서드 등으로 열거할 수 없다.                                                                                                      |
| \[\[Configurable\]\] | configurable                        | \- 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다. \- \[\[Configurable\]\]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값이 변경이 금지된다. 단, \[\[Writable\]\]이 true인 경우 \[\[Value\]\]의 변경과 \[\[Writable\]\]을 false로 변경하는 것은 허용된다. |

★ 즉, 일반적으로 넣는 데이터 값은 위의 프로퍼티 어트리뷰트를 갖고, 이는 자바스크립트 엔진이 프로퍼티를 생성할 때 자동으로 정의.

---

#### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로 값을 갖지않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

| 프로퍼티 어트리뷰트  | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                                                                                                                                                                                                |
| -------------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[\[Get\]\]          | get                                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수이다. 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰터 \[\[Get\]\]의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다. |
| \[\[Set\]\]          | set                                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 \[\[Set\]\]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다. |
| \[\[Enumberable\]\]  | enumberable                         | 데이터 프로퍼티의 \[\[Enumberable\]\]과 같다.                                                                                                                                                                                       |
| \[\[Configurable\]\] | configurable                        | 데이터 프로퍼티의 \[\[Configurable\]\]과 같다.                                                                                                                                                                                      |

접근자 함수는 getter/setter 함수라고도 부른다. 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고 하나만 정의 할 수도있다.

```
const person = {
	// 데이터 프로퍼티
    firstName: 'Jang',
    lastName: 'Jaymee',

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
    // getter 함수
    get fullName() {
    	return `${this.firstName} ${this.lastName}`;
    },

    // setter 함수
    set fullName(name) {
    	[this.firstName, this.lastName] = name.split(' ');
    }
};

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장 및 참조
// fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Code Kyo';
console.log(person); // { firstName: 'Code', lastName: 'Kyo' }

// fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName) // Code Kyo

// 즉, 여기서 firstName과 lastName은 데이터 프로퍼티,
// fullName은 접근자 프로퍼티이다.
console.log(Object.getOwnPropertyDescriptor(person, 'firstName'));
/*
{ value: 'Code',
  writable: true,
  enumerable: true,
  configurable: true }
*/
console.log(Object.getOwnPropertyDescriptor(person, 'fullName'));
/*
{ get: [λ: get fullName],
  set: [λ: set fullName],
  enumerable: true,
  configurable: true }
*/
```

접근자 프로퍼티는 자체적으로 값을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여할 뿐이다.

---

#### ▶ 프로토타입

> 프로토타입은 어떤 객체의 상위 객체의 역할을 하는 객체다.  
> 하위 객체에게 자신의 프로퍼티와 메서드를 상속한다.  
> 프로토타입 객체의 프로퍼티나 메서드를 상속받은 하위 객체는 자신의 프로퍼티 또는 메서드인 것처럼 자유롭게 사용할 수 있다.

---

### 프로퍼티 정의

새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의 하는 것을 말한다.

프로퍼티 값을 갱신 가능하도록 할 것인지, 프로퍼티를 열거 가능하도록 할 것인지, 프로퍼티를 재정의 가능하도록 할 것인지 정의할 수 있다.

Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰를 정의할 수 있다.

인수로는 객체의 참조와 데이터 프로퍼티의 키니 문자열, 프로퍼티 디스크립터 객체를 전달한다.

```
const person = {};

Object.defineProperty(person, 'firstName', {
	value: 'Jang',
    writable: true,
    enumberable: true,
    configurable: true
});

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
Object.defineProperty(person, 'lastName', {
	value: 'Jaymee'
});
// [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]값을 변경할 수 없다.
// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제할 수 없다. 해당 프로퍼티를 재정의할 수도 없다.
// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
```

프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략할 수 있다.

프로퍼티 디스크립터 객체에서 생략된 어트리뷰트는 다음과 같이 기본값이 적용된다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을때의 기본값 |
| ----------------------------------- | ---------------------------- | ------------------- |
| value                               | \[\[Value\]\]                | undefined           |
| get                                 | \[\[Get\]\]                  | undefined           |
| set                                 | \[\[Set\]\]                  | undefined           |
| writable                            | \[\[Writable\]\]             | false               |
| enumberable                         | \[\[Enumberable\]\]          | false               |
| configurable                        | \[\[Configurable\]\]         | false               |

Object.defineProperty 메서드는 한번에 하나의 프로퍼티만 정의할 수 있지만

Object.defineProperties 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

---

### 객체 변경 방지

자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.

객체 변경 방지 메서드들은 객체의 변경을 금지하는 강도가 다르다.

| 구분           | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| -------------- | ------------------------ | ------------- | ------------- | ---------------- | ---------------- | -------------------------- |
| 객체 확장 금지 | Object.preventExtensions | X             | O             | O                | O                | O                          |
| 객체 밀봉      | Object.seal              | X             | X             | O                | O                | X                          |
| 객체 동결      | Object.freeze            | X             | X             | O                | X                | X                          |

#### 객체 확장 금지

Object.preventExtensions 메서드는 객체의 확장을 금지한다.

**확장이 금지된 객체는 프로퍼티 추가가 금지된다.**

확장이 가능한 객체인지 여부는 Object.isExtensible 메서드로 확인할 수 있다.

---

#### 객체 밀봉

Object.seal 메서드는 객체를 밀봉한다.

**밀봉된 객체는 읽기와 쓰기만 가능하다.**

밀봉된 객체인지 여부는 Object.isSealed 메서드로 확인할 수 있다.

---

#### 객체 동결

Object.freeze 메서드는 객체를 동결한다.

**동결된 객체는 읽기만 가능하다.**

동결된 객체인지 여부는 Object.isFrozen 메서드로 확인할 수 있다.

---

#### 불변객체

위의 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 병경이 방지되고 중첩 객체까지는 영향을 주지 못한다.

```
const person = {
	name: 'Jang',
    address: { city: 'Seoul' }
};

console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // false
// 이 처럼 중첩 객체까지 동결하지는 못한다.

person.address.city = 'Busan';
console.log(person); // { name: 'Jang', address: { city: "Busan" } }
```

객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 같은 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출 해야한다.

```
function deepFreeze(target) {
	if ( target && typeof target === 'object' && !Object.isFrozen(target)) {
    	Object.freeze(target);
        Object.keys(target).forEach(key => deepFreeze(target[key]));
    }
    return target;
}
```

---

**참고**

이웅모, 「모던자바스크립트 Deep Dive」, 위키북스, p.219~233
