6장. 데이터타입

### **자바스크립트의 타입 분류**

| 구분           | 데이터 타입                                       | 설명                                               |
| -------------- | ------------------------------------------------- | -------------------------------------------------- |
| 원시타입       | 숫자 타입                                         | 숫자. 정수와 실수 구분 없이 하나의 숫자타입만 존재 |
| 문자열 타입    | 문자열                                            |
| boolean 타입   | 논리적 참(true)과 거짓(false)                     |
| undefined 타입 | 키워드로 선언된 변수에 암묵적으로 할당되는 값     |
| null 타입      | 값이 없다는 것을 의도적으로 명시할 때 사용하는 값 |
| 심벌 타입      | ES6에서 추가된 7번째 타입                         |
| 객체타입       |                                                   | 원시 타입을 제외한 모든 것                         |

### **typeof 연산자**

▶ 변수의 타입을 확인할 수 있다.

### **숫자 타입**

▶ 자바스크립트의 모든 숫자 타입을 실수로 처리한다.

```
let age = 28; // 정수
let pi = 3.14; //실수
let negative = -20; // 음의 정수
```

▶ 숫자 타입은 다양한 수학적 연산이 가능하다.

```
let q = 2+3; // 5
let w = 10-3; // 7
let e = 4*6; // 24
let r = 14/7; // 2

// 나머지 연산자 %
let remain = 10%3; // 1
// 거듭제곱 **
let squ = 2**3; // 8
```

▶ 숫자 타입은 추가로 다음과 같은 세 가지 특수한 값을 갖는다.

1\. NaN ( Not-a-Number) :

NaN 숫자가 아님을 나타내는 값이다.

정의되지 않은 수학 연산의 결과로 생성되는 값으로 산술 연산이 불가하다.

※ isNaN() 함수를 사용해 숫자가 아닌지 boolean 값으로 나타낼 수 있다.

```
let result = 0/0;
console.log(result); // NaN

console.log(isNaN(NaN)); // true
console.log(isNaN(10)); // false
console.log(isNaN("Hello")); // true
```

2\. Infinity 와 -Infinity :

양의 무한대와 음의 무한대를 나타내는 값이며, 수학적 계산에서 오버플로우, 언더플로우가 발생할 때 생성된다.

수학적 계산 시, 유효한 숫자로 간주된다.

※ isFinite() 함수를 사용해 유한한 숫자인지 boolean 값으로 나타낼 수 있다.

```
console.log(Infinity/10); // Infinity
console.log(10/Infinity); // 0
console.log(10/-Infinity); // -0
console.log(Infinity/Infinity); // NaN

console.log(isFinite(10)); // true
console.log(isFinite(Infinity)); // false
```

▶ 숫자 타입을 문자열로 변환하기 위한 'toString()' 메서드 사용이 가능하다.

```
let num = 2023;
let str = num.toString();

console.log(str); // '2023'
```

### **문자열 타입**

