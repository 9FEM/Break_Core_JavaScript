## 스택과 큐 개념

### **스택(STACK)**
<img src="https://user-images.githubusercontent.com/105140201/233772131-6e6d89cb-6472-41ce-9cde-2f16490db298.png" height="50%"> 

- LIFO(Last in, First out) 원칙으로 만들어진 자료구조  →  나중에 들어온 데이터가 먼저 나간다.
- 함수 실행 콘텍스트들이 쌓이는 Call stack과 브라우저의 방문 기록이 쌓이는 History stack이 대표적이다.
- 서로 관계가 있는 여러 작업을 연달아 수행하면서 이전의 작업 내용을 저장해 둘 필요가 있을 때 널리 사용된다.

<aside>
💡 3 가지 기본 동작

- Push : 스택 안으로 데이터를 넣습니다. 만약 스택이 꽉 찬 상태에서 데이터를 더 넣을 경우, Overflow condition(오버플로우 상태)라고 이야기합니다.
- Pop : 스택 안에 있는 데이터를 제거합니다. 아이템은 넣어진 역순으로 빠져나옵니다. 만약 스택이 비어있는 상태에서 데이터를 꺼내려는 경우, Underflow condition(언더플로우 컨디션)라고 이야기합니다.
- Peek or Top : 스택의 꼭대기 요소를 반환합니다.
- isEmpty : 만약 스택이 비어있다면, true를 반환하고, 비어있지 않다면 false를 반환합니다.
</aside>

  예) 프링글스(택!)

    

### **큐(QUEUE)**
<img src="https://user-images.githubusercontent.com/105140201/233772144-850e84df-6053-4d31-a5a2-ad2c68591089.png" width="50%"> 


- FIFO(First in, First out) 원칙으로 만들어진 자료구조  →  먼저 들어온 데이터가 먼저 나간다.
- 큐는 순서대로 처리해야 하는 작업을 임시로 저장해두는 버퍼(buffer)로서 많이 사용된다.

<aside>
💡 4가지 기본 동작

- Enqueue: 큐에 데이터를 추가한다. 만약 큐가 꽉 찼을 때, Overflow condition이 된다.
- Dequeue: 큐에서 데이터를 제거한다. 데이터는 들어온 순서대로 빠져나온다. 만약 큐가 비어져 있을 경우, Underflow condition이 된다.
- Front: 큐의 앞부분의 데이터를 가진다.
- Rear: 큐의 뒷부분의 데이터를 가진다.
</aside>

예) ATEEZ 팬 사인회 큐(시작)

🔍 스택과 큐의 차이점  ⇒  제거

     **스택**은 가장 마지막에 쌓인 데이터를 제거하고, **큐**는 가장 앞에 추가된 데이터를 삭제

## 스택과 큐 구현 실습


### 배열을 이용한 스택 구현

- 사용되는 메서드?
    
    데이터를 추가하는 `push`, 데이터를 추출하는 `pop`
    

```jsx
class Stack {
	constructor() {
		this._arr = [];
	}
push(item) {
		this.arr_push(item);
	}
pop() {
		return this._arr.pop();
	}
peek() {
		return this._arr[this._arr.length - 1];
	}
}

const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3); // 최상위 데이터
stack.pop();   // 3 ==> LIFO(Last In First Out)
```  
<img src="https://user-images.githubusercontent.com/105140201/233772167-1c5c8588-83f9-4b6b-9f92-7779e3df49c6.png" width="30%"> 


### 배열을 이용한 큐 구현

- 사용되는 메서드?
    
    데이터를 추가하는 `push`, 데이터를 추출하는 `shift` 
    

```jsx
class Queue {
	constructor() {
		this._arr = [];
	}
	enqueue(item) {
		this._arr.push(item);
	}
	dequeue() {
		return this._arr.shift();
	}
}

const queue = new Queue();
queue.enqueue(1); // 가장 앞에 위치
queue.enqueue(2);
queue.enqueue(3); 
queue.dequeue(); // 1 ==> FIFO(First In First Out)
```
<img src="https://user-images.githubusercontent.com/105140201/233772551-98e7382c-aed1-4088-bbbb-66994da43bda.png" width="40%"> 



### 스택과 큐를 이용한 알고리즘 문제

**[문제]** 

괄호가 짝지어졌다는 것은’(’ 문자로 열렸으면 반드시 짝지어서 ‘)’ 문자로 닫혀야 한다는 뜻입니다. ‘(’ 또는 ‘)’로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return하는 solution 함수를 완성해 주세요.

**[입출력 예]** 

s = “()()”, answer = true

s = “(())()”, answer = true

