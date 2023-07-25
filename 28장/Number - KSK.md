### **Number 생성자 함수**

​  
표준 빌트인 객체 Number 는 생성자 함수 객체다. new 연산자와 함께 호출해 인스턴스를 생성할 수 있다.  
​

```
// 인수 전달이 없으면 내부 슬롯에 0을 할당한 Number 래퍼 객체 생성
const numObj = new Number();
console.log(numObj);
// Number {0}
// [[Prototype]]: Number
// [[PrimitiveValue]]: 0
​
// 숫자 전달 시
const numObj2 = new Number(10);
console.log(numObj2);
// Number {10}
// [[Prototype]]: Number
// [[PrimitiveValue]]: 10
​
// 인수를 숫자 변경 불가 시 NaN 저장
const const numObj3 = new Number('Hello');
console.log(numObj3);
// Number {NaN}
// [[Prototype]]: Number
// [[PrimitiveValue]]: NaN
```

​  
new 연산자를 사용하지 않고 Number 생성자 함수 호출 시 인스턴스가 아닌 숫자를 반환한다.  
​  
이를 명시적으로 타입을 변환하는데 사용할 수 있다. => 9장 명시적 타입 변환  
​

```
Number('123.456'); // 123.456
​
Number(true); // 1
Number(false); //0
```

​

### **Number 프로퍼티**

​  
**▶ Number.EPSILON**  
​  
1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.  
​  
대략 2.2204460492503130808472633361816E-16 의 값을 갖는다.  
​  
부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다. 정수는 2진법으로 오차없이 저장가능하지만, 부동소수점을 표현하기위한 IEEE 754 는 2진법 변환 시 무한소수가 되어 미세한 오차가 발생하여, 이런 오차를 해결하기 위해 Number.EPSILON 을 사용한다.  
​

```
0.1+0.2; // 0.300000000000004
0.1+0.2 === 0.3 // false
​
function isEqual(a,b){
  return Math.abs(a-b) < Number.EPSILON;
}
​
isEqual(0.1+0.2,0.3); // true
```

​  
**▶ Number.MAX_VALUE**  
​  
자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. 이보다 큰 숫자는 Infinity 다.  
​

```
Number.MAX_VALUE; // 1.7976931348623157e+308
```

​  
**▶ Number.MIN_VALUE**  
​  
자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다. 이보다 작은 숫자는 0 이다.  
​

```
Number.MIN_VALUE; // 5e-324
console.log(Number.MIN_VALUE / 2); // 0
```

​  
**▶ Number.MAX_SAFE_INTEGER**  
​  
자바스크립트에서 안전한게 표현할 수 있는 가장 큰 정수 값이다.  
​

```
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

​  
**▶ Number.MIN_SAFE_INTEGER**  
​  
자바스크립트에서 안전한게 표현할 수 있는 가장 작은 정수 값이다.  
​

```
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

​  
**▶ Number.POSITIVE_INFINITY**  
​  
양의 무한대를 나타내는 숫자값 Infinity 와 같다.  
​  
**▶ Number.NEGATIVE_INFINITY**  
​  
음의 무한대를 나타내는 숫자값 -Infinity 와 같다.  
​  
**▶ Number.NaN**  
​  
숫자가 아닌 값을 나타내는 숫자값이다.  
​

```
Number.NaN // NaN
window.NaN // NaN
```

​  
☆ Number 프로퍼티들을 사용해서 수치 연산 안전성을 높히고 코드 가독성을 높히며, 배열의 크기를 제한하는 등 메모리 사용량을 효율적으로 관리할 수 있다.  
​

### **Number 메서드**

​  
**▶ Number.isFinite**  
​  
인수가 유한수인지 아닌지 불린값 반환  
​

```
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Infinity); // false
​
Number.isFinite(NaN); // false
```

​  
빌트인 전역 함수 isFinite 와는 차이가 있다.  
​

```
Number.isFinite(null); // false
​
// 빌트인 함수는 인수를 숫자로 암묵적으로 타입변환하여 null은 0으로 변환한다.
isFinite(null) // true
```

​  
**▶ Number.isInteger**  
​  
인수가 정수인지 아닌지 불린값 반환  
​

```
Number.isInteger(10); // true
Number.isInteger(0.5) // false
```

​  
**▶ Number.isNaN**  
​  
인수가 NaN 인지 아닌지 불린값 반환  
​  
마찬가지로 빌트인 전역 함수 isNaN 과 차이가 있다.  
​

```
Number.isNaN(undefined); // false
​
// 빌트인 함수는 인수를 숫자로 암묵적으로 타입변환하여 undefined은 NaN으로 변환한다.
isNaN(undefined) // true
```

​  
**▶ Number.isSafeInteger**  
​  
인수가 안전한 정수인지 아닌지 불린값 반환  
​  
안전한 정수값은 -(2^53-1) 과 2^53-1 사이의 정수값이다.  
​

```
Number.isSafeInteger(0); // true
Number.isSafeInteger(1000000000000000); // true
Number.isSafeInteger(10000000000000000); // false
```

​  
**▶ Number.prototype.toExponential**  
​  
숫자를 지수 표기법으로 변환하여 문자열로 반환한다.  
​  
지수 표기법은 e 앞에 있는 숫자에 10의 n 승을 곱하는 형식으로 수를 나타낸다.  
​  
인수로 소수점 이하로 표현할 자릿수를 전달  
​

```
(77.1234).toExponential(); // '7.71234e+1'
(77.1234).toExponential(2); // '7.71e+1'
(77.1234).toExponential(4); // '7.7123e+1'
```

​  
**▶ Number.prototype.toFixed**  
​  
숫자를 반올림하여 문자열로 반환한다.  
​  
인수로 소수점 이하 자릿수를 나타내는 0~20 사이의 정수 값을 넣는다. 넣지않으면 0이 기본값  
​

```
(12345.6789).toFixed(); //'12346'
(12345.6789).toFixed(1); // '12345.7'
(12345.6789).toFixed(2); // '12345.68'
```

​  
**▶ Number.prototype.toPrecision**  
​  
인수로 전달 받은 전체 자릿수까지 나머지 자릿수를 반올림하여 문자열 반환  
​  
전체 자릿수로 표현불가 시 지수 표기법으로 결과반환  
​

```
(12345.6789).toPrecision(); // '12345.6789'
(12345.6789).toPrecision(1); // '1e+4'
(12345.6789).toPrecision(2); // '1.2e+4'
(12345.6789).toPrecision(6); // '12345.7'
```

​  
**▶ Number.prototype.toString**  
​  
숫자를 문자열로 변환하여 반환  
​  
인수에 진법을 나타내는 2~36 사이 정수값을 전달 가능. 생략 시 10 진법이 기본값  
​

```
(10).toString(); // '10'
(16).toString(2); // '10000'
```
