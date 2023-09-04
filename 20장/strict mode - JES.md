[##_Image|kage@p8wDt/btssS3PILjV/iikt2lMHqEC0Kl12sLprNK/img.png|CDM|1.3|{"originWidth":1080,"originHeight":1080,"style":"alignCenter","filename":"strict mode.png"}_##]

### strict mode란?

```
function foo() {
	x = 10;
}
foo();

console.log(x); // ?
```

foo함수 내에서 선언하지 않은 x 변수에 값을 할당했을때, 변수 x를 찾아야 x에 값을 할당할 수 있기때문에

자바스크립트 엔진은 x 변수가 어디에서 선언되었는지 스코프 체인을 통해 검색하기 시작한다.

먼저 foo함수 내에서 검색하는데 함수내에는 x 변수의 선언이 없으므로 검색에 실패할 것이고,

상위 스코프에서 x변수의 선언을 검색할 것이다.

어디에도 x 변수의 선언이 없으면 ReferenceError를 발생시킬 것 같지만 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다.

이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있다.

이러한 현상을 **암묵적 전역**이라고 한다.

이런 잠재적인 오류를 조금 더 근본적으로 접근하여 해결하기 위해

ES5부터 **strict mode ( 엄격 모드 )** 가 추가되었다.

strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.

린트 도구는 strict mode가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있따.

🚨 참고로 ES6에서 도입된 클래스와 모듈은 기본적으로 strict mode가 적용된다.

### strict mode의 적용

적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가한다.

전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.

```
'use strict';

function foo() {
	x = 10; // ReferenceError : x is not defined
}

foo();
```

🚨 코드의 선두에 위치시키지 않으면 제대로 동작하지 않는다.

### strict mode가 발생시키는 에러

1.  암묵적 전역 👉🏻 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.
2.  변수, 함수, 매개변수의 삭제 👉🏻 delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.
3.  매개변수 이름의 중복 👉🏻 중복된 매개변수를 사용하면 SyntaxError가 발생한다.
4.  with문의 사용 👉🏻 with문을 사용하면 SyntaxError가 발생한다.
    1.  with문은 전달된 객체를 스코프체인에 추가한다. 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다. 따라서 **with문은 사용하지 않는 것이 좋다.**

---

**참고**

이웅모, 「모던자바스크립트 Deep Dive」, 위키북스, p.313~319
