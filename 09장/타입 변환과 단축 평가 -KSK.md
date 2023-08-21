### **타입 변환이란?** 

자바스크립트의 모든 값은 타입이 있다. 값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있다.

이런 의도적인 타입 변환을 **명시적 타입 변환** 또는 **타입 캐스팅**이라 한다.

```
let x = 10;

let str = x.toString();
console.log(typeof str); // string
console.log(typeof x); // number
```

개발자의 의도와 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입 변환하는 경우를 **암묵적 타입** 변환 또는 **타입 강제 변환**이라한다.

```
let x = 10;
let str = x +'';

console.log(typeof str); // string
console.log(typeof x); // number
```

암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아니라, 자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 피연산자의 값을 암묵적 타입 변환하여 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다.

### **암묵적 타입 변환**

암묵적 타입 변환이 발생하면 문자열,숫자,불린과 같은 원시 타입 중 하나로 타입을 자동 변환한다.

```
// 피연산자가 모두 문자열 타입이어야 하는 문맥
'10' + 2 // '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * '10' // 50

// 피연산자가 모두 불린 타입이어야 하는 문맥
!0 // true
if(1){}
```

**▶ 문자열 타입으로 변환**

```
1+'2' // '12'
```

**\+ 연산자**는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.

따라서, 문자열 타입이 아닌 피연산자를 문자열로 암묵적 타입 변환한다.

```
// 피연산자에 숫자 타입이 있을 때
0 + '' // '0'
-0 + '' // '0'
1 + '' // '1'
-1 + '' // '-1'
NaN + '' // 'NaN'
Infinity + '' // 'Infinity'
-Infinity + '' // '-Infinity'

// 불린 타입
true + '' // 'true'

// null
null + '' // 'null'

// undefined
undefined + '' // 'undefined'

// 심벌은 변환 불가
(Symbol()) + '' // TypeError

// 객체 타입
({}) + '' // '[object Object]'
Math + '' // '[object Math]'
[] + '' // ''
[10,20] + '' // '10,20'
(function(){}) + '' // 'function(){}'
Array + '' // 'function Array(){[native code]}'
```

**▶ 숫자 타입으로 변환**

산술 연산자 ( -, \* , / ) 의 역할은 숫자 값을 만드는 것이다.   
피연산자가 숫자타입으로 변경 불가시 표현식 평가 결과는 NaN

```
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
```

비교 연산자도 마찬가지로 불린 값을 내뱉기 위해 피연산자들을 숫자 타입으로 암묵적 타입 변환한다.

```
'1' > 0 // true
```

\+ 단항 연산자는 피연산자가 숫자 타입이 아니면 숫자 타입의 값으로 암묵적 타입 변환한다.

```
// 문자열 타입
+ '' // 0
+ '0' // 0
+ '1' // 1
+ 'string' // NaN

// 불린 타입
+true // 1
+false // 0

// null
+null //0

// undefined
+undefined // NaN

// 심벌은 마찬가지로 불가
+Symbol() // TypeError

// 객체 타입
+{} // NaN
+[] // 0
+[10,20] // NaN // 10,20 을 풀어서 숫자열로 표현 불가하기 때문
+(function(){}) // NaN
```

**▶ 불린 타입으로 변환**

if 문이나 for 문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가되어야 하는 표현식이다. 자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다.

```
if('') console.log('1');
if(true) console.log('2');
if(0) console.log('3');
if('str') console.log('4');
if(null) console.log('5');

// 2 4
```

false 로 평가되는 값들

\=> false, undefined, null, 0, -0, NaN, ' '

```
[] + {}; // '[object Object]'
// 문자열로 타입 변환
// [] => ''
// {} => '[object Object]'


{} + []; // 0
// {} => 빈 코드 블록으로 인식
// +[] => 0

{} + [1,2]; // NaN
// +[1,2] // 1,2 를 숫자열로 변환불가 NaN
```

ㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣㅣ

### **명시적 타입 변환** 

개발자의 의도에 따라 명시적으로 타입을 변경하는 법에는

표준 빌트인 생성자 함수(String, Number, Boolean) 를 new 연산자 없이 호출하는 방법, 빌트인 메서드 사용 등등 다양하다.

