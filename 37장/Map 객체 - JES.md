### Map 객체

Map객체는 키와 값의 쌍으로 이루어진 컬렉션이다. ( Set과 함께 es6에서 새로도입한 자료구조 )

한 Map에서의 키는 **오직 단 하나만 존재** 한다.

객체와 유사하지만 다음과 같은 차이가 있다.

| 구분                   | 객체                                                                  | Map 객체                                            |
| ---------------------- | --------------------------------------------------------------------- | --------------------------------------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값                                                   | 객체를 포함한 모든 값                               |
| 이터러블               | X                                                                     | O                                                   |
| 요소 개수 확인         | Object.keys(obj).length                                               | map.size                                            |
| 성능                   | 키-값 쌍의 빈번한 추가 및 제거와 관련된 상황에서는 성능이 좀 더 좋다. | 키-값 쌍의 빈번한 추가 및 제거에 최적화되지 않았다. |

→ 객체와 Map객체에 대한 더 자세한 비교는 MDN공식문서에 표로 정리되어 있다.

---

### Map 객체의 생성

Map 객체는 Map 생성자 함수로 생성한다.

```
// 인수를 전달하지 않으면 빈 Map객체가 생성된다.
const map = new Map();
console.log(map); // Map(0) {}

// 이터러블을 인수로 전달 받아 생성할 수 있다.
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

// 키와 값이 쌍으로 이루어지지 않은 이터러블을 넣으면 에러가 뜬다.
const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object

// 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.
// => 즉, 중복된 키를 갖는 요소가 존재할 수 없다.
const map3 = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map3); // Map(1) {"key1" => "value2"}
```

---

### Map.prototype.size

Map 객체의 요소 개수를 확인할 때에는 Map.prototype.size 프로퍼티를 사용한다.

```
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2

// size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이다.
// => 따라서, size 프로퍼티에 숫자를 할당하여 Map 객체의 요수 개수를 변경할 수 없다.
```

---

### Map.prototype.set

Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.

```
const map = new Map();

map.set('key1', 'value1');
console.log(map); // Map(1) {"key1" => "value1"}


// 객체처럼 할당을 할 수는 있지만 Map 객체의 데이터 구조와 상호작용하지는 않는다.
const wrongMap = new Map();
wrongMap['bla'] = 'blaa';
wrongMap['bla2'] = 'blaaa2';

console.log(wrongMap) // Map { bla: 'blaa', bla2: 'blaaa2' }

wrongMap.has('bla')    // false
wrongMap.delete('bla') // false
console.log(wrongMap)  // Map { bla: 'blaa', bla2: 'blaaa2' }
// => 따라서 set(key, value)를 사용하는 것이 올바른 방법이다.

// set 메서드는 새로운 요소가 추가된 Map 객체를 반환한다.
// => 따라서, set 메서드를 호출한 후에 set 메서드를 연속적으로 호출할 수 있다.

map
    .set('key2', 'value2')
    .set('key3', 'value3');

console.log(map); // Map(3) {"key1" => "value1", "key2" => "value2", "key3" => "value3"}

// Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이
// 덮어진다. 이때 에러가 발생하지는 않는다.

map.set('key1', 'new');

console.log(map); // Map(3) {"key1" => "new", "key2" => "value2", "key3" => "value3"}
```

#### Map 객체에서의 key 타입

객체는 문자열 또는 심벌 값만 키로 사용할 수 있지만 Map 객체는 키 타입에 제한이 없다.

객체를 포함한 모든 값을 키로 사용할 수 있는데, Map 객체와 일반 객체의 가장 두드러지는 차이점이다.

```
const myMap = new Map();

const keyString = 'a string';
const keyObj = {};
const keyFunc = function() {};

// 값 설정
myMap.set(keyString, "value associated with 'a string'");
myMap.set(keyObj, 'value associated with keyObj');
myMap.set(keyFunc, 'value associated with keyFunc');

console.log(myMap.size); // 3

// 값 불러오기
console.log(myMap.get(keyString)); // "value associated with 'a string'"
console.log(myMap.get(keyObj)); // "value associated with keyObj"
console.log(myMap.get(keyFunc)); // "value associated with keyFunc"

console.log(myMap.get('a string')); // "value associated with 'a string'", 왜냐하면 keyString === 'a string'
console.log(myMap.get({})); // undefined, 왜냐하면 keyObj !== {}
console.log(myMap.get(function() {})); // undefined, 왜냐하면 keyFunc !== function () {}
```

#### Map 객체에서의 NaN

일치비교연산자 === 을 사용하면 NaN과 NaN을 다르다고 평가하지만

Map객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.

MDN에서도 나와있는데 예시를 통해 확인해보자.

```
console.log(NaN === NaN); // false

const myMap = new Map();

myMap.set(Nan, 'value').set(NaN, 'not a number');

myMap.get(NaN);
// "not a number"

const otherNaN = Number('foo');
myMap.get(otherNaN);
// "not a number"
```

---

### Map.prototype.get

get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다.

요소가 존재하지 않으면 undefined를 반환한다.

```
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key'));  // undefined
```

---

### Map.prototype.has

has 메서드는 특정요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

---

### Map.prototype.delete

delete 메서드는 Map 객체의 요소를 삭제할 때 사용되며, 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

→ 따라서 set 메서드와 달리 연속적으로 호출할 수 없다.

만약 존재하지 않는 키로 요소를 삭제하려하면 에러없이 무시된다.

---

### Map.prototype.clear

Map 객체의 요소를 일괄 삭제할 때 사용되며 언제나 undefined를 반환한다.

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

map.clear();
console.log(map); // Map(0) {}
```

---

### Map 객체에서의 요소 순회

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.

배열에서의 forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달한다.

- 첫 번째 인수 : 현재 순회 중인 요소값
- 두 번째 인수 : 현재 순회 중인 요소키
- 세 번째 인수 : 현재 순회 중인 Map 객체 자체 ( optional )

```
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'],[kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));

/*
developer { name: 'Lee' } Map(2) { { name: 'Lee' } => 'developer', 
  { name: 'Kim' } => 'designer' } 
designer { name: 'Kim' } Map(2) { { name: 'Lee' } => 'developer', 
  { name: 'Kim' } => 'designer' }
*/
```

Map 객체는 이터러블이므로 for ... of 문으로도 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링\[footnote\]포이마웹: [https://poiemaweb.com/es6-destructuring](https://poiemaweb.com/es6-destructuring)\[/footnote\]할당의 대상이 될 수도 있다.

```
for ( const entry of map ) {
	console.log(entry);
    }
// [ { name: 'Lee' }, 'developer' ], [ { name: 'Kim' }, 'designer' ]

console.log([...map])
// [ { name: 'Lee' }, 'developer' ], [ { name: 'Kim' }, 'designer' ]

const [a, b] = map;
console.log(a, b);
//[ { name: 'Lee' }, 'developer' ] [ { name: 'Kim' }, 'designer' ]
```

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

| Map 메서드            | 설명                                                                       |
| --------------------- | -------------------------------------------------------------------------- |
| Map.prototype.keys    | 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환          |
| Map.prototype.values  | 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환          |
| Map.prototype.entries | 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |

Map 객체는 요소의 순서에 의미를 갖지 않지만 Map 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.

다른 이터러블의 순회와 호환성을 유지하기 위함이다.

---

**참고**

이웅모, 「모던자바스크립트 Deep Dive」, 위키북스, p.653~659

MDN docs <[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global\_Objects/Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)\>
