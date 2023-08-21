제어문은 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다.

일반적으로 코드는 위에서 아래 방향으로 순차 진행한다. 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어 가능하다.

### **블록문** 

블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드블록 또는 블록이라고 부른다.

자바스크립트는 블록문을 하나의 실행 단위로 취급한다. 일반적으로 제어문이나 함수를 정의할 때 사용한다.

```
// 단독 블록문
{
  let foo = 10;
}

// 제어문
let x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a,b){
  return a+b;
}
```

### **조건문**

조건문은 주어진 조건식의 평가 결과에 따라 코드 블록(블록문)의 실행을 결정한다. **조건식은 불린 값으로 평가될 수 있는 표현식이다**.

자바스크립트의 조건문은 if..else 문과 switch 문으로 두 가지 조건문을 제공한다.

▶ if...else 문

주어진 조건식의 평가 결과, 즉 논리적 참 거짓에 따라 실행할 코드 블록을 결정한다.

조건식의 평가 결과가 true 일 경우에 if 문의 코드 블록이 실행되고, false 일 경우에 else 문의 코드 블록이 실행된다.

조건식을 추가하여 조건에 따라 실행될 코드 블록을 늘리고 싶으면 else if 문을 사용한다. (else if 는 여러 번 사용가능)

```
if(조건식1) {
  // 조건식1 true
} else if(조건식2) {
  // 조건식2 true
} else {
  // 둘다 false
}
```

만약 코드 블럭 내의 문이 하나뿐이라면 중괄호를 생략할 수 있다.

```
let num = 2;
let kind;

if(num > 0) kind = '양수';
else if(num < 0) kind = '음수';
else kind = '0';
```

대부분의 if...else 문은 삼항 조건 연산자로 바꿀 수 있다.

```
let num = 2;
let kind = num > 0 ? '양수' : (num < 0 ? '음수' : '0');
```

단순 값을 할당하는 경우 삼항 조건 연산자가 가독성이 좋다.

복잡한 여러 줄의 문이 있을 때는 if...else 문을 쓰자.

▶ switch 문

주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행흐름을 옮긴다.

case 문은 상황을 의미하는 표현식을 지정하고 콜론으로 마친다. 그리고 그 뒤에 실행할 문들을 위치시킨다.

표현식이 일치하는 case 문이 없다면, 실행 순서는 default 문으로 이동한다. ( 사용 선택 )

```
switch (표현식){
  case 표현식1 :
    // switch 문의 표현식과 표현식1이 일치할때 실행문
    break;
  case 표현식2 :
    // switch 문의 표현식과 표현식2이 일치할때 실행문
    break;
  default :
    // switch 문의 표현식과 일치하는 case 문이 없을때 실행문
}
```

switch 문의 표현식은 불린값보다는 문자열이나 숫자 값인 경우가 많다.

즉, 녹리적 참,거짓 보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.

```
let num = 2;
let message;

switch (num) {
  case 1:
    message = '하나';
    break;
  case 2:
    message = '둘';
    break;
  case 3:
    message = '셋';
    break;
  default:
    message = '다른 숫자';
    break;  // 마지막 break 문은 switch 문이 종료될 때이므로 별도로 필요하진 않다.
}

console.log(message);   // '둘'
```

주의할 점은 break 문이 없을 때, switch 문에서 표현식이 일치하여 실행되는 문 뒤의 실행문도 전부 실행된다.

딥다이브에는 폴스루 (fall through) 라 설명된다.