**▶ 문자열 타입으로 변환**

1\. String 생성자 함수를 new 연산자 없이 호출

```
String(1); // '1'
String(NaN); // 'NaN'
```

2\. Object.prototype.toString 메서드

```
(1).toString(); // '1'
(NaN).toString(); // 'NaN'
```

3\. 문자열 연결 연산자 이용

```
1 + ''; // '1'
NaN + ''; // 'NaN'
```

**▶ 숫자 타입으로 변환**

1\. Number 생성자 함수를 new 연산자 없이 호출

```
Number('0'); // 0
Number(true); // 1
```

2\. parseInt, parseFloat 함수 사용 ( 문자열만 가능 )

```
parseInt('0'); // 0
parseInt(true); // NaN
```

3\. + 단항 산술 연산자

```
+'0'; // 0
+true; // 1
```

4\. \* 산술 연산자

```
'0' * 1; // 0
true * 1; //1
```

**▶ 불린 타입으로 변환**

1\. Boolean 생성자 함수를 new 연산자 없이 호출

```
Boolean('x'); true
Boolean(0); false
Boolean([]); true
Boolean({}); true
```

2\. ! 부정 논리 연산자를 두 번 사용

```
!!'x'; // true
!!''; // false
!!{}; // true
!![]; //true
```

### **단축 평가**

▶ 논리 연산자를 사용한 단축 평가

논리합(||) 또는 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다. 2개의 피연사 중 어느 한쪽으로 평가된다.

논리곱 연산자는 두 개의 피연산자가 모두 true 로 평가될 때 true 를 반환한다. 좌항에서 우항으로 평가가 진행된다.

```
'Cat' && 'Dog' // 'Dog'
// Dog 까지 평가가 끝나야 true 로 나오므로 Dog 반환
```

논리합 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true 를 반환한다. 마찬가지로 좌항에서 우항으로 평가된다.

```
'Cat' || 'Dog' // 'Cat'
// Cat 만 true 로 나와도 평가가 끝나므로 'Cat' 반환
```

**이처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는 것을 단축 평가라 한다. 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.**

**사용 예시**

**1\. 객체가 가리키기를 기대하는 변수가 null 또는 undefined 가 아닌지 확인하고 프로퍼티를 참조할 때**

```
let elem = null;
let value = elem.value; // TypeError

let value2 = elem && elem.value; // null
```

**2\. 함수 매개 변수에 기본값을 설정할 때**

```
function getStrLen(str) {
  str = str || '';
  return str.length;
}

getStrLen(); // 0
getStrLen('hi') // 2


// ES6 매개변수 기본값 설정 방식
function getStrLen(str = '') {
  return str.length;
}

getStrLen(); // 0
getStrLen('hi') // 2
```

**▶ 옵셔널 체이닝 연산자**

옵셔널 체이닝 연산자는 (?.) 는 좌항의 피연산자가 null 또는 undefined 인 경우 undefined 를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```
let elem = null;
let value = elem?.value;
console.log(value); // undefined
```

객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.

다음과 같은 논리 연산자 && 사용시 좌항연산자가 false 로 평가될 때 그대로 좌항 피연산자를 반환하는 것을 경우를 막는다.

```
let str = '';
let length = str && str.length;

console.log(length); // ''
// 좌항 평가가 false이므로 str.length 는 참조하지못하고 좌항을 그대로 반환


let str = '';
let length = str?.length;
console.log(length); // 0
// 좌항이 false 라도 null 또는 undefined 가 아니라면 우항 프로퍼티 참조로 간다.
```

**▶ null 병합 연산자**

null 병합 연산자 ( ?? ) 는 좌항의 피연산자가 null 또는 undefined 인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

변수에 기본값을 설정할 때 유용하다.

```
let foo = null ?? 'default string';
console.log(foo); // 'default string';
```

논리 연산자 || 를 사용한 단축 평가 시 좌항의 피연산자가 false 로 평가될때 우항을 반환하는데, 이 false 로 평가되는 값인 0 이나 '' 도 기본값으로서 유효하면 예기치 않은 동작이 발생할 수 있다.

```
let foo = '' || 'default string'
console.log(foo); // 'default string'

let foo = '' ?? 'default string'
console.log(foo); // ''
```
