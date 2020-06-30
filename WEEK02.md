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