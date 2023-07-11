[##_Image|kage@dhSFUG/btsm829GjMH/EhaK5xHstHUddQc9IqdkT0/img.png|CDM|1.3|{"originWidth":1080,"originHeight":1080,"style":"alignCenter","filename":"Javascript-001.png"}_##]

### Set 객체

Set 객체는 중복되지 않는 유일한 값들의 집합이다.

Set 객체는 배열과 유사하지만 다음과 같은 차이가 있다.

| 구분                                 | 배열 | Set 객체 |
| ------------------------------------ | ---- | -------- |
| 동일한 값을 중복하여 포함할 수 있다. | O    | X        |
| 요서 순서에 의미가 있다.             | O    | X        |
| 인덱스로 요소에 접근할 수 있다.      | O    | X        |

Set 객체의 특성은 수학적 집합의 특성과 일치하며, Set은 수학적 집합을 구현하기 위한 자료구조다.

따라서 Set을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

---

### Set 객체의 생성

Set 객체는 Set 생성자 함수로 생성한다.

```
const set = new Set();
console.log(set); // Set(0) {}
```

Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성하는데 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.

```
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

이런 중복을 허용하지 않는 Set 객체의 특성을 활용해서 중복요소를 활용할 수 있다.

```
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [ 2, 1, 3, 4 ]

// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [ 2, 1, 3, 4 ]
```

---

### Set.prototype.size

Set 객체의 요소를 확인할때 Set.prototype.size 프로퍼티를 사용한다.

```
const { size } = new Set([1, 2, 3, 3]);
console.log(size) // 3
```

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이므로 size  프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.

```
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

set.size = 10; // 무시된다
console.log(set.size); // 3
```

---

### Set.prototype.add

Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.

```
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}

// add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. 따라서 연속적으로 호출( method chaining )할 수 있다.

set.add(2).add(3);
console.log(set); // Set(3) {1, 2, 3}

// 위에서 말했던 것처럼 중복된 요소의 추가는 허용되지 않는다.
// 이때 에러가 발생하지는 않고 무시된다.

set.add(2).add(2).add(4);
console.log(set); // Set(4) {1, 2, 3, 4}
```

Map 객체와 동일하게 중복을 추가를 허용하지 않기때문에 NaN과 NaN을 같다고 평가한다.

\-0 과 +0의 경우도 마찬가지로 같다고 평가하여 중복추가를 허용하지 않는다.\[footnote\] 원래는 Set객체에서 +0과 -0이 다른값이였지만 ECMAScript 2015 명세에서 변경되었다고 한다. MDN 브라우저 호환성의 "Key equality for -0 and 0"을 참고.\[/footnote\]

```
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(NaN).add(NaN);
console.log(set); // Set(1) { NaN }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(0).add(-0)
console.log(set); // Set(2) { NaN, 0 }
```

Set 객체는 객체나 배월과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```
const set = new Set();

set
	.add(1)
    .add('a')
    .add(true)
    .add(undefined)
    .add(null)
    .add({})
    .add([])
    .add(() => {});

console.log(set); // Set(8) { 1, "a", true, undefined, null, {}, [], () => {}}
```

---

### Set.prototype.has

Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다.

has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

---

### Set.prototype.delete

Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다.

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

★ delete 메서드에는 인덱스가 아니라 삭제하려는 요소값을 인수로 전달해야 한다.

Set 객체는 순서에 의미가 없다. 배열과 같이 인덱스를 갖지 않는다.

```
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) { 1, 3 }

// 만약 존재하지 않는 요소를 삭제하면 에러 없이 무시된다.
set.delete(0);
console.log(set); // Set(2) { 1, 3 }

// 삭제 성공 여부를 나타내는 불리언 값을 반환하기 때문에 연속적으로 호출( method chaining )할 수 없다.
set.delete(1).delete(3) // TypeError : set.delete(...).delete is not a function
```

---

### Set.prototype.clear

Set 객체의 모든 요소를 일관 삭제하려면 Set.prototype.clear 메서드를 사용한다.

clear 메서드는 언제나 undefined를 반환한다.

```
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

---

### Set.prototype.forEach

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.

Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달한다. 콜백 함수는 다음과 같이 3개의 인수를 전달 받는다.

- 첫 번째 인수 : 현재 순회 중인 요소값
- 두 번째 인수 : 현재 순회 중인 요소값
- 세 번째 인수 : 현제 순회 중인 Set 객체 자체

첫 번째 인수와 두 번째 인수는 같은 값이다.

이렇게 동작하는 이유는 배열의 forEach 메서드와 인터페이스를 통일하기 위함이며 다른 의미는 없다고한다.

배열의 forEach는 두 번째 인수로 현재 순회 중인 요소의 인덱스를 전달 받는데 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지않는다.

```
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));

/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

Set 객체는 이터러블이다. 따라서 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for ... of 문으로 순회할 수 있다.
for ( const value of set ) {
	console.log(value) // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

> Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.  
> 이는 따로 ECMAScript 사양에 규정되어 있지는 않지만 다른 이터러블의 순회와 호환성을 유지하기 위함이다.

---

### 집합연산

Set 객체는 수학적 집합을 구현하기 위한 자료구조다.

Set 객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다.

집합 연산을 수행하는 프로토타입 메서드를 구현하면 다음과 같다.

#### 교집합

```
Set.prototype.intersection = function (set) {
	const result = new Set();

    for ( const value of set ) {
    	if ( this.has(value) ) result.add(value);
    }

    return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

for문이 아닌 filter를 통해서도 구현이 가능하다.

```
Set.prototype.intersection = function (set) {
	return new Set([ ... this ].filter(v => set.has(v)));
};
```

---

#### 합집합

```
Set.prototype.union = function (set) {
	// this(Set 객체)를 복사
    const result = new Set(this);

    for ( const value of set ) {
    	// 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
        result.add(value);
    }

    return result;
};

// 스프레드 문법으로 한다면 이렇게 구현도 가능하다.
Set.prototype.union = function (set) {
	return new Set([ ... this, ...set ]);
};
```

---

#### 차집합

```
Set.prototype.difference = function (set) {
	const result = new Set(this);

    for ( const value of set ) {
    	// 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
        result.delete(value);
    }

    return result;
};

// filter를 이용하면 다음과 같이 구현가능하다.
Set.prototype.difference = function (set) {
	return new Set([ ...this].filter(v => !set.has(v)));
};
```

---

#### 부분집합과 상위집합

집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분집합( subset )이며, 집합 B는 집합 A의 상위집합( superset ) 이다.

```
Set.prototype.isSuperset = function (subset) {
	for ( const value of subset ) {
    	if ( !this.has(value) ) return false;
    }

    return true;
};

// 다른 방법
Set.prototype.isSuperset = function (subset) {
	const supersetArr = [ ...this ];
    return [...subset].every(v => supersetArr.includes(v));
};
```

---

**참고**

이웅모, 「모던자바스크립트 Deep Dive」, 위키북스, p.643~652

MDN docs <[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global\_Objects/Set](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)\>
