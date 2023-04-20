# **VariableEnvironment란?**

**VariableEnvironment**(변수환경)는 현재 범위 내에서 선언된 모든 변수와 함수를 저장하는 실행 컨텍스트의 구성 요소입니다.

**함수가 호출될 때 생성**되며 함수 인수, 로컬 변수 및 명명된 함수 식을 포함하여 해당 함수 내에 정의된 모든 변수와 함수로 채워집니다.

Variable Environment는 세 가지 프로퍼티를 가지고 있습니다.

바로 **`environment record`** 와 **`outer environment reference`** 그리고 **`ThisBinding`** 입니다.

**environment record**

- 현재 문맥의 식별자 정보 (변수나 함수 등)
- 변수 객체(VO)

**ThisBinding**

`this` 의 값이 여기서 결정된다. 글로벌 실행 컨텍스트에서 `this` 는 global object 입니다.

함수 실행 컨텍스트에서는 `this`값은 어떻게 함수가 호출되었는지에 따라 달라집니다. 만약 함수가 object reference로 호출되었다면 `this`는 해당 객체를 가리키게 됩니다.

그렇지 않으면 `this`는 글로벌 객체(window)를 가리키거나 strict mode에서는 `undefined`를 가리키고 있습니다.

**outer environment reference**

- 상위 Lexical Environment를 참조하는 포인터.
- 중첩된 자바스크립트 코드에서 스코프 탐색을 하기 위해 사용.

## VariableEnvironment와 LexicalEnvironment 차이?

VariableEnvironment와 LexicalEnvironment에 담기는 내용은 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다릅니다.

실행 컨텍스트를 생성할 때 VariableEnvironment에 먼저 정보를 담고, 이를 그대로 복사해서 LexicalEnvironment를 만들고 이후에는 **LexicalEnvironment를 주로 활용**합니다.

VariableEnvironment는 함수가 호출될 때마다 생성되는 반면 LexicalEnvironment는 함수가 정의될 때 생성됩니다.

VariableEnvironment는 함수 실행 중에 변경할 수 있지만 LexicalEnvironment는 함수 정의 시 고정합니다.

ES6에서 한 가지 차이점이 생기는데,

**VariableEnvironment는 변수 var만 저장**하고

**LexicalEnvironment는 함수 선언과 변수(let과 const) 바인딩을 저장**합니다.
