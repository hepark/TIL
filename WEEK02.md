# TIL
오늘 내가 배운 것들(Today I Learned)   

---

#### 2주차 질문
- Q. forEach를 공부하다가 야무님이 사용하신 예제 중에 for 문 부분이 이해가 안 되서 문의드립니다.
  ```javascript
  var members = ['a', 'b', 'c'];
  for(var i=members.length; members[--i];) {
      console.log(members[i]);
  }
  ```
  https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for<br>
  MDN 사이트를 보면 for 반복문의 3개 식은 선택사항이라고 하지만, 그 영역을 표시하지 않으면 에러가 생기는데 야무님 소스는 2개밖에 없어서요.
  ```javascript
  //MDN 소스
  var i = 0;
  for (; i < 9; i++) {
      console.log(i);
  }

  for (var i = 0;; i++) {
   console.log(i);
   if (i > 3) break;
  }
  ```
<details open>
<summary>1일차 학습</summary>
<div markdown="1">

  #### [함수 객체]
  - 함수 가이드
    - 함수 정의(선언/표현식)
      ```javascript
        선언: function
        표현식(참조): var
      ```
    
    - 함수 실행(호출, ( ))
    - 함수 결과반환 or 종료(return)
      ```javascript
        function sqare(x){
          if(!x || typeof x !== 'number'){
              return; // 종료
          }
          return Math.pow(x, 2);  // 반환
        }

        sqare(20);
        400
        sqare("20");
        undefined //함수는 기본적으로 undefined를 반환한다.
      ```

    - 함수 재귀(Recursion) => break가 필요.
      ```javascript
      function loop(n) {
        if ( n > 15 ) { return; }
        loop(++n); // loop(11), loop(12), ...
      }

      loop(10); // 11, 12, 13, 14, 15, 16
      ```

    - 함수 스택(Stack)
      ```javascript
        // LIFO(Last In First Out)
        // DevTools > Sources > Call Stack(with Breakpoint) 확인
        function foo(i) {
          if (i < 0)
            return;
          console.log('begin:' + i);
          foo(i - 1);
          console.log('end:' + i);
        }

        foo(3);
      ```
    - 함수 실행 주체 참조(this) : 
      함수는 객체의 소유, 객체는 소유된 함수의 주인(this), 함수 = 객체의 method

    - 전달인자 객체(arguments, 유사배열(Array-like Object))
      ```javascript
        function sum(x,y){
          return x+y;
        }
        sum(10,3);
        13

        sum(21, -10, 8, 9);
        11

        function sum(){
          console.log(arguments);
        }
        sum(21,-10,8,9);
        VM7624:2 Arguments(4) [21, -10, 8, 9, callee: ƒ, Symbol(Symbol.iterator): ƒ]

        function sum(){
          console.log(arguments[0], arguments[3]);
        }
        sum(21,-10,8,9);
        VM7844:2 21 9

        function sum(){
          console.log(arguments.length);
          console.log(arguments.pop);
        }

        sum(21,-10,8,9);
        VM8779:2 4
        VM8779:3 undefined

        function sum(){
          for ( var total=0, i=arguments.length, arg; (arg=arguments[--i]); ){
            console.log(arg);
            total += arg;
          }
          return total;
        }

        sum(21, -10, 8, 9, 1, 2, 3, 4, 5, 6);

        function myConcat(separator) {
          var result = "";
          var i = 1, l = arguments.length; 

          for(; i<l; i++) {
            result += arguments[i] + (i<l-1 ? separator: '');
          }
          return result;
        }

        myConcat ('//', 'html', 'css', 'javascript', 'java', 'C++');
      ```
  ---------------------------------------

  - 함수 용어
    - 이름이 없는 함수 (표현식)
    - 이름이 있는 함수 (선언)
    - 중첩하는(바깥) 함수, 중첩된(안쪽) 함수 (클로저)
    - 재귀(Recursion)호출 함수
    - 즉시 실행 함수 표현식 (IIFE)
    - 콜백 함수 : 콜백 함수는 코드를 통해 명시적으로 호출하는 함수가 아니라, 개발자는 단지 함수를 등록하기만 하고, 어떤 이벤트가 발생했거나 특정 시점에 도달했을 때 시스템에서 호출되는 함수를 말한다.
  
  - 함수 객체의 속성과 메서드
    ```javascript
      function thisIsFunction(you) {
      console.log('name ⥤', thisIsFunction.name);
      console.log('length ⥤', thisIsFunction.length);
      console.log('caller ⥤', thisIsFunction.caller);  //사용 권장하지 않음, 함수 이름으로 재귀해야
      console.log('%c----------------------', 'color: #3527d5'); //console을 css로 꾸미는 법
      console.log('arguments.length:', arguments.length);
      console.log('this ⥤', this);
    }
    ```

    - 메서드 활용
      ```javascript
      1. call() 메서드 (바로 실행)
      thisIsFunction.call(document.body, 10, 20, 30);

      1. apply() 메서드 (바로 실행)
      thisIsFunction.apply(document.documentElement, [10, 20, 30]);

      // ES 5+
      1. bind() 메서드 (바로 실행되지 않음)
      var onClickFn = thisIsFunction.bind(document, 10, 20, 30);
      document.addEventListener('click', onClickFn);

      document.addEventListener('click', thisIsFunction.bind(document, 10, 20, 30));
      ```

