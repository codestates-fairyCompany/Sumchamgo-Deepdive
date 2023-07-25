표준 빌트인 객체인 Math 는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다.

생성자 함수가 아니므로, 정적 프로퍼티와 정적 메서드만 제공한다.

### **Math 프로퍼티** 

▶ Math.PI

원주율 PI 값을 반환

```
Math.PI; // 3.141592653589793
```

### **Math 메서드**

**▶ Math.abs**

인수로 전달된 숫자의 **절대값**을 반환한다. 절대값은 반드시 0 또는 양수이다.

```
Math.abs(-1); // 1
Math.abs('-1'); // 1
Math.abs(''); // 0
Math.abs([]); // 0 // 배열은 숫자변환가능
Math.abs(null); // 0
Math.abs(undefined); // NaN
Math.abs({}); // NaN // 객체는 숫자변환불가
Math.abs('string'); // NaN
Math.abs(); // NaN
```

**▶ Math.round**

인수로 전달된 숫자의 소수점 이하를 **반올림**한 정수를 반환한다.

```
Math.round(1.2); // 1
Math.round(1.6); // 2
Math.round(-1.4); // -1
Math.round(); // NaN
```

**▶ Math.ceil**

인수로 전달된 숫자의 소수점 이하를 **올림**한 정수를 반환한다.

```
Math.ceil(1.2); // 2
Math.ceil(1.6); // 2
Math.ceil(-1.4); // -1
Math.ceil(); // NaN
```

**▶ Math.floor**

인수로 전달된 숫자의 소수점 이하를 **내림**한 정수를 반환한다.

```
Math.floor(1.2); // 1
Math.floor(1.6); // 1
Math.floor(-1.4); // -2
Math.floor(); // NaN
```

**▶ Math.sqrt**

인수로 전달된 숫자의 **제곱근**을 반환한다.

```
Math.sqrt(9); //3
Math.sqrt(-9); // NaN
Math.sqrt(2); // 1.414....
Math.sqrt(0); // 0
Math.sqrt(); // NaN
```

**▶ Math.random**

임의의 난수(랜덤 숫자) 를 반환한다.  난수는 0이상 1 미만의 실수이다.

```
const random = Math.floor((Math.random()*10)+1);
console.log(random); // 1에서 10 범위의 정수
```

**▶ Math.pow**

첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과 반환

```
Math.pow(2,8); // 256
Math.pow(2,-1); // 0.5
Math.pow(2); // 0

// 지수연산자로 표현가능
2**2**2; // 16
```

**▶ Math.max**

전달받은 인수 중 가장 큰 수를 반환한다. 인수 미전달 시 -Infinity 반환

```
Math.max(1,2,3); // 3
Math.max(); // -Infinity
```

**▶ Math.min**

전달받은 인수 중 가장 작은 수를 반환한다. 인수 미전달 시 Infinity 반환

```
Math.min(1,2,3); // 1
Math.min(); // Infinity
```
