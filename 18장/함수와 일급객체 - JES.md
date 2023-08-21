[##_Image|kage@WhQb1/btspClSDzZJ/raIzJ8L9E2M9Kvc2VHPEyK/img.png|CDM|1.3|{"originWidth":1080,"originHeight":1080,"style":"alignCenter","filename":"함수와 일급객체.png"}_##]

### 일급객체

- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
- 변수나 자료구조 ( 객체, 배열 등) 에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

👆🏻 위 조건을 만족하는 객체를 **일급 객체**라 한다.

```
// 자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체다.
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
	return ++num;
};

const decrease = function (num) {
	return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
	let num = 0;

    return function() {
    	num = aux(num);
        return num;
    };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2
```

함수가 일급객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미.

> 객체는 값이므로 함수는 값과 동일하게 취급할 수 있다.  
> 따라서 함수는 값을 사용할 수 잇는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면  
> 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가된다.

일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로 사용할 수도 있다는 것

👆🏻 이는 함수형 프로그래밍을 가능케 하는 자바스크립트의 장점 중 하나다.

일반 객체와 차이점도 있다.

- 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다.
- 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

---

### 함수 객체의 프로퍼티

함수도 객체이기 때문에 프로퍼티를 가질 수 있다.

[##_Image|kage@pEcjN/btspsGjKCPH/RYuKyiFKKW58ryCzvW5F60/img.png|CDM|1.3|{"originWidth":500,"originHeight":410,"style":"alignCenter","filename":"스크린샷 2023-08-01 오후 10.34.56.png"}_##]

위 처럼 console.dir 메서드를 사용하여 확인할 수 있다.

```
function square(number){
  return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));

/*
{
  length:
   { value: 1,
     writable: false,
     enumerable: false,
     configurable: true },
  name:
   { value: 'square',
     writable: false,
     enumerable: false,
     configurable: true },
  arguments:
   { value: null,
     writable: false,
     enumerable: false,
     configurable: false },
  caller:
   { value: null,
     writable: false,
     enumerable: false,
     configurable: false },
  prototype:
   { value: square {},
     writable: true,
     enumerable: false,
     configurable: false }
}
*/
```

arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티다.

**이들 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.**

---

#### arguments 프로퍼티

arguments 프로퍼티의 값음 arguments 객체다.

arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.

즉, 함수 회부에서는 참조할 수 없다.

> 🚨 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다. 따라서 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않는다.

함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다.

즉, **함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 이후 인수가 할당된다.**

> 👆🏻 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우 인수가 전달되지 않은 매개변수는 undefined로 초기환된 상태를 유지한다.

매개변수의 개수보다 인수를 더 많이 전달한 경우 초과된 인수는 무시된다.

> 👆🏻 초과된 인수가 그냥 버려지는 것은 아니고 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다.

arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타낸다.

> 선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성 때문에 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 필요가 있을 수 있다.  
> 이 때 유용하게 사용되는 것이 arguments 객체다.

arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다.

```
function sum () {
	let res = 0;

    // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있다.

    for ( let i = 0; i < arguments.length; i++ ) {
    	res += arguments[i];
    }

    return res;
}

console.log(sum());        // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3)); // 6
```

Rest 파라미터를 이용하여 작성하면 이렇다.

```
function sum (...args) {
	return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2));          // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

---

#### length 프로퍼티

함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

> 🚨 arguments 객체의 length 프로퍼티는 인자의 개수, 함수 객체의 length 프로퍼티는 매개변수의 개수

---

#### prototype 프로퍼티

prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티이다.

일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

---

**참고**

이웅모, 「모던자바스크립트 Deep Dive」, 위키북스, p.249~258