</div>
</details>

<details open>
<summary>2일차 학습</summary>
<div markdown="2">

  #### [배열 객체] 
  - 배열 객체 생성
    ```javascript
    [] 또는 new Array()
    ````
  - 인덱스(index)로 배열 아이템에 접근
    ```javascript
    array[index]
    ````
  - 배열 객체 아이템들을 순환 처리
    ```javascript
    array.forEach(function(member, index){
      i = index, array[i] = member
    });
    ```
  - 배열 객체에 새로운 아이템 추가 (Last In)
    ```javascript
    members.push({
      gender: '남성'
    });
    ```
  - 배열 객체의 마지막 아이템 제거
    ```javascript
    array.pop();
    ```
  - 배열 객체에 새로운 아이템 추가 (First In)
    ```javascript
    array.unshift();
    ```
  - 배열 객체의 첫번째 아이템 제거
    ```javascript
    array.shift();
    ```
  - 배열 객체 아이템 인덱스 찾기
    ```javascript
    // array.indexOf(item)
    var a = [2,9,22];
    a.indexOf(2);
    0
    a.indexOf(-1);
    -1
    ```
  - 배열 객체 아이템 1개 제거
    ```javascript
    array.splice(index, 1)
    ```
  - 배열 객체 아이템 1개 제거
    ```javascript
    // array.splice(index, n)
    array.splice(1, 0, {name: '야무'});  //원하는 위치에 새로운 데이터 추가
    ```
  - 배열 객체 아이템 1개 제거
    ```javascript
    array.length = 0
    ```
  - 배열 복사
    ```javascript
    // copy_array = array.slice()
    var copy_members = members.slice();
    ```
  - 배열 검증
    ```javascript
    // Array.isArray(array)
    Array.isArray(members)
    true
    ```
  - 배열 순서 정렬
    ```javascript
    // array.sort()
    [-1,20,1,9].sort(function(a,b){
        return a-b;
    });
    (4) [-1, 1, 9, 20]
    [-1,20,1,9].sort(function(a,b){
        return b-a;
    });
    (4) [20, 9, 1, -1]

    members.sort(function(p1, p2){
        return p1.name < p2.name ? -1 : p1.name > p2.name ? 1 : 0;
    });

    var key = 'email';
    undefined
    members.sort(function(p1, p2){
        return p1[key] < p2[key] ? -1 : p1[key] > p2[key] ? 1 : 0;
    });
    ```
  - 배열 순서 뒤집기
    ```javascript
    array.reverse()
    ```
  - slice( )와 splice( )의 차이점 : 
    - slice() 메소드는 begin부터 end 전까지의 복사본을 새로운 배열 객체로 반환한다. 즉, 원본 배열은 수정되지 않는다.
    - splice() 메소드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경한다. 이 메소드는 원본 배열 자체를 수정한다.

  **속성**
  - array.length

  **메서드**
  - 변경 메서드 (원본 배열 데이터 수정) 
    - array.push()
    - array.pop()
    - array.unshift()
    - array.shift()
    - array.reverse()
    - array.sort()
    - array.splice() 
  - 접근 메서드 (원본 배열 데이터 보존)
    - array.concat()
    - array.indexOf()
    - array.lastIndexOf()
    - array.join()
    - array.slice()
    - array.toString() 
  - 반복 메서드
    - array.forEach(function(item, index){})
    - array.map(function(item, index){})
    - array.filter(function(item, index){})
    - array.every(function(item, index){})
    - array.some(function(item, index){})
    - array.reduce(function(item, index){})