[참고 : https://ko.javascript.info/switch](https://ko.javascript.info/switch)

```
let num = 2;
let message;

switch (num) {
  case 1:
    message = '하나';
  case 2:
    message = '둘'; // 여기서 표현식일치
  case 3:
    message = '셋'; // 하지만 switch 문이 끝나지 않고 뒤에 이어지는 case 3 , default 의 실행문도 실행
  default:
    message = '다른 숫자';
}

console.log(message);   // '다른 숫자'
```

\*\* if ... else 문으로 해결할 수 있다면 switch 문보다 if ... else 문을 사용하자.  
조건이 많을 때는 가독성을 위해서 switch 문을 사용할 수도 있다.

### **반복문**

반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. ( 조건식이 거짓일 때까지 반복 )

자바스크립트는 세 가지 반복문 for 문, while 문, do ... while 문을 제공한다.

++ 배열 forEach 메서드, 객체 프로퍼티 열거 for ... in 문, 이터러블 순회 for ... of 문 등 반복문 대체 기능이 있다.

▶ for 문

조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다.

```
for (let i = 0; i < 2; i++ ) {
  console.log(i);
}

// 0
// 1
```

1\. 변수 선언문 let i = 0 이 실행 ( 처음 한번만 실행 )

2\. 조건식 실행, 처음 i 값은 0 이므로 true

3\. true 이므로 코드 블록 실행 // 실행 흐름이 증감문으로 가는 것이 아님을 주의

4\. 코드 블록 실행 종료 후 증감식 i++ 실행, i 가 1이 된다.

5\. 증감식 실행 후 조건식 재실행, i 는 1 이므로 true

6\. true 이므로 코드 블록 재실행

7\. 코드 블록 실행 종료 후 증감식 재실행, i 가 2가 된다.

8\. 조건식 재실행, i 가 2 이므로 false >> 종료

모든 식이 생략 가능하지만, 어떤 식도 적지 않으면 무한루프가 된다. (코드블록 무한 실행)

```
// 무한루프
for(;;){...}
```

for 문 안에 for 문을 넣어 중첩 for 문을 만들 수 있다.

```
for(let i = 1; i <= 6; i++){
  for(let j = 1; j <= 6; j++){
    if(i+j===6) console.log(i,j);
  }
}
```

▶ while 문

주어진 조건식의 평과 결과가 참일 때 코드블록 반복 실행

**for 문은 반복 횟수가 명확할 때, while 문은 반복 횟수가 불명확할 때 주로 사용한다.**

조건식의 평가 결과가 불린 값이 아닐 땐, 강제 타입 변환

```
let count = 0;

while(count < 3){
  console.log(count) // 0 1 2
  count++;
}
```

```
// 무한 루프
while(true){...}
```

```
let count = 0;

while(true){
  console.log(count);
  count++;
  if(count === 3) break;
}
// 0 1 2
// break 문으로 무한루프 탈출
```

▶ do ... while 문

코드 블럭을 먼저 실행하고 조건식을 평가한다. >> 코드 블록은 무조건 한번 이상 실행된다.

```
let count = 0;

do {
  console.log(count); // 0 1 2
  count ++
} while (count < 3);
```

### **break 문** 

switch 문과 while 문에서 보았듯이 break 문은 코드 블록을 탈출한다. ( 레이블 문, 반복문, switch 문 )

**\*\* 레이블 문**

프로그램의 실행 순서를 제어하는 데 사용

```
label :
  statement
// label : 자바스크립트에서 사용할 수 있는 식별자 모두 가능 ( let 은 사용 못함 )
// statement : 구문, break 는 모든 레이블 구문에서 사용가능, continue 는 반복 레이블 구문만
```

```
// foo 라는 레이블 식별자가 붙은 레이블 문
foo : console.log('foo');
```

```
foo : {
  console.log(1);
  break foo; // foo 레이블 블록문 탈출
  console.log(2);
}
// 1
```

```
outer : for(let i = 0; i < 3; i++){
  for(let j = 0; j < 3; j++){
    // i+j ===3 이면 outer 식별자 레이블 for 문 탈출
    if(i+j===3) break outer;
    console.log(i,j);
  }
}
// 0 0
// 0 1
// 0 2
// 1 0
// 1 1
```

```
// 반복문 사용 예시

let str = 'Hello World.';
let serach = 'l';
let idx;

for (let i = 0; i < str.length; i++){
  if(str[i]===search){
    idx = i;
    break;
  }
}

console.log(idx); // 2
```

### **continue 문** 

반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. ( 탈출 x )

```
let str = 'Hello World.';
let serach = 'l';
let count = 0;

for (let i = 0; i < str.length; i++){
  // 'l' 이 아니면 현 시점에서 실행 중단, 증감식으로 이동
  if(str[i]!==search) continue;
  count ++; // continue 가 실행되면 이 문은 실행안된다.
}

console.log(count); // 3
```

위와 같이 if 문 내에서 실행할 코드가 한 줄이라면

```
for(let i = 0; i < str.length; i++){
  if(str[i]===search) count++;
}
```

이와 같이 적는게 가독성이 좋다.

하지만, 코드가 길다면 들여쓰기가 한 단계 더 깊어지므로 continue 문을 사용하는 편이 가독성이 좋다.
