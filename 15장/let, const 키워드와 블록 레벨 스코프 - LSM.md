## 📌 var

ES5까지 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이었습니다.

var 키워드는 다른 언어와 구별되는 독특한 특징을 갖고있어, 주의를 기울이지 않으면 심각한 문제를 발생시킬 수 있습니다.

<br/>

- 변수 중복 선언 허용

```JS
var x = 1;
var y = 1;

var x = 100;

var y;

console.log(x); // 100
console.log(y); // 1
```

var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용합니다.

초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것 처럼 동작하고, 초기화문이 없는 변수 선언문은 무시됩니다.

이때 에러가 발생하지 않아 의도치 않게 먼저 선언된 변수 값이 변경되는 등의 부작용이 발생하게 됩니다.

<br/>

- 함수 레벨 스코프

```JS
var x = 1;

if(true) {
	var x = 10;
}

console.log(x); // 10


var i = 10;

for(var i = 0; i < 5; i++) {
	console.log(i); // 0 1 2 3 4
}

console.log(i); // 5
```

var 키워드로 선언한 변수는 오로지 함수의 코드 블록 만을 지역 스코프로 인정합니다. 함수 외부에서 선언한 변수나 코드 블록 내에 선언된 변수들은 모두 전역 변수가 됩니다.

if 문의 블록 스코프 내 var 키워드로 선언된 x는 이미 선언된 전역 변수 x를 중복 선언한 것이 되어 부작용을 초래합니다.

for 문에서 선언된 i는 전역변수가 되고, 이미 선언된 전역 변수 i가 있으므로 이는 마찬가지로 중복 선언됩니다.

<br/>

- 변수 호이스팅

```JS
console.log(foo); // undefined

foo = 123;

console.log(foo) // 123

var foo;
```

var 키워드로 선언된 변수는 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작합니다.

즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있습니다.

하지만 할당문 이전에 변수를 참조하면 언제나 undefined를 반환합니다.

<br/>

## 📌 let

예상치 못한 오류를 발생시키는 var 키워드의 단점을 보완하기 위해 ES6에서 let이 도입되었습니다.

let을 var와 비교하여 특징을 살펴보겠습니다.

<br/>

- 변수 중복 선언 금지

```JS
var foo = 123;

var foo = 456;

console.log(foo) // 456


let bar = 123;

let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

var 키워드는 중복 선언을 허용하기 때문에 아무런 에러를 반환하지 않는 반면, let 키워드는 중복 선언을 허용하지 않기 때문에 이름이 같은 변수를 중복 선언하면 SyntaxError가 발생합니다.

<br/>

- 블록 레벨 스코프

```JS
let foo = 1; // 전역 변수

{
	let foo = 2; // 지역 변수
    let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따르는 반면, let 키워드로 선언한 변수는 모든 코드 블록(함수, if, for, while, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따릅니다.

<br/>

- 변수 호이스팅

```JS
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

var 키워드로 선언한 변수는 변수 호이스팅이 일어난 것 처럼 동작하는 반면, let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않습니다.

let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생합니다.

<br/>

```JS
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1;
console.log(foo); // 1


console.log(bar); // ReferenceError: bar is not defined

let bar;
console.log(bar); // undefined

bar = 1;
console.log(bar); // 1
```

var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 "선언 단계" 와 "초기화 단계"가 동시에 진행됩니다.

따라서 undefined로 초기화 하고, 할당문에 도달하면 비로소 값이 할당됩니다.

그와 반면, let 키워드로 선언한 변수는 "선언 단계" 와 "초기화 단계"가 분리되어 진행됩니다. 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만, 초기화 단계는 변수 선언문에 도달했을 때 비로소 실행됩니다.

만약, 초기화 단계 이전에 변수에 접근하려고 하면 참조 에러가 발생하게 됩니다. let 키워드로 선언된 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없습니다.

스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대(Temporal Dead Zone: TDZ)라고 합니다.

<br/>

```JS
let foo = 1; // 전역 변수

{
	console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
    let foo = 2; // 지역 변수
}
```

let 키워드로 선언한 변수는 호이스팅이 발생하지 않는 것 처럼 보이지만, 호이스팅이 발생하기 때문에 참조에러가 발생합니다.

자바스크립트는 ES6에서 도입된 let, const를 포함하여 모든 선언(var, let, const, function, class 등)을 호이스팅합니다.

단, let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것 처럼 동작합니다.

<br/>

- 전역 객체와 let

```JS
var x = 1; // 전역 변수

y = 2; // 암묵적 전역

function foo(){} // 전역 함수

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티
console.log(window.x); // 1

// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용 가능
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티
console.log(window.foo); // 𝒇 foo() {}

// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있음
console.log(foo); // 𝒇 foo() {}
```

var 키워드로 선언한 전역 변수와 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 됩니다.

전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있습니다.

<br/>

```JS
let x = 1;

console.log(window.x); // undefined
console.log(x); // 1
```

그에 반면, let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아닙니다.

즉, window.foo와 같이 접근할 수 없습니다. let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 됩니다.

<br/>

## 📌 const

const 키워드는 상수(constant)를 선언하기 위해 사용됩니다. 하지만 반드시 상수만을 위해 사용하지는 않습니다.

let 키워드와 대부분 동일하므로 다른 점을 중심으로 살펴보겠습니다.

<br/>

- 선언과 초기화

```JS
const foo; // SyntaxError: Missing initializer in const declaration

const foo = 1;
```

const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화를 해야합니다.

<br/>

```JS
{
	console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
    const foo = 1;
    console.log(foo); // 1
}

console.log(foo); // ReferenceError: foo is not defined
```

let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것 처럼 보입니다.

<br/>

- 재할당 금지

```JS
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable
```

var 또는 let 키워드로 선언한 변수는 재할당이 자유롭지만, const 키워드로 선언한 변수는 재할당이 금지됩니다.

<br/>

- 상수

```JS
let preTax = 100;

let afterTax = preTax + (preTax * 0.1);
console.log(afterTax); // 110


// 상수 적용 코드
const TAX_RATE = 0.1;

let preTax = 100;

let afterTax = preTax + (preTax * TAX_RATE);
console.log(afterTax); // 110
```

const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없습니다. 원시 값은 변경 불가능한 값이므로 재할당 없이 값을 변경할 수 있는 방법이 없기 때문입니다. 이러한 특징을 이용해 const 키워드를 상수를 표현하는 데 사용하기도 합니다.

상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 합니다.

일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타내며, 여러 단어로 이루어진 경우에는 언더스코어(\_)로 구분하여 스네이크 케이스로 표현하는 것이 일반적입니다.

<br/>

- const 키워드와 객체

```JS
const person = {
	name : 'Mia'
};

person.name = 'SM';
console.log(person); // {name: 'SM'}
```

const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없습니다. 하지만 const 키워드로 선언된 변수에 객체를 할당한 경우 객체 내 값을 변경할 수 있습니다.

const 키워드는 재할당을 금지할 뿐 "불변"을 의미하진 않습니다. 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능합니다. 이 때, 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않습니다.

<br/>

## 📌 var VS let VS const

변수 선언 시 기본적으로 const 키워드를 생활화하고, let 키워드는 재할당이 필요한 경우에 한정적으로 사용하는 것이 좋습니다.

var 키워드와 let, const 키워드는 다음과 같이 사용하는 것을 권장합니다.

- ES6를 사용한다면 var 키워드는 사용하는 것을 지양합니다.
- 재할당이 필요한 경우 한정해 let 키워드를 사용하고, 변수의 스코프는 최대한 좁게 만듭니다.
- 변경이 없고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 적극 활용합니다.