</details>

<details open>
<summary>3일차 학습</summary>
<div markdown="2">

  #### [ES6] Arrow Function
  - 화살표 함수 식(arrow function expression)
  - function 표현에 비해 구문이 짧다.
  - 자신의 this, arguments, super 또는 new.target을 바인딩 하지 않는다. 
  - 화살표 함수는 항상 "익명(Anonymous)"이다.
  - 이 함수 표현은 메소드 함수가 아닌 곳에 가장 적당하다. 
  - 생성자 함수로 사용할 수 없다.
    ```javascript
    function isType(o) {
      return Object.prototype.toString.call(o).toLowerCase().slice(8,-1);
    }

    var isType = function(o) {
      return Object.prototype.toString.call(o).toLowerCase().slice(8,-1);
    }

    ES6 // 함수표현식에서만 사용가능, return 생략 가능

    let isType = (o, type) => {
      return typeof o === type;
    };
    let isType = (o, type) => typeof o === type;

    let r1 = isType('hi', 'string');
    let r2 = isType(100, 'string');

    console.log(r1);
    console.log(r2);
    ```
  #### [ES6] Default Parameter
  - 기본 매개변수(Default Parameter)
  - ES6에서는 함수 매개변수 기본 값을 설정할 수 있다.
  - 비구조 할당(Object Destructuring)을 활용할 수 있다.
  
    ```javascript
    // ES5
    function addTwoNumbers(x, y) {
      x = x || 0;
      y = y || 0;
      return x + y;
    }
    // ES6
    function addTwoNumbers(x=0, y=0) {
      return x + y;
    }

    addTwoNumbers(2, 4); // 6
    addTwoNumbers(2);    // 2
    addTwoNumbers();     // 0

    -------

    // ES5
    function isRequired(name) {
      throw new Error(name + '매개변수 전달인자 값을 필수로 요구합니다.');
    }

    // 사용자 전달 값 혹은 기본 매개변수 값 설정 함수
    function defaultParam(param, defaults){
      return typeof param !== 'undefined' ? param : defaults;
    }

    // 지불 내역 계산 함수(가격, 세금, 할인)
    function calcuratePayment(price, tax, discount) {
      if(!price) {isRequired('price'); }
      tax = defaultParam(tax, 0.1);
      discount = defaultParam(discount, 0);
      return Math.floor( price * (1 + tax) * (1 - discount) );
    }
    console.log(calcuratePayment(10000,0.3,0.1));

    //ES6
    function isRequired(name) {
      throw new Error(name + '매개변수 전달인자 값을 필수로 요구합니다.');
    }
    // 지불 내역 계산 함수(가격, 세금, 할인)
    function calcuratePayment(price = isRequired('price'), tax = 0.1, discount = 0) {
      return Math.floor( price * (1 + tax) * (1 - discount) );
    }
    calcuratePayment(10000, 0.1, 0.165);

    //비구조할당
    function isRequired(name) {
      throw new Error(name + '매개변수 전달인자 값을 필수로 요구합니다.');
    }

    function calcuratePayment( {price = isRequired('price'), tax = 0.1, discount = 0 } = {} ){
      return Math.floor( price *(1 + tax) * (1 - discount));
    }
    calcuratePayment({ price: 10000, discount: 0.165 }); // 9185


    ```
  #### [ES6] Rest Parameter
  - 나머지 매개 변수(rest parameter) 구문은 정해지지 않은 수(an indefinite number, 부정수)를 배열로 나타낼 수 있게 한다.
  - 함수의 나머지 매개변수(arguments)를 한데 모아 배열 값으로 사용 가능하다.

  ```javascript
    // ES5
    function sum () {
      // arguments는 배열 객체가 아니라, 유사 배열 객체이다.
      for ( var l = arguments.length, r = 0, n; (n=arguments[--l]); ){
        r += n;
      }
      return r;
    }

    // 전달된 인자의 개수에 상관없이 사용 가능
    sum(1, 3, 10); //14
    sum(29, 102, 7, 203, 10) // 351

    // ES6
    function sum (...nums) {
      // 나머지 매개변수는 배열 객체이다.
      let r = 0;
      nums.forEach(n => r += n);
      return r;
    }

    function n_multiply(r, ...nums) {
      nums.forEach(n => r *= n);
      return r;
    }

    n_multiply(101, 1, 2, 3); 
    n_multiply(29, ...[3, -1, 8, 2]); 
  ```

  #### [ES6] Spread Operator
  - 전개 연산자(...) 는 펼쳐주는 방식이다.
  - 함수 또는 배열 등에서 유용하게 활용된다.
   ```javascript
   var integer = [0, -10, 10];
   var copy_integer = integer.map(function(int) {
     return int;
   });

   console.log(copy_integer);
   console.log(copy_integer === integer);
  
  ------

   let integer = [0, -10, 10];
   let copy_integer = integer.map(int => int);

   console.log(copy_integer);
   console.log(copy_integer === integer);

  ------

   let integer = [0, -10, 10];
   let copy_integer = integer.slice();

   console.log(copy_integer);
   console.log(copy_integer === integer);

  ------

   let integer = [0, -10, 10];
   let copy_integer = [...integer];

   console.log(copy_integer);
   console.log(copy_integer === integer);

  ```
  -  배열(역)순차 결합
  ```javascript
   var integer = [0, -10, 10];
   var decimal = [0.8, 0.43, 0.7823];
   var numbers = integer.slice().concat(decimal); //[0, -10, 10, 0.8, 0.43, 0.7823]

   // 유틸리티 함수
   function combineArray() {
     for (var r=[], i=0, a; (a=arguments[i++]);){
       r = r.concat(a);
     }
     return r;
   }

   function reverseCombineArray() {
     return combineArray.apply(undefined, arguments).reverse();
   }

   var numbers = combineArray(integer, decimal);
   var r_numbers = reverseCombineArray(integer, decimal);

   // ES6: 배열(역)순차 결합
   let integer = [0, -10, 10];
   let decimal = [0.8, 0.43, 0.7823];
   let numbers = [...integer, ...decimal]; //[0, -10, 10, 0.8, 0.43, 0.7823]
   let r_numbers = numbers.reverse(); //[0.7823, 0.43, 0.8, 10, -10, 10, 0]
 

  // 유틸리티 함수
  function combineArrays(a,b) {
    return [...a, ...b];
  }

  function reverseCombineArrays(a, b) {
    return [...a, ...b].reverse();
  }

  let numerics = combineArrays(decimal, integer);
  let r_numerics = reverseCombineArrays(decimal, integer);
  ```
  -  배열 중간 삽입 결합
  ```javascript
   var integer = [3, 6, 9];
   var decimal = [0.9, 0.66];

   // 중간 삽입 결합(인덱스 2 위치에 삽입)
   var numbers = integer.slice(), idx = 2;
   decimal.forEach(function(dec) {
     numbers.splice(idx++, 0, dec)
   })  //[3, 6, 0.9, 0.66, 9]

   console.log(numbers);

   // 유틸리티 함수
   var integer = [3, 6, 9];
   var decimal = [0.9, 0.66];

   function insertCombineArray(o1, n, o2) {
     var c = o1.slice();
     o2.forEach(function(i){
       c.splice(n++, 0, i);
     });
     return c;
   }
   var numbers = insertCombineArray (integer, 2, decimal);

   console.log(numbers);

   // ES6
   let integer = [3, 6, 9];
   let decimal = [0.9, 0.66];

   let numbers = null;
   (numbers = [...integer]).splice(2, 0, ...decimal);

   console.log(numbers);
  ```

  -  Example
  ```javascript
   var members = [
     {
       "gender" : "male",
       "name" : "hudson lewis",
       "email" : "hudson.lewis@example.com",
       "picture" : "https"
     }, {...}
   ];

   var new_members = [
     {
       "gender" : "female",
       "name" : "gina reynolds",
       "email" : "gina.reynolds@example.com",
       "picture" : "https"
     }, {...}
   ]

   var communityManager = {
     _members: members,
     // ES5 : addMembers 메서드 정의
     addMembers: function() {
       var new_members = [].slice.call(arguments);
       new_members.forEach(function(member) {
         this._members.push(member);
       }, this);
     }
   };

   communityManager.addMembers.apply(communityManager, new_members);

   // ES6
   var communityManager = {
     _members: members,
     // ES6 : addMembers 메서드 정의
     addMembers: function(...members) {
       this._members = [...this._members, ...members];
     }
   };
    communityManager.addMembers(...new_members);

    communityManager
   ```
  #### [ES6] Array Additions
  - Array Static Methods
  ```javascript
  // ES5
  // DOM 객체 수집(collection) = NodeList
  // lis 변수에 참조된 값은 length 속성을 가진 유사 배열 객체
  var lis = document.querySelectorAll('ul.demo li');

  function makeArray(o) {
    return Array.prototype.slice.call(o);
  }
  // 유틸리티 함수 makeArray()를 사용하여 lis 유사 배열을 배열로 변경
  makeArray(lis).forEach(function(li) {
    console.log(li); //li 순환
  });

  // ES6  Array.from
  let lis = document.querySelectorAll('ul.demo li');
  Array.from(lis).forEach(li => console.log(li));
  [...lis].forEach(li => console.log(li)); //li 순환
  ```

  ```javascript
  // ES5
  // 0부터 100까지 채운 배열이 필요할 경우?
  var array_101 = [];

  for(var i = 0, l = 100; i<l; i++){
    array_101[i] = i;
  } 
  console.log(array_101);

  // ES6  Array.from
  // Array.from(arrayLike, mapFunc?, thisArg?)
  const array_101 = Array.from(new Array(101), (x, i) => i);

  let arr101 = Array.from(new Array(101), (x,i) => i);
  ```
  - Array.of
    ```javascript
    // ES6
    const data = Array.of(3);
    console.log(data.length);

    let html_children = Array.of(document.body, document.head);

    for(let child of html_children){
      console.log(child);
    } 
    ```

