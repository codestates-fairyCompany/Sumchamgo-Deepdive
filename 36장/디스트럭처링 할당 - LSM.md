# 구조 분해 할당

> 구조화된 배열과 같은 이터러블 또는 객체를 비구조화(구조를 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 뜻한다. <br/>
> 배열과 같은 이터러블(반복 가능한) 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.

<br/>

### 📌 배열 구조 분해 할당

<br/>

```JS
// ES5
var arr = ['ES','SK','SM'];
var person1 = arr[0];
var person2 = arr[1];
var person3 = arr[2];

console.log(person1, person2, person3); // ES, SK, SM
```

ES5에서는 구조화된 배열을 구조 분해하여 1개 이상의 변수에 할당한다.

```JS
// ES6 배열 구조 분해 할당
const arr = ['JES', 'KSK', 'LSM'];
const [person1, person2, person3] = arr;

console.log(person1, person2, person3); // JES, KSK, LSM
```

ES6에서의 배열 구조 분해 할당은 배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당한다.<br/>
이 때, 배열 구조 분해 할당의 대상(할당문의 우변)은 이터러블이어야 하며 할당 기준은 배열의 인덱스이다.

```JS
// 1
const [a,b] = ['apple', 'banana'];

// 2
let a, b;
[a,b] = ['apple', 'banana'];

// 3
const [a,b] = ['apple', 'banana'];
console.log(a,b); // apple banana

const [c,d] = ['carrot'];
console.log(c,d); // carrot undefined

const [e,f] = ['egg', 'flower', 'game'];
console.log(e,f); // egg flower

const [h,,j] = ['hat', 'ice', 'jump'];
console.log(h,j); // hat jump
```

1. 할당 연산자 왼쪽에 값을 할당받을 변수를 선언해야하고, 변수는 배열 리터럴 형태로 선언한다.

2. 변수 선언문은 선언과 할당을 분리할 수 도 있는데, 이는 const 키워드로 변수를 선언할 수 없으므로 지양한다.

3. 배열 구조 분해 할당의 기준은 배열의 인덱스, 즉 앞에서부터 순서대로 할당된다. 따라서 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요가 없다. 할당되지 않은 변수의 인덱스는 undefined, 할당 되었으나 매개변수 또는 인자로 불러오지 않을 경우 해당 인덱스는 무시된다.

<br/>

우변에 이터러블을 할당하지 않으면 아래와 같은 에러가 발생한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoF1VE%2Fbtsm8PCL3iq%2F69Mkub9GFkU8aEdGtVLkIK%2Fimg.png">

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKOrdy%2Fbtsm91iaVHM%2FeFXuSdkhpCvPSksqHc5dM1%2Fimg.png">

```JS
// c에 기본값 설정
const [a,b,c = 3] = [1,2];
console.log(a,b,c); // 1 2 3

// 기본값과 할당값이 공존할 경우 할당된 값이 더 우선된다.
const [d,e = 4, f = 6] = [4,8];
console.log(d,e,f); // 4 8 6
```

배열 구조 분해 할당을 위한 변수에 기본값(초기값)을 설정할 수 있다. 기본값과 할당값이 공존할 경우에는 할당된 값이 더 우선시된다.

```JS
// rest 요소로 변수 a를 제외한 나머지 요소 전체를 출력함
const [a, ...c] = ['apple', 'banana', 'carrot'];
console.log(a,c); // apple [banana,carrot]
```

배열 구조 분해 할당을 위한 변수에 rest 파라미터와 유사하게 rest 요소를 사용할 수 있다. rest 요소는 반드시 마지막에 위치해야 한다.

---

<br/>

### 📌 객체 구조 분해 할당

<br/>

```JS
// ES5
var person = { name: 'SM', age: 30 };

var name = person.name;
var age = person.age;

console.log(name, age); // SM 30
```

ES5에서는 객체의 각 프로퍼티를 객체로부터 구조 분해하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.

```JS
// ES6
const person = { name: 'SM', age: 30 };

const { name, age } = person;
console.log(age,name); // 30 SM
```

ES6에서의 객체 구조 분해 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.
<br/>
이 때, 객체 구조 분해 할당의 대상(할당문의 우변)은 객체여야 하며 할당 기준은 프로퍼티 키이다.

```JS
// 1
const { age, name } = { name: 'SM', age: 30 };

// 2
const { age: age, name: name } = person;
const { age, name } = person;

const person = { name: 'SM', age: 30 };
const { name: n, age: a } = person;

console.log(a,n); // 30 SM
```

1. 배열 구조 분해 할당과 마찬가지로 할당 연산자 왼쪽에 프로퍼티 값을 할당받을 변수를 선언해야하고, 변수는 객체 리터럴 형태로 선언한다.

2. 객체 리터럴 형태로 선언한 변수는 프로퍼티 축약 표현을 통해 선언할 수 있다. 프로퍼티 키와 변수명이 같으면 생략이 가능하다. 따라서 객체의 프로퍼티 키와 다른 변수 이름으로도 프로퍼티 값을 할당받을 수 있다.

<br/>

우변에 객체 또는 객체로 평가될 수 있는 표현식(문자열, 숫자, 배열 등)을 할당하지 않으면 아래와 같은 에러가 발생한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCAhYv%2Fbtsm8Q2MiXX%2FPkn03oPWUSnjrl8nfVknO0%2Fimg.png">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbz8gxd%2Fbtsm9fVshAn%2FmE779XfUB1SECl4EKBNFqK%2Fimg.png">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJcDIb%2Fbtsm8PQjEdn%2Fxd3FdFMPK5koceSuO0nazK%2Fimg.png">

```JS
const { name: 'MIA', age } = { age: 29 };
console.log(name,age); // MIA 29

const { name: n = 'MIA', age: a } = { age: 29 };
console.log(n,a); // MIA 29
```

객체 구조 분해 할당을 위한 변수에 기본값을 설정할 수 있다.

```JS
// 1
const str = 'JavaScript';
const { length } = str;
console.log(length); // 10

const schedule = { id: 1, content: '영화보기', isCompleted: true };
const { id } = schedule;
console.log(id); // 1

// 2
const printSchedule = (schedule) => {
	console.log(`오늘의 할 일은 ${schedule.content}인데, 현재 ${schedule.isCompleted ? '완료' : '미완료'} 상태입니다!`)
}

printSchedule({ id: 1, content: '영화보기', isCompleted: true }); // 오늘의 할 일은 영화보기인데, 현재 완료 상태입니다!

// 3
const printSchedule = ({ content, isCompleted }) => {
	console.log(`오늘의 할 일은 ${content}인데, 현재 ${isCompleted ? '완료' : '미완료'} 상태입니다!`)
}

printSchedule({ id: 1, content: '영화보기', isCompleted: false }); // 오늘의 할 일은 영화보기인데, 현재 미완료 상태입니다!
```

1. 객체 구조 분해 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하게 사용할 수 있다.

2. 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.

3. 객체를 인수로 전달받는 함수의 매개변수에 객체 구조 분해 할당을 사용하여 더 간단하고 가독성 좋게 표현이 가능하다.

```JS
const schedule = [
	{id: 1, content: '영화보기', isCompleted: true},
    {id: 2, content: '스터디하기', isCompleted: false},
    {id: 3, content: '운동하기', isCompleted: true},
];

const [, ,{content}] = schedule;
console.log(content); // 운동하기
```

배열의 요소가 객체인 경우 배열 구조 분해 할당과 객체 구조 분해 할당을 혼용할 수 있다.

```JS
const person = {
	name: 'SM',
    age: 30,
    address: {
    	city: 'Incheon',
        number: '032',
    }
};

const { address: { number } } = person;
console.log(number); // 032
```

중첩 객체는 위와 같이 사용된다.

```JS
// rest 프로퍼티로 a 프로퍼티 키의 프로퍼티 값을 제외한 나머지 프로퍼티 전체를 출력함
const { a, ...c } = {a: 'apple', b: 'banana', c: 'carrot'};
console.log(a,c); // apple { b: 'banana', c: 'carrot' }
```

객체 구조 분해 할당을 위한 변수에 rest 파라미터나 rest 요소와 유사하게 rest 프로퍼티를 사용할 수 있다. rest 프로퍼티는 반드시 마지막에 위치해야 한다.
