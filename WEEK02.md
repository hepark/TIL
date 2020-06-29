# TIL
오늘 내가 배운 것들(Today I Learned)   

---

#### 2주차 질문

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

      2. apply() 메서드 (바로 실행)
      thisIsFunction.apply(document.documentElement, [10, 20, 30]);

      // ES 5+
      3. bind() 메서드 (바로 실행되지 않음)
      var onClickFn = thisIsFunction.bind(document, 10, 20, 30);
      document.addEventListener('click', onClickFn);

      document.addEventListener('click', thisIsFunction.bind(document, 10, 20, 30));
      ```

</div>
</details>