- keys, values, entries
  ```javascript
  // ES5
  var numbers = [100, 103, 108, 105];
  for(var i=0, l=numbers.length; i<l; i++) {
    console.log(i, numbers[i]);
  }

  numbers.forEach(function(n, i){
    console.log(i, n);
  });

  // ES6
  let numbers = [100, 103, 108, 105];

  for(let index of numbers.keys()) {
    console.log(index);
  }
  for(let value of numbers.values()) {
    console.log(value);
  }
  for(let [index, value] of numbers.entries()) {
    console.log(index, value);
  }

  ```

- find
  ```javascript
  // ES5
  var numbers = [100, 103, 108, 105];
  function findItemArray(array, cb) {
    for (var i=0, l=array.length; i<l; i++) {
      if(cb(array[i], i, array)) {return array[i]}
    }
  }

  // 유틸리티 함수를 사용해 조건에 부합하는 첫번쨰 아이템 반환
  var item = findItemArray(numbers, function(item, index, array) {
    return item > 100 && item < 105;
  });
  console.log(item);

  // ES6
  let numbers = [100, 103, 108, 105];

  var item = numbers.find(item => item > 100 && item < 105 );
  console.log(item);

  ```

  - findIndex
  ```javascript
  // ES5
  var numbers = [100, 103, 108, 105];
  function findItemIndexArray(array, cb) {
    for (var i=0, l=array.length; i<l; i++) {
      if(cb(array[i], i, array)) {return i;}
    }
    return -1;
  }

  // 유틸리티 함수를 사용해 조건에 부합하는 첫번쨰 아이템 인덱스를 반환
  var item = findItemIndexArray(numbers, function(item) {
    return item > 105;
  });
  console.log(item);

  // ES6
  let numbers = [100, 103, 108, 105];

  var item = numbers.findIndex(item => item > 105 );
  console.log(item);

  ```

  - indexOf vs findIndex
  ```javascript
  // Array.prototype.indexOf
  [false, 10, NaN, {}].indexOf(10); //1

  // Array.prototype.findIndex
  [false, 10, NaN, {}].findIndex(x => x ===10); //1



  // Array.prototype.indexOf
  [false, 10, NaN, {}].indexOf(NaN); // -1

  // Array.prototype.findIndex
  [false, 10, NaN, {}].findIndex(x => Object.is(x, NaN)); // 2
  ```