s = “)()(”, answer = false

s = “(()(”, answer = false

- 풀이
    
    ```jsx
    function solution(s){    
        const stack = [];
        
        for(let i = 0; i < s.length; i++) {
            if (stack[stack.length - 1] === '(' && s[i] === ')') {
                stack.pop();
            } else {
                stack.push(s[i]);
            }
        }
        
        return !stack.length;
    }
    ```
    

[[프로그래머스] - 올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

## 실행 컨텍스트


### 실행 컨텍스트란?

> `실행 컨텍스트`는 **실행할 코드에 제공할 환경 정보들을 모아놓은 객체**
> 

자바스크립트는 동일한 환경에 있는 환경 정보들을 모은 `실행 컨텍스트`를 
콜 스택에 쌓아 올린 후, 

가장 위에 쌓여 있는 컨텍스트와 관련 있는 코드를 실행하는 스택 구조의 방식 **FILO** (First In, Last Out)대로 실행하여 **순서를 보장,**

콜스택 내부에 쌓인 실행 컨텍스트의 정보를 통해 코드 전체의 **환경을 보장**한다.

### 실행 컨텍스트를 구성하는 방법

<aside>
💡 실행 컨텍스트를 만들 수 있는 방법으로는 1) 전역공간 2) 함수 3) eval() 함수 가 있다.

전역공간은 자동으로 생성되며,  
eval 함수는 문자열로 된 자바스크립트 코드를 전달하면 그게 그대로 실행되는 함수인데, 속도나 보안이 좋지 않아 현재는 거의 쓰지 않는다. ~~없다고 생각하자~~

</aside>

따라서 자동으로 생성되는 전역공간과 eval을 제외하면, 실행 컨텍스트가 생성되는 시점은 함수를 실행하는 시점이다.

자바스크립트 엔진이 스크립트를 처음 마주할 때, **전역 컨텍스트**를 자동으로 생성하고 콜 스택에 `push`한다. 엔진이 스크립트를 쭉 읽어내려가면서 함수 호출을 발견할 때마다 **함수의 실행 컨텍스트**를 스택에 `push` 하는 방식이다.

함수를 선언할 때가 아니라 **실행**할 때!

### 실행 컨텍스트와 콜 스택

```jsx
// -------------------------------- (1)
var a = 1;
function outer(){
	function inner(){
		console.log(a); // undefined
		var a = 3;
	}
	inner(); 
	// --------------------- (3)
	console.log(a);    // 1
}
outer(); 
// ----------------------- (2)
console.log(a);      // 1
```

<img src="https://user-images.githubusercontent.com/105140201/233772581-dabf21d1-f920-4610-8c7e-f14ca160b22d.png" width="50%"> 

- 콜 스택에 실행 컨텍스트가 쌓이는 과정

    <img src="https://user-images.githubusercontent.com/105140201/233772600-24bd5257-eb8a-4ec5-85e9-2324778fa8ed.png" width="50%"> 
    
    1. 처음 자바스크립트 코드를 실행하는 순간 **전역 컨텍스트**가 콜 스택에 담기며 전역 컨텍스트 관련 코드를 진행한다.
        
        <aside>
        💡 최상단의 공간은 코드 내부에서 별도의 실행 명령이 없어도 브라우저에서 자동 실행되기에 자바스크립트 파일이 열리는 순간 전역 컨텍스트가 활성화 되는 것으로 이해하기
        
        </aside>
        
    2. 전역 컨텍스트 관련 코드를 진행 중 outer 함수를 실행하면 자바스크립트 엔진은 **outer에 대한 환경 정보를 수집**해 **outer 실행 컨텍스트**를 생성한 후 콜 스택에 담는다.
        
        → **전역 컨텍스트 관련 코드의 실행을 일시중단`(2)`**한 후 outer 실행 컨텍스트 관련 코드인 outer 함수 내부의 코드들을 순차로 실행한다.
        
    3. outer 함수 내부에서 inner함수를 실행하면 inner 실행 컨텍스트를 생성, 콜 스택의 가장 위에 담는다. 
    → outer 컨텍스트 관련 코드인 **outer 함수 내부의 코드들을 중단`(3)`**하고 inner 함수 내부의 코드를 순서대로 진행한다.
    
- 콜 스택에 실행 컨텍스트가 제거되는 과정

    <img src="https://user-images.githubusercontent.com/105140201/233772667-2ba671f2-b3bc-4278-b2bc-21ebd8095e60.png" width="50%"> 

    
    1. inner 함수 내부에서 a변수를 출력하고 나면 inner 함수의 실행이 종료되면서 **inner 실행 컨텍스트가 콜 스택에서 제거**된다.
        
        outer 컨텍스트가 맨 위에 존재하므로 중단했던 `**(3)**`의 다음 줄부터 이어서 실행, a 변수의 값을 출력한다.
        
    2. 출력하고 나면 outer 함수의 실행이 종료되어 **outer 실행 컨텍스트가 콜 스택에서 제거**되고, 전역 컨텍스트만 남아있게 된다.
    3. 실행을 중단했던 `**(2)**`의 다음 줄부터 이어서 실행한다.
    a 변수의 값을 출력하고 나면 전역 공간에도 더는 실행할 코드가 남아있지 않아 **전역 컨텍스트도 제거**되고, 콜 스택에는 아무것도 남지 않은 상태로 종료된다.

### 실행 컨텍스트의 수집 정보 구조

- `**variableEnvironment**`
현재 컨텍스트 내의 **식별자들에 대한 정보 + 외부 환경 정보**를 담는다. 
선언 시점의 `LexicalEnvironment`의 **스냅샷**으로 변경 사항은 반영되지 않는다.
    - environmentRecord (snapshot) : 현재 컨텍스트 내의 식별자들에 대한 정보
    - outerEnvironmentReference (snapshot) : 외부 환경 정보
- `**LexicalEnvironment**`(어휘적 환경, 정적 환경)
    
    처음에는 `variableEnvironment`와 같지만 변경 사항이 실시간으로 반영된다.
    
    - environmentRecord : 현재 컨텍스트 내의 식별자들에 대한 정보
    - outerEnvironmentReference : 외부 환경 정보
    
    <aside>
    💡 Variable Environment에 정보를 먼저 담고, 이를 그대로 복사해서 Lexical Environment를 만들고 이후 Lexical Environment를 주로 사용한다.
    
    </aside>
    
- `**ThisBinding**`
`this`식별자가 바라봐야 할 대상 객체. 실행 컨텍스트가 활성될 때 this가 지정되지 않은 경우 this에는 전역 객체가 저장된다.
