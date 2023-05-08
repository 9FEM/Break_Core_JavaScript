# call 메서드

<aside>
💡 this에 상황별로 어떤 값이 바인딩 되는지 살펴봤는데,
이러한 규칙을 깨고 this에 별도의 대상을 바인딩 할 수 있습니다.

</aside>

**call 메서드는 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령**입니다.

call의 구문.

```jsx
func.call(thisArg[, arg1[, arg2[, ...]]])
```

call 메서드는 첫 번째 인자를 제외한 나머지 모든 인자들 함수의 매개변수로 지정합니다.

```jsx
var func = function(a,b,c){
	console.log(this, a, b, c);
};
func(1,2,3); // window(.../.../) 1 2 3
func.call({x : 1},4,5,6); // {x : 1} 4 5 6
```

위 코드에서 볼 수 있듯이 func()의 this는 전역을 가르키고 있습니다.

하지만 call을 사용해 임의의 객체를 this로 지정할 수 있습니다.

```jsx
var obj = {
	a : 1,
	method: function(x,y){
		console.log(this.a, x, y);
	}
};

obj.method(2,3); // 1 2 3
obj.method.call({a:4},5,6); // 4 5 6
```

위 코드에서도 마찬가지로 첫번째 obj.method 호출에서는 객체 안에 있는 a를 참조하지만

call 메서드를 사용하면 임의의 객체인 a:4를 참조하는 것을 볼 수 있습니다.