</details>

<details open>
<summary>4일차 학습</summary>
<div markdown="1">

  #### [배열 객체의 메서드]
   - forEach
     - 주어진 함수를 배열 요소 각각에 대해 실행
     - 매개변수
        - callback
          - currentValue
          - index(Optional) : 처리할 현재 요소의 인덱스.
          - array(Optional) : forEach()를 호출한 배열.
        - thisArg(Optional) : callback을 실행할 때 this로 사용할 값.
      - 반환값 : undefined
      ```javascript
        const array1 = ['a', 'b', 'c'];

        array1.forEach(element => console.log(element));

        // expected output: "a"
        // expected output: "b"
        // expected output: "c"
      ```
   - map
     - 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환
     - 기존 배열의 값을 변형하지 않는다.
     - map이 시작한 이후 배열에 추가되는 요소들은 callback을 호출하지 않는다.
     - map을 호출한 배열의 중간이 비어있는 경우, 결과 배열 또한 동일한 인덱스를 빈 값으로 유지한다.
     - 매개변수
        - callback
          - currentValue
          - index(Optional) : 처리할 현재 요소의 인덱스.
          - array(Optional) : map()를 호출한 배열.
        - thisArg(Optional) : callback을 실행할 때 this로 사용할 값.
      - 반환값 : 배열의 각 요소에 대해 실행한 callback의 결과를 모은 새로운 배열.
      ```javascript
        const array1 = [1, 4, 9, 16];

        // pass a function to map
        const map1 = array1.map(x => x * 2);

        console.log(map1);
        // expected output: Array [2, 8, 18, 32]
      ```
   - filter
      - 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환
      - 호출한 배열의 값을 변형하지 않는다.
      - map이 시작한 이후 배열에 추가되는 요소들은 callback을 호출하지 않는다.
      - map을 호출한 배열의 중간이 비어있는 경우, 결과 배열 또한 동일한 인덱스를 빈 값으로 유지한다.
      - 매개변수
        - callback
          - element : 처리할 현재 요소
          - index(Optional) : 처리할 현재 요소의 인덱스.
          - array(Optional) : filter()를 호출한 배열.
        - thisArg(Optional) : callback을 실행할 때 this로 사용할 값.
      - 반환값 : 테스트를 통과한 요소로 이루어진 새로운 배열. 어떤 요소도 테스트를 통과하지 못했으면 빈 배열을 반환
      ```javascript
        const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

        const result = words.filter(word => word.length > 6);

        console.log(result);
        // expected output: Array ["exuberant", "destruction", "present"]
      ```
   - slice 
      - 어떤 배열의 begin부터 end까지(end 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환
      - 원본 배열을 변형하지 않는다.
      - end가 생략되면 slice()는 배열의 끝까지(arr.length) 추출
      - 매개변수
        - begin(Optional) : 0을 시작으로 하는 추출 시작점에 대한 인덱스를 의미
        - end(Optional) : 추출을 종료 할 0 기준 인덱스
      - 반환값 : 추출한 요소를 포함한 새로운 배열.
      ```javascript
        const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

        console.log(animals.slice(2));
        // expected output: Array ["camel", "duck", "elephant"]

        console.log(animals.slice(2, 4));
        // expected output: Array ["camel", "duck"]

        console.log(animals.slice(1, 5));
        // expected output: Array ["bison", "camel", "duck", "elephant"]
      ```
   - splice
      - 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가
      - 원본 배열을 변형한다.
      - end가 생략되면 slice()는 배열의 끝까지(arr.length) 추출
      - 매개변수
        - start : 배열의 변경을 시작할 인덱스
        - deleteCount(Optional) : 배열에서 제거할 요소의 수
        - item1, item2, ...(Optional) : 배열에 추가할 요소
      - 반환값 : 제거한 요소를 담은 배열.
      ```javascript
        const months = ['Jan', 'March', 'April', 'June'];
        months.splice(1, 0, 'Feb');
        // inserts at index 1
        console.log(months);
        // expected output: Array ["Jan", "Feb", "March", "April", "June"]

        months.splice(4, 1, 'May');
        // replaces 1 element at index 4
        console.log(months);
        // expected output: Array ["Jan", "Feb", "March", "April", "May"]
      ```
   - sort 
      - 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환
      - 원본 배열을 변형한다.
      - 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따른다.
      - 매개변수
        - compareFunction(Optional) : 정렬 순서를 정의하는 함수.
      - 반환값 : 정렬한 배열.
      ```javascript
        const months = ['March', 'Jan', 'Feb', 'Dec'];
        months.sort();
        console.log(months);
        // expected output: Array ["Dec", "Feb", "Jan", "March"]

        const array1 = [1, 30, 4, 21, 100000];
        array1.sort();
        console.log(array1);
        // expected output: Array [1, 100000, 21, 30, 4]
      ```

      ```javascript
        function compare(a, b) {
          if (a is less than b by some ordering criterion) {
            return -1;
          }
          if (a is greater than b by the ordering criterion) {
            return 1;
          }
          // a must be equal to b
          return 0;
        }
      ```
   - reduce 
      - 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환
      - 원본 배열을 변형한다.
      - 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따른다.
      - 빈 요소는 제외한다.
      - 매개변수
        - callback : 배열의 각 요소에 대해 실행할 함수
          - accumulator : 콜백의 반환값을 누적
          - currentValue : 처리할 현재 요소
          - currentIndex(Optional) : 처리할 현재 요소의 인덱스
          - array(Optional) : reduce()를 호출한 배열
        - initialValue(Optional) : callback의 최초 호출에서 첫 번째 인수에 제공하는 값.
      - 반환값 : 누적 계산의 결과 값.
      ```javascript
        const array1 = [1, 2, 3, 4];
        const reducer = (accumulator, currentValue) => accumulator + currentValue;

        // 1 + 2 + 3 + 4
        console.log(array1.reduce(reducer));
        // expected output: 10

        // 5 + 1 + 2 + 3 + 4
        console.log(array1.reduce(reducer, 5));
        // expected output: 15
      ```
   - isArray
      - 인자가 Array인지 판별
      - 매개변수
        - obj : 검사할 객체.
      - 반환값 : 객체가 Array라면 true, 아니라면 false.
      ```javascript
        Array.isArray([1, 2, 3]);  // true
        Array.isArray({foo: 123}); // false
        Array.isArray('foobar');   // false
        Array.isArray(undefined);  // false
      ```
   - some
      - 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트
      - 빈 배열에서 호출하면 무조건 false를 반환.
      - 매개변수
        - callback : 각 요소를 시험할 함수
          - currentValue : 처리할 현재 요소
          - index(Optional) : 처리할 현재 요소의 인덱스
          - array(Optional) : some을 호출한 배열
        - thisArg(Optional) : callback을 실행할 때 this로 사용할 값.
      - 반환값 : callback이 어떤 배열 요소라도 대해 참인(truthy) 값을 반환하는 경우 true, 그 외엔 false.
      ```javascript
        const array = [1, 2, 3, 4, 5];

        // checks whether an element is even
        const even = (element) => element % 2 === 0;

        console.log(array.some(even));
        // expected output: true

      ```
   - every
      - 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트
      - 빈 배열에서 호출하면 무조건 true를 반환.
      - 매개변수
        - callback : 각 요소를 시험할 함수
          - currentValue : 처리할 현재 요소
          - index(Optional) : 처리할 현재 요소의 인덱스
          - array(Optional) : every를 호출한 배열
        - thisArg(Optional) : callback을 실행할 때 this로 사용할 값.
      - 반환값 : callback이 모든 배열 요소에 대해 참(truthy)인 값을 반환하는 경우 true, 그 외엔 false.
      ```javascript
        const isBelowThreshold = (currentValue) => currentValue < 40;

        const array1 = [1, 30, 39, 29, 10, 13];

        console.log(array1.every(isBelowThreshold));
        // expected output: true

      ```
</details>

<details open>
<summary>5일차 학습</summary>
<div markdown="1">

  #### [Set]
  - 배열 객체와 달리 Set 객체는 아이템 중복을 허용하지 않는다.
  ```javascript     
  // 배열(Array)
  const features = ['modules','arrow function','let, const','rest parameter','modules'];

  console.log(features.length, features[0]); // 5, 'modules'


  // 세트(Set) : new Set([iterable]);
  const features_set = new Set(features);

  console.log(features_set.size, features_set[0]); // 4, undefined
  ```

  - Set 객체 메서드

    - size : 아이템갯수
    - add() : 추가
    - has() : 소유 확인
    - delete() : 제거
    - clear() : 모두 제거


  ```javascript
  // Set 객체 생성
  const set = new Set([3, 9]);

  // 아이템 추가
  set.add(10);

  // 아이템 개수 출력
  console.log(set.size); // 3

  // 아이템 제거
  set.delete(9);

  console.log(set.size); // 2

  // 아이템 소유 확인
  console.log(set.has(9)); // false

  // 아이템 모두 제거
  set.clear();

  console.log(set.size); // 0
  ```
  - Set 객체를 순환하는 메서드

    - forEach()
    - entries()
    - values()
    - keys()
```javascript
  const oneTwoThree = new Set(['하나', '둘', '둘', '셋']);

  oneTwoThree.forEach(n => console.log(n)); // '하나', '둘', '셋'

  for (let item of oneTwoThree.entries()) {
    console.log(item);
    // ['하나', '하나']
    // ['둘', '둘']
    // ['셋', '셋']
  }

  for (let key of oneTwoThree.keys()) {
    console.log(key);
    // '하나'
    // '둘'
    // '셋'
  }

  for (let value of oneTwoThree.values()) {
    console.log(value);
    // '하나'
    // '둘'
    // '셋'
  }
```
  - Set 확장 클래스 
```javascript
class y9Set extends Set {

  // 합집합
  union(x){ return new Set([...this, ...x]) }

  // 교집합
  intersect(x){return new Set([...this].filter(y => x.has(y)))}

  // 차집합
  diff(x){return new Set([...this].filter(y => !x.has(y)))}

  // 상위 집합 유무 확인
  isSuperset(x){
    for (let y of x) {
      if (!this.has(y)) { return false; }
    }
    return true;
  }
  ```
  #### [WeakSet]
  - 객체만 수집할 수 있고 약한 참조가 이루어져 메모리 누수를 예방할 수 있다.
  - 차이점
    - size 속성을 가지지 않습니다.
    - 객체 타입만 .add()하거나, .delete() 할 수 있습니다.
    - forEach, for..in문을 사용해 순환할 수 없습니다.
    - 약한 참조로 메모리 누수 관리에 효과적입니다.
  ```javascript
  // 데이터(객체)
  let arr = [1, 3, 5, 7],
    obj = {key: 'value'};

    // Set 객체 생성
    let set = new Set();

    // WeakSet 객체 생성
    let wset = new WeakSet();

    // 아이템 추가
    set.add(arr).add(obj);
    wset.add(arr).add(obj);

    // 아이템 사이즈
    console.log(set.size);  // 2
    console.log(wset.size); // undefined

    // 객체가 아닌 데이터 추가
    set.add(true);
    wset.add(true); // 오류 발생: Invalid value used in weak set

    // 아이템 소유 여부 확인
    set.has(obj);  // true
    wset.has(obj); // true

    // 아이템 제거
    set.delete(arr);  // true
    wset.delete(arr); // true

    // 세트 순환
    set.forEach(item => console.log(item));  // 참조된 데이터에 접근 및 사용 가능
    wset.forEach(item => console.log(item)); // 오류 발생: wset.forEach is not a function

    // 메모리 참조
    let set  = new Set();
    let wset = new WeakSet();

    (() => {

      let o1 = {a: 1}; // 메모리
      let o2 = {a: 2}; // 가비지 컬렉터에 의해 메모리 삭제

      set.add(o1);
      wset.add(o2);

    });
  ```

  ```javascript
  // WeakSet 객체 생성
  let ownClass = new WeakSet();

  // 클래스 OffCanvasMenu 정의
  class OffCanvasMenu {

    constructor() {
      // 클래스 자신을 ownClass에 추가
      ownClass.add(this);
      // ...
    }

    toggle() {
      // OffCanvasMenu 객체가 아닌,
      // 다른 객체가 toggle() 메서드를 사용하려 할 경우 오류 출력
      if ( !ownClass.has(this) ) {
        throw new TypeError('toggle() 메서드는 OffCanvasMenu 객체만 사용 가능합니다!');
      }
    }
  }                 
  ```