## flow control

### if 문

``` js

    if (수식1){
        // to do
    }

    if (수식1){

    }else{

    }

    if (수식1){

    }else if(수식2){

    }
```

### switch 문

``` js
    switch(수식1){
        case 값1:

            break;
        case 값2:

            break;
        default:

            break;
    }
```


### for 문

``` js
    // (초기식; 조건식; 갱신식)
    for (var i = 0; i < 10; i++){       // let i = 0;
        // to do
    }
```

- **for in** 과 **for of** 반복.
- for-in 은 배열의 인덱스가 되고, for-of 는 배열의 요소가 된다.
- for-of 는 **ECMAScript 6** 에서 추가됨.

``` js
    var arr = [10, 20, 30, 40];

    for (var i in arr){       // i => 0, 1, 2, 3
        alert(i);
    }

    for (var i of arr){       // i => 10, 20, 30, 40
        alert(i);
    }
```

### while 문

``` js
    var val = 0;
    while (val < 10){
        // to do
        val++;
    }

    // do-while
    do {
        // to do
    }while(수식);
```

- 반복문과 함께 사용하는 `break` , `continue` 에 대해서 알아 두자.

### array

``` js
    var arr = [];            // arr => 객체
    var arr = [1, 2, 3, 4];
    var arr = [123, '123', true, function() {}, {}, [1, 2, 3, 4]];

    var len = arr.length;   // arr 길이
```

- 배열에 요소를 추가할 때  `push()` 메소드를 사용할 수 있다.

``` js
    var arr = [];
    for( var i = 0; i < 5; i++)
        arr.push(i);
    
    alert(arr);
```
