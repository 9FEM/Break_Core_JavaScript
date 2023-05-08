## 1. 유사배열객체에 배열 메서드 적용

객체는 배열 메서드를 직접 사용할 수 없다. 하지만,

키가 0 또는 양의 정수인 프로퍼티가 존재하고 length 프로퍼티의 값이 0또는 양의 정수인 객체 

즉, 배열의 구조와 유사한 객체(유사배열객체)의 경우에는 `call` 또는 `apply` 메서드를 이용해서 배열 메서드를 사용할 수 있다. 

<br>

<aside>

💡 유사 배열 객체를 직접 만들 수 있다. 객체와 동일하게 작성하지만, 꼭 length 프로퍼티가 들어가야한다.

</aside>

![image](https://user-images.githubusercontent.com/88657261/236718606-11215b09-5f31-4f4b-8a83-635bb3bb6d3e.png)

- 7번줄에서 배열 메서드인 `push` 를 객체 `도하` 에 적용해 프로퍼티 3에 치킨 값을 추가
- 9번줄에서 `slice` 메서드를 사용해 객체를 배열로 반환.

`slice` 메서드는 매개변수를 아무것도 넘기지 않을 경우에는 그냥 원본 배열의 얕은 복사본을 반환한다. 즉, `call` 메서드를 이용해서 원본인 유사배열객체의 얕은 복사를 수행한 것이다. `slice` 메서드가 배열 메서드이기에 복사본은 배열로 반환 되었다. 함수 내부에서 접근할 수 있는 arguments 객체도 유사배열객체이므로 위의 방법으로 배열로 전환해서 활용할 수 있다. 

<aside>
  
✏️ `querySelectorAll`, `getElementsByClassName` 등의 Node 선택자로 선택한 결과인 NodeList도 마찬가지

</aside>

<br>

### NodeList에 배열메서드 적용

<aside>
  
💡 유사배열 객체란?

배열처럼 인덱스를 사용하여 요소에 접근할 수 있고, `length` 프로퍼티가 있지만, 배열이 아닌 객체

대표적인 유사 배열객체로는 `arguments` 가 있다. `arguments` 는 함수 내에서 해당 함수에 전달된 인자들을 나타내고, 배열처럼 인덱스를 사용하여 접근할 수 있고 length 프로퍼티가 있다. 또한 DOM API에서 반환되는 `NodeList` 객체가 있다. 

</aside>

<br>

```html
<body>
  <div>a</div>
  <div>b</div>
  <div>c</div>
</body>
```

```jsx
let nodeList = document.querySelectorAll('div');
console.log(nodeList);
// NodeList(3)
// 0: div, 1: div, 2: div, length: 3
let nodeArr = Array.prototype.slice.call(nodeList);
console.log(nodeArr);
// Array(3)
// 0: div, 1: div, 2: div, length: 3

nodeArr.forEach((node) => {
  console.log(node);
  // <div>a</div>
  // <div>b</div>
  // <div>c</div>
});
```

위의 예제를 보면,

- HTML body 내에 있는 div를 querySelectorAll로 불러오기 (NodeList)
- nodeList를 `call`을 이용해, `slice` 배열 메서드를 사용하여 배열로 복사
- 복사한 배열 nodeArr에 `forEach`메서드를 사용해 각 요소에 `console.log` 함수 사용

유사배열 객체에는 `call` `apply` 메서드를 이용해 모든 배열 메서드를 적용할 수 있다. 

<br>

<aside>
  
💡 `forEach`는 Array.prototype 메소드이지만, 메소드 자체가 배열에서 유사 배열 객체, 즉 인덱스와 length 속성이 있는 객체에도 적용할 수 있도록 구현되어있다. 이와 같은 이유로 유사 배열 객체에 바로 사용할 수 있다.

</aside>

<br>

### 문자열에 배열 메서드 적용

배열처럼 인덱스와 length 프로퍼티를 지니는 문자열도가능하다. 단, 문자열의 length는 읽기 전용이므로 원본 문자열에 변경을 가능하게하는 (`push`, `pop`, `shift`, `unshift`, `splice` 등)은 에러를 던지며, `concat`처럼 대상이 반드시 배열이어야 하는 경우에는 에러는 나지 않아도 제대로 된 결과를 얻을 수 없다. 

```jsx
let 식사 = "햄버거 피자";

Array.prototype.push.call(식사, '치킨'); // error

Array.prototype.concat.call(식사, 'string'); // [String, '치킨']

Array.prototype.every.call(식사, (char) => char !== ' '); // false

Array.prototype.some.call(식사, (char) => char === ' '); // ture

let 새로운식사 = Array.prototype.map.call(식사, (char) => char + '!');
console.log(새로운식사); // ['햄!', '버!', '거!', ' !', '피!', '자!']

let 새로운밥상 = Array.prototype.reduce.apply(식사, [
function(string, char, i) { return string + char + i; },
''
])
console.log(새로운밥상); // 햄0버1거2 3피4자5
```

- `Array.prototype.concat()` : 배열을 결합(concatenate)하여 새로운 배열을 반환, 배열과 문자열 및 유사 배열 객체와 같은 다른 타입의 인자도 받을 수 있다. 이러한 경우 해당 인자를 배열로 변환한 뒤, 이를 원본 배열에 추가한 새로운 배열을 반환.
- `Array.prototype.every()` : 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트, 반환 값은 Boolean.
- `Array.prototype.some()` : 배열 안의 어떤 요소라도 주어진 판별 함수를 하나라도 통과하는지 테스트. 배열에서 주어진 함수가 true를 반환하면 true. 그렇지 않으면 false를 반환. 배열을 변경하지 않음
- `Array.prototype.map()` : 배열 내의 모든 요소 각각에 주어진 함수의 호출한 결과를 모아 새로운 배열로 반환.
- `Array.prototype.reduce()` : 배열의 각 요소에 대해 주어진 리듀서 함수를 실행하고, 하나의 결과값을 반환.

call 과 apply 를 사용해 형변환하는 것은 this를 원하는 값으로 지정해 호출한다라는 의도와는 떨어져 있다. 직접 코드를 작성해본 경험이 있지 않는 한 결과값을 유추하기가 힘들 수 있다. 그렇기 때문에 ES6에서 도입된 Array.from 메서드를 사용할 수 있다. 

<br>

### Array.from 메서드 적용

- `Array.from()` 유사배열객체 및 순회 가능한 모든 종류의 데이터 타입을 배열로 전환. 두 개의 매개변수를 가질 수 있으며 첫 번째 매개변수는 배열로 변환하려는 유사 배열 객체나 iterable 객체이고, 두 번째 매개변수는 선택적으로 배열 요소에 대한 매핑 함수이다. 이는 `Array.prototype.map()` 과 동일하게 작동한다.

```jsx
let 점심시간 = {
  0: '배',
  1: '고',
  2: '파',
  length: 3
};

let 점심시간배열 = Array.from(점심시간);
console.log(점심시간배열);
// [ '배', '고', '파' ]
```

위 예제처럼 `Array.from()` 과 `Array.prototype.slice.call()` 모두 유사한 방식으로 유사 배열 객체나 iterable 객체를 배열로 변환할 수 있다. 하지만 `Array.from()` 이 더 직관적이고 간단하다. `Array.prototype.slice.call()` 은 매우 구식이고, 작성하는 데 필요한 코드양이 많고 직관적이지 않아 가독성이 떨어진다. 가독성과 간결성을 고려할 때는 `Array.from()`을 사용하는 것이 좋다. 

<br>

## 2. 생성자 내부에서 다른 생성자를 호출

`call` 과 `apply` 를 사용하면 this를 바인딩할 객체를 지정한 상태로 함수를 호출

<br>

<aside>
  
💡 생성자 내부에 다른 생성자와 공통된 내용이 있을 경우 call 또는 apply를 이용해 다른 생성자를 호출하면 간단하게 반복을 줄일 수 있습니다.

</aside>

`Child` 생성자 함수에서 `Parent` 생성자 함수를 호출하는 방법

```jsx
function Parent(name) {
  this.name = name;
}

function Child(name, age) {
  Parent.call(this, name);  
  this.age = age;
}

var child = new Child("Alice", 10);
console.log(child); //{name: "Alice", age: 10 }
```

- `Child` 생성자 함수에서  `Parent.call(this, name)`을 사용하여 `Parent` 생성자 함수를 호출
- **this**를 첫 번째 인자로 받으며, 이 인자를 통해 `Parent` 생성자 함수 내부에서 `this`가 `Child` 객체를 가리키도록 합니다.

**Person, Student, Employee 생성자** 

```jsx
function Person(name, gender){
	this.name = name;
	this.gender = gender;
}

function Student(name, gender, school){
	this.school = school;
}

function Employee(name, gender, company){
  this.company = company;
}
```

**Person 에 정의되어 있는 인스턴스 멤버 name, gender를 가져오고 싶은 경우, call / apply 사용**

```jsx
function Student(name, gender, school){
	Person.call(this, name, gender); //= Person.apply(this,[name,gender])
	this.school = school;
}

function Employee(name, gender, company){
	Person.apply(this, [name, gender]);
  this.company = company;
}

var by = new Student('보영', 'female', '단국대');
var jn = new Employee('재난', 'male', '구글');
```

- 생성자 `Student`, `Employee`는 생성자 `Person`에 대해 `call/apply`를 이용해 호출
- `Student`, `Employee` 생성자 내부에서 `Person.call(this, name, gender)`을 호출하면 이 때의 `this`는 현재 생성된 `Student`, `Employee` 객체에 바인딩 됩니다.
- 즉, `this`를 통해 멤버를 정의하면 그 멤버는 `Student`, `Employee` 인스턴스의 멤버가 되는 것입니다.

```jsx
console.log(by);  // Student {name: "보영", gender: "female", school: "단국대"}
console.log(jn); // Employee {name: '재난', gender: 'male', company: '구글'}
```

<aside>
  
💡 위와 같이 `Person` 생성자를 `Student` 와 `Employee`생성자 내부에서 호출하면,  코드의 재사용성과 유지보수성을 높일 수 있습니다.

</aside>

<br>

## 3. 여러 인수를 묶어 하나의 배열로 전달하고 싶을 때

### 3-1. 배열에서 최대/최솟값을 구해야 할 경우 apply를 사용하지 않을 때

예를 들어, 배열에서 최대/최솟값을 구해야 할 경우 어떠한 메소드를 사용하지 않는다면 다음과 같은 방식으로 직접 구현하게 될 것이다.

![image](https://user-images.githubusercontent.com/88657261/236718703-2e575598-9546-4623-af8a-3eb33df63b3b.png)

이보다는 `Math.max`/`Math.min` 메서드에 **apply**를 적용하면 훨씬 간단해진다.

<br>

### 3-2. 배열에서 최대/최솟값을 구해야 할 경우 apply를 사용할 때

```jsx
func.apply(thisArg, [argsArray])
```

apply에서의 두번째 인자는 **인수들의 단일 배열**을 받는다는 특징을 이용하여, 여러 개의 인수를 받는 메서드에게 하나의 배열로 인수들을 전달하고 싶을 때 apply 메서드를 사용하면 좋다. 

![image](https://user-images.githubusercontent.com/88657261/236718721-a38544ac-1308-4b9a-834a-ce0e368c9354.png)

`func.apply(thisArg, [argsArray*배열)`에서, numbers의 배열을 각각의 인자로 받아서 계산 가능하게 해준다.

<aside>
  
💡 Math.max/min 메소드에서는 this 역할이 없기 때문에 첫번째 인자에 들어가는 값은 중요하지 않다. null 자리에는 아무거나 써도 상관없으며, 숫자를 쓰던, undefined를 쓰던 정상적으로 값을 산출한다.

</aside>

<aside>
  
💡 그러나 만약

```jsx
*Math.max(numbers) // => undefined*
```

숫자만 arguments로 받을 수 있기 때문에 놉.

</aside>

<br>

### 3-2. 배열에서 최대/최솟값을 구해야 할 경우 전개구문을 사용할 때

근데 사실? 자바스크립트에서는 펼치기 연산자(전개 구문)를 이용하면 apply를 적용하는 것보다 더욱 간편하게 작성할 수 있다.

<aside>
  
💡 전개구문은 배열을 펼쳐서 인수로 전달하는 방법

</aside>

![image](https://user-images.githubusercontent.com/88657261/236718765-895ccb6c-b6d0-412f-9221-a3b2521ecec4.png)

전개구문을 이용한 깔끔한 방법

![image](https://user-images.githubusercontent.com/88657261/236718784-5d677248-90d2-4fea-97c4-a4d8d6732289.png)

전개구문 + call을 사용한 방법

<br>

### 3-4. call/apply 정리

call/apply 메서드는 명시적으로 별도의 this를 바인딩하면서 함수 또는 메서드를 실행하는 훌륭한 방법이지만 오히려 코드의 복잡성을 증가시키고, `this`가 가리키는 값을 예측하기 어렵게 만들어 코드 해석을 방해한다는 단점이 있다.

![image](https://user-images.githubusercontent.com/88657261/236718819-27571f81-f7df-4314-9253-03195819e327.png)

<aside>
  
💡 따라서 이러한 상황에서는 `bind()` 메서드를 사용하여 `person` 객체에서 호출할 수 있는 새로운 함수를 생성하는 것이 더 명시적이고 예측 가능하다.

`bind()` 메서드 : `call`과 비슷하지만 즉시 호출하지 않고 받은 `this`랑 인수들로 새로운 함수를 반환하기만 하는 메서드

![image](https://user-images.githubusercontent.com/88657261/236718838-d8f9d773-174e-4db1-a403-be4d1e4d1b47.png)

</aside>
