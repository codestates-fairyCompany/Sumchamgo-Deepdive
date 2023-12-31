[##_Image|kage@ujc5n/btsn6J2By6p/kvmSqLqUk0BeTf0liZ2lFK/img.png|CDM|1.3|{"originWidth":1080,"originHeight":1080,"style":"alignCenter","filename":"객체 리터럴.png"}_##]

### 객체

> 자바스크립트는 **객체기반의 프로그래밍 언어**이며, 자바스크립트를 구성하는 거의 **"모든 것"**이 객체다.  
> 원시 값을 제외한 나머지 값( 함수, 배열, 정규 표현식 등 )은 모두 객체다.

객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 **키**와 **값**으로 구성된다.

👆🏻 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있다.

따라서, 함수도 프로퍼티 값으로 사용할 수 있다.

프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 **메서드**라 부른다.

객체는 프로퍼티와 메서드로 구성된 집합체다.

- **프로퍼티** : 객체의 상태를 나타내는 값
- **메서드** : 프로퍼티를 참조하고 조작할 수 있는 동작

객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 **객체지향 프로그래밍**이라 한다.

---

### 객체 리터럴에 의한 객체 생성

자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성방법을 지원한다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 ( ES6 )

**리터럴**은 사람이 이해할 수 있는 문자(알파벳, 한글 등) 또는 약속된 기호('', "", \[\], {}, // 등)를 사용하여 값을 생성하는 표기법을 말한다.

객체 리터럴은 중괄호 내에 0개 이상의 프로퍼티를 정의한다.

변수가 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.

프로퍼티를 정의하지 않으면 빈 객체가 생성된다.

---

### 프로퍼티

객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.

- **프로퍼티 키** : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- **프로퍼티 값** : 자바스크립트에서 사용할 수 있는 모든 값

```
var person = {
	// 프로퍼티 키는 name, 프로퍼티 값은 'Lee"
    name: 'Lee',
}
```

심벌 값도 프로퍼티 키로 사용할 수 있지만 일반적으로 문자열을 사용한다.

식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표("")를 사용해야 한다.

```
var person = {
	firstName: 'Jay-mee', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
    last-name: 'Jang' // SyntaxError: Unexpect token -
}

// last-name을 - 연산자가있는 표현식으로 해석한다.
```

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.

👆🏻이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호(\[...\])로 묶어야 한다.

```
var obj = {};
var key = 'hello';

obj[key] = 'world';
// ES6
// var obj = { [key]: 'world' };

console.log(obj); // {hello: "world"}
```

이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.

🚨**이때 에러가 발생하지 않으니 주의.**

```
var foo = {
	name: 'Kim',
    name: 'Jang'
};

console.log(foo); // {name: "Jang"}
```

---

### 메서드

자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용할 수 있다.

자바스크립트의 함수는 일급 객체이므로 함수를 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.

🙋🏻‍♂️ **프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드( method )라 부른다.**

```
var circle = {
	radius: 5, // <- 프로퍼티

    // 원의 지름
    getDiameter: function () { // <- 메서드
    	return 2 * this.radius; // this는 circle을 가리킨다.
    }
};

console.log(cicle.getDiameter()); // 10
```

---

### 프로퍼티 접근

- 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법 ( dot natation )
- 대괄호 프로퍼티 접근 연산자(\[...\])를 사용하는 대괄호 표기법 ( bracket notation )

프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름이면 마침표 표기법과 대괄호 표기법을 모두 사용할 수 있다.

🚨 **대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.**

→ 대괄호 프로퍼티 접근 연산자 내에 따옴표로 감싸지 않은 이름을 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석한다.

```
var person = {
	name: 'Jang'
};

console.log(person[name]) // ReferenceError: name is not defined

// 여기서 ReferenceError가 발생한 이유는 대괄호 연산자 내의 따옴표로 감싸지 않은 이름,
// 즉 식별자 name을 평가하기 위해 선언된 name을 찾았지만 찾지 못했기 때문이다.

// 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.
// 이때는 ReferenceError가 발생하지 않는 것에 주의하자.
```

프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.

🚨**그 외의 경우 대괄호 내에 들어가는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이여야 한다!**

---

### 프로퍼티 값 갱신

```
var person = {
	name: 'Jang'
};

person.name = 'Kim';

console.log(person) // {name: "Kim"}
```

---

### 프로퍼티 동적 생성

```
var person = {
	name: 'Jang'
};

person.age = 20;

console.log(person); // {name: "Jang", age: 20}
```

---

### 프로퍼티 삭제

delete 연산자는 객체의 프로퍼티를 삭제한다.

이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다.

만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.

```
var person = {
  name: 'Jang'
};

person.age = 20;

delete person.age;
console.log(person); // { name: 'Jang' }

delete person.address; // 에러가 발생하지 않는다.
console.log(person); // { name: 'Jang' }
```

---

### ES6에서 추가된 객체 리터럴의 확장 기능

#### 프로퍼티 축약 표현

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다.

이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```
let x = 1, y = 2;

const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

---

#### 계산된 프로퍼티 이름

문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.

단, 프로퍼티 키로 상요할 표현식을 대괄호(\[...\])로 묶어야 한다.

이를 계산된 프로퍼티 이름이라 한다.

ES5에선 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호(\[...\]) 표기법을 사용해야 하지만

ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있다.

```
const prefix = 'prop';
let i = 0;

const obj = {
	[`${prefix}-${i++}`]: i,
    [`${prefix}-${i++}`]: i,
    [`${prefix}-${i++}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

---

#### 메서드 축약 표현

```
const obj = {
	name: 'Jang',
    // 메서드 축약 표현
    sayHi() {
    	console.log('Hi');
    }
};

obj.sayHi(); // Hi
```

---

**참고**

이웅모, 「모던자바스크립트 Deep Dive」, 위키북스, p.124~136
