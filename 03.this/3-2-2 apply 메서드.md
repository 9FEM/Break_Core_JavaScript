# apply 메서드

apply 메서드는 call 메서드와 기능적으로 완전히 동일합니다.

둘의 차이점을 보면

apply의 구문.

```jsx
func.apply(thisArg, [argsArray])
```

apply메서드는 **두 번째 인자를 배열로 받아** 그 배열의 요소들을 호출할 함수의 매개변수로 지정하는 점에서만 차이가 있습니다.

```jsx
var func = function (a, b, c) {
  console.log(this, a, b, c);
};
func.apply({ x: 1 }, [4, 5, 6]); //{x:1} 4 5 6

var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  },
};
obj.method({ a: 4 }, [5, 6]); //4 5 6
```

위의 코드를 보면 call과 마찬가지의 결과가 나오지만 두 번째 인자부터 배열로 받는 모습을 볼 수 있습니다.