**▶** 문자열 타입은 작은따옴표(' '), 큰따옴표(" ") 또는 백틱(\` \`)으로 텍스트를 감싼다.

▶ 템플릿 리터럴 :  
템플릿 리터럴은 백틱 기호로 둘러싸인 문자열로 문자열 내에 변수 값을 삽입하기 위해 \`${}\` 문법을 사용할 수 있다.

또한 여러 줄로 이루어진 문자열 작성에 유용하다. 일반 따옴표 문자열은 줄바꿈을 위해 \\n 을 사용해야한다.

```
let name = "Kyo";
let concat = "Hello, " + name + "!";
console.log(concat); // Hello, Kyo!

let tempConcat = `Hello, ${name}!`;
console.log(tempConcat); // Hello, Kyo!
```

### **boolean 타입**

▶ boolean 타입의 값은 논리적 참, 거짓을 나타내는 true와 false 뿐이다.

### **undefined 타입**

▶ undefined 타입은 값이 할당되지 않은 상태를 나타내는 타입이다.  
즉, 변수가 선언되었지만 초기화되지 않았거나 값이 할당되지 않은 경우 undefined 가 할당된다.

```
let x;
console.log(x); // undefined

function greet(name) {
  console.log("Hello, " + name);
}
greet(); // Hello, undefined

// undefined 를 값으로도 할당가능
let y = undefined;
console.log(y); //undefined
```

### **null 타입**

▶ null 타입은 값이 의도적으로 비어있음을 나타내는 타입이다.

즉, null 이 변수에 할당되어 해당 변수가 이전에 할당되어 있던 값에 대한 참조를 제거한다.

```
let x = "Hello";
console.log(x); // Hello

x = null;
console.log(x); // null
```

### **심벌 타입**

▶ 심벌타입은 유일하고 변경 불가능한 데이터 타입으로, ES6 에서 도입되었다.  
다른 어떤 값과도 충돌하지 않는 고유한 식별자를 생성하기 위해 사용한다.

▶ 주로 객체의 속성을 정의할 때 심벌을 키로 사용하며, 다른 속성과 충돌하지 않고 유일한 식별자로 사용할 수 있다.

```
let nameSymbol = Symbol('name');

let person = {
  [nameSymbol]: 'Kyo'
};

console.log(person[nameSymbol]); // Kyo

// 다른 키, 속성과 충돌없이 고유 식별자 사용가능
```

▶ 내장된 몇 가지 심벌 값과 함께 사용한다.  
※ 33장 symbol 참고 예시 Symbol.iterator 는 반복 가능한 객체의 기본 반복자를 정의하는데 사용한다.

```
let arr = [1,2,3];
let iterator = arr[Symbol.iterator]();

console.log(iterator.next()); // {value:1, done:false}
console.log(iterator.next()); // {value:2, done:false}
console.log(iterator.next()); // {value:3, done:false}
console.log(iterator.next()); // {value:undefined, done:true}

// Symbol.iterator 가 배열 arr 의 기본 반복자를 반환 >> arr 순회 ( 순회끝나면 done 값이 true )
```

### **객체 타입**

※ 11장에서 학습 필요

객체는 이름과 값으로 이루어진 프로퍼티의 집합으로 키-값 쌍으로 구성된다.

```
let person = {
  name : 'Kyo',
  age : 28,
  city : 'Seoul'
};

console.log(person.name); // Kyo
console.log(person['age']); // 28

// 대괄호 표기법은 동적으로 프로퍼티 이름을 결정해야할 때 유용 => 변수나 계산결과 등 (사용자 입력 or 외부 데이터)

person.gender = 'Male'; // 프로퍼티추가
person.age = 27; // 프로퍼티 수정
delete person.city; // 프로퍼티 삭제
```

### **데이터 타입의 필요성**

▶ 데이터 타입에 의한 메모리 공간 확보와 참조 :  
변수에 할당되는 메모리의 크기와 저장 방식을 결정한다.  
각 데이터 타입은 특정 크기를 갖고, 변수는 해당 크기의 메모리 공간을 차지한다.

따라서 메모리를 효율적으로 관리하여 성능을 최적화하고 자원을 절약한다.

▶ 메모리에 저장된 값이 타입에 따라 다르게 해석되어, 해당 값에 대한 원하는 연산과 동작을 데이터 타입을 명확히 하여 수행가능하다.

**동적 타이핑**

▶ 동적 타이핑은 변수의 데이터 타입을 선언 시점이 아닌 실행 시점에 결정하는 특성을 말한다.

```
let message;
message = 10; // 숫자 타입으로 변경
message = 'Hello' // 문자열 타입으로 변경

// 타입을 명시적으로 선언안하고, 동적으로 결정

let num = 10;
let str = '5';

let result = num + str; // 숫자 + 문자
console.log(result); // '105' (문자열)
```

위의 예시처럼 자동 타입 변환으로 편리하지만 때론 의도치 않은 타입변환이 일어날 수 있다.

### **변수 사용 Tip**

1\. 변수는 필요한 경우에만 제한적으로 => 많으면 충돌 및 오류 발생 확률이 높아진다.

2\. 변수의 스코프는 최대한 좁게 만들어 오류를 억제한다.

3\. 전역 변수의 사용을 최대한 피한다. => 처리 흐름 파악이 어렵고, 디버깅이 어려워진다.

4\. 변수보다 상수 사용으로 값의 변경을 억제한다.

5\. 변수명은 목적이나 의미를 파악할 수 있도록 한다.
