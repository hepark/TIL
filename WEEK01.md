# TIL
오늘 내가 배운 것들(Today I Learned)   

---

#### 1주차 질문
- ~~Q. let & const 학습내용 중 아래 함수의 동작원리가 이해가 안 됩니다.~~<br>
- Q. 아래 부분이 콜백함수라는 건 찾았는데요.(https://humahumahuma.tistory.com/m/74?category=803785)<br>
혹시 강의 중에 콜백함수에 관해 설명해놓으신 부분이 있을까요?? 
알 것 같다가도 잘 모르겠어요.

```javascript
fn_list.forEach(function(f){
    f();
});
```

<details open>
<summary>1일차 학습</summary>
<div markdown="1">

#### [값 복사 vs 값 참조]
- 값 복사(pass by value)
  - null
  - undefined
  - number
  - string
  - boolean
  
- 값 참조(pass by reference)
  - ojbect
    - array
    - function

- object를 값 복사하는 방법
```javascript
  for( var prop in my_family){
    y[prop] = my_family[prop];
  }
```

- array를 값 복사하는 방법
```javascript
  function copyArray(array) {
    for (var copy=[], i = 0, l = array.length; i<l; i++ ){
      copy[i] = array[i];
    }
    return copy;
  }
```
---------------------------------------

#### [함수 영역 vs 블록 영역]
- 함수 영역(Function Scope)
  - 대부분의 프로그래밍 언어는 함수 내에서만 유효한 지역 변수를 제공한다. 
    함수 영역을 사용하는 지역 변수는 함수가 반환되면 더 이상 사용할 수 없다.

- 블록 영역(Block Scope)
  - 알골과 그의 자손인 C, 그리고 그에 영향을 받은 많은 현대 언어들은 블록 단위 지역 변수를 지원한다.
  - 블록 문 내부에 선언된 변수는 블록 영역에서만 접근 가능.
  - ECMAScript 2015 (ES6) => let 키워드 지원

---------------------------------------

#### [함수 호이스팅]
- **호이스팅**이란? : 변수, 함수 선언이 코드의 실행 이전에 메모리에 등록되는 것을 말합니다. 마치 끌어 올려지는 것과 같다고 하여 호이스트(Hoist)라고 불립니다

#### [스코프 체이닝]
- **스코프 체이닝**이란? 
  - 변수(식별자)를 찾는 일련의 행위
  - 부모 영역까지 올라가서 변수를 찾는 행위
- **변수선언순서**
  - 변수 선언, 함수 선언 ⇒ 초기화(값 할당) ⇒ 변수 허용 영역(Scope) 설정

---------------------------------------

#### [즉시 실행 함수 식/IIFE 패턴]
- 전역을 오염시키지 않기 위해 사용하는 대중적인 방법
- 자바스크립트는 전역을 오염시키지 않기 위해서 함수 scope가 필요하게 됩니다.
- 이 안에 있는 var를 사용한 변수는 지역변수가 되기 때문에 전역을 오염시키지 않는다.
- 이 안에 있는 함수도 지역함수가 되기 때문에 전역을 오염시키지 않는다.
- 그리고 전역에서는 이 내부의 코드에 접근할 수 없다.(보안)
- 함수는 호출을 해야 하는데, IIFE는 호출을 안해도 되기 때문에 전역을 오염시키지 않는다.

- 이는 Self-Executing Anonymous Function 으로 알려진 디자인(설계) 패턴이며, 크게 두 부분으로 구성
  - 첫 번째는 
    괄호((), Grouping Operator)로 둘러싸인 익명함수(Anonymous Function)입니다.
    이는 전역범위를 오염시키는 것 뿐만 아니라 IIFE 내부의 변수에 접근하는 것을 방지합니다.

  - 두 번째는 
    즉시 실행 함수를 실행하는 괄호() 입니다. 이를 통해 자바스크립트 엔진은 함수를 즉시 해석해서 실행합니다.


```javascript
function showMetheMoney(){
    console.log("쇼 미 더 머니");
}
showMetheMoney();

(function showMetheMoney(){
    console.log("쇼 미 더 머니");
})();


IIFE(이름이 없는 익명함수)
(function(){
    console.log("쇼 미 더 머니");
}());
```

<br><br>
---

Q. 외부에서 즉시실행함수 내 변수나 함수 사용 방법<br>
```javascript
// IIFE 패턴으로 감싸진 영역(Scope) 
(function($){ 
  'use strict'; 
  // jQuery 객체의 컨텍스트(context) 객체가 없을 경우 
  // jQuery.Context 객체 생성 
  if (!$.Context) { $.Context = {} } 
  // IIFE 내부에서 사용되는 함수 toggle_class 
  function toggle_class(el, className){ 
    var $el = $(el); 
    $el.on("click", function(e){ 
      e.preventDefault(); 
      $(this).toggleClass(className); 
    }); 
  } 
  // IIFE 외부에서 toggle_class 함수를 사용해야 할 경우  
  // $.Context.toggle_class로 내보냄 
  $.Context.toggle_class = toggle_class 
})(jQuery); 
// 외부에서 jQuery.Context 객체를 통해 toggle_class 함수에 접근 가능 
jQuery.Context.toggle_class(document.body, 'share-contenxt-props');
```

</details>

<details open>
<summary>2일차 학습</summary>
<div markdown="1">

#### [ES6] 블록영역 ⎼ let & const
- **var**
  - 함수영역(function scope)
  - 초기값 할당이 없으면: undefined
  - 가급적 사용하지 않는 것이 좋지만 사용해야 한다면 top level에서만 사용
  - 전역에서 선언시, window 객체의 속성으로 접근 가능
- **let / const**  
  - 블록영역(block scope)
  - 초기 값 할당이 없으면 ReferenceError
  - 데이터 값 변경이 필요한 경우라면 let 사용 권장.
  - 전역에서 선언해도 window 객체의 속성으로 접근 가능하지 않음
- **const** : 
  - 초기 값 할당이 필수!
  - 값 유형 변경은 허용되지 않지만, 배열/객체 유형의 경우 새로운 아이템 추가, 변경 가능
  - 데이터 값 유형이 배열/객체일 경우 사용 권장
- **함수 실행 시점&스코프 체이닝**

#### [ES6] IIFE → Block
IFE 패턴은 일반적으로 변수들을 별도의 영역 안에서만 사용하기 위해 사용되었습니다.<br>
ES6+에서는 Block을 기반으로 영역을 만들 수 있으므로, 더 이상 함수 기반 영역을 사용하지 않아도 됩니다.

```javascript
IIFE 패턴을 사용하는 경우
(function () {
  var food = '허니버터칩';
}());

console.log(food); // Reference Error



ES6+ 블록을 사용하는 경우
{
  let food = '허니버터칩';
};

console.log(food); // Reference Error
```
</details>

<details open>
<summary>3일차 학습</summary>
<div markdown="1">

#### JavaScript 클로저(Closure)
- **클로저란**
  - 함수와 함수가 선언된 어휘적 환경의 조합
  - Lexical Environment : 선언 당시의 환경에 대한 정보를 담는 객체(구성 환경)
  - 최초 선언시의 정보를 기억함

- **실행 컨텍스트(Execution Contenxt)**
  - 실행 컨텍스트는 해당 함수 내의 변수와 해당 부모 환경에 대한 참조를 의미하는 환경으로 구성됩니다. 
  - 어느 시점이든 하나의 실행 컨텍스트만 실행 될 수 있습니다.
    이것이 JavaScript가 "단일 스레드"인 이유입니다.

  - 즉, 한 번에 하나의 명령만 처리 할 수 있습니다. 일반적으로 
  브라우저는 "스택(Stack)"을 사용하여 이 실행 컨텍스트를 유지 관리합니다. 
  스택은 LIFO(Last In First Out) 데이터 구조입니다. 
  
  - 스택에 푸시(push) 한 마지막 것이 가장 먼저 꺼내집니다. 스택의 
  맨 위에 요소만 삽입하거나 삭제할 수 있기 때문입니다. 현재 또는 
  "실행 중인" 실행 컨텍스트는 항상 스택의 맨 위에 있는 항목입니다. 

  - 실행 중인 실행 컨텍스트의 코드가 완전히 평가되면 최상위 항목이 
  팝(pop) 된 다음 실행 항목이 실행 컨텍스트를 실행하는 것으로 
  간주됩니다.
  
  - 또한 실행 컨텍스트가 실행되고 있다고 해서 다른 실행 컨텍스트를 
    실행하기 전에 실행이 완료되어야한다는 것을 의미하지는 않습니다. 
    실행 중인 실행 컨텍스트가 일시 중단되고 다른 실행 컨텍스트가 
    실행 중인 실행 컨텍스트가되는 경우가 있습니다. 

  - 일시중단 된 실행 컨텍스트는 나중에 중단 된 부분을 선택합니다. 
    한 실행 컨텍스트가 이와 같이 다른 컨텍스트로 대체 될 때마다 
    새 실행 컨텍스트가 만들어져 스택에 푸시되고 현재 실행 컨텍스트가 됩니다.
  
  ```javascript
  // 글로벌 실행 컨텍스트 (Global Execution Context)
  var x = 9;

  // 함수 실행 컨텍스트 (outerFn: Execution Context)
  function outerFn() {
    var y = 12;   // 환경 레코딩
    
    // 함수 실행 컨텍스트 (innerFn: Execution Context)
    function innerFn() {
      var z = 6;         // 환경 레코딩
      return x + y + z;  // 환경 레코딩, y는 외부에 대한 참조
    }

    return innerFn;
  }
  ```

  다른 곳에도 비슷한 예제가 있어서 참조해봤습니다.

  ```javascript
  function setName(name) {
    return function() {
      return name;
    }
  }

  var sayMyName = setName('홍길동');
  sayMyName();

  0. 전역 실행컨텍스트 생성[Global]
      1. 함수 setName 선언 [Global > setName]
      2. 변수 sayMyName 선언
      3. setName(홍길동) 호출 -> setName 실행 컨텍스트 생성
          1. 지역변수 name 선언 및 '홍길동' 할당
          2. 익명함수 선언[Global > setName > unnamed]
          3. 익명함수 반환
      4. setName 실행 컨텍스트 종료
      5. 변수 sayMyName에 반환된 함수를 할당
      6. sayMyName 호출 -> sayMyName 실행 컨텍스트 생성
          1. unnamed scope에서 name 탐색 -> setName에서 name 탐색 -> '홍길동' 반환
      7. sayMyName 실행 컨텍스트 종료
  1. 전역 실행컨텍스트 종료
  ```
  
- **실용적인 클로저 활용**

  ```javascript
  // ES5, bind 함수 사용
  function onNavLink(i, e){
    e.preventDefault();
    console.log(i, this);
  }
  for(; i<l; ++i){
    var link = nav_links[i];
      nav_links[i].addEventListener('click', (function(index){
        return onNavLink.bind(link, index);
      })(i));
  }

  // ES6
  for(let i=0, l=nav_links.length; i<l; ++i){
      nav_links[i].addEventListener('click', function(e){
        e.preventDefault();
        console.log(i);
      });
  }
</details>

<details open>
<summary>4일차 학습</summary>
<div markdown="1">

#### 숫자 / 수학 객체
  - [숫자 값(리터럴)의 4가지 유형]
    - 10진수(주로 사용): 0으로 시작 가능.
	    하지만 0다음에 나오는 모든 수가 8보다 작으면 8진수로 해석.
	    *0으로 시작하는 수를 사용하지 않아야 한다.
    - 2진수 : 0 또는 b 또는 B 사용. 0b 이후 숫자가 0 또는 1이 아니면 오류
    - 8진수: 앞에 0 사용. 0 이후 숫자가 범위(0,1,2,3,4,5,6,7)을 벗어나면 10진수로 해석됨.
    - 16진수: 0 또는 x 또는 x 사용. 0x 이후 숫자가 범위(0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f)를 벗어나면 오류.

  - **Number**
  ```javascript
  [속성]
  Number.MAX_VALUE
  1.7976931348623157e+308
  Number.MIN_VALUE
  5e-324
  Number.EPSILON
  2.220446049250313e-16
  Number.MIN_SAFE_INTEGER
  -9007199254740991
  Number.MAX_SAFE_INTEGER
  9007199254740991

  [메서드]

      [스태틱 메서드 ] - ie 미지원
          Number.parseFloat()
          Number.parseInt()
          Number.isFinite()
          Number.isInteger()
          Number.isNaN()
          Number.isSafeInteger()
      [인스턴스 메서드]
          .toExponential()
          .toFixed()
          .toPrecision()

          (1203).toExponential()
          "1.203e+3"
          (12.01020100001).toFixed(2)
          "12.01"
          Number((12.01020100001).toFixed(2))
          12.01
          (12.01020100001).toPrecision(6)
          "12.0102"

          window.parseFloat
          ƒ parseFloat() { [native code] }
  ```
  - **Math**
  ```javascript
    Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}abs: ƒ abs()acos: ƒ acos()acosh: ƒ acosh()asin: ƒ asin()asinh: ƒ asinh()atan: ƒ atan()atanh: ƒ atanh()atan2: ƒ atan2()ceil: ƒ ceil()cbrt: ƒ cbrt()expm1: ƒ expm1()clz32: ƒ clz32()cos: ƒ cos()cosh: ƒ cosh()exp: ƒ exp()floor: ƒ floor()fround: ƒ fround()hypot: ƒ hypot()imul: ƒ imul()log: ƒ log()log1p: ƒ log1p()log2: ƒ log2()log10: ƒ log10()max: ƒ max()min: ƒ min()pow: ƒ pow()random: ƒ random()round: ƒ round()sign: ƒ sign()sin: ƒ sin()sinh: ƒ sinh()sqrt: ƒ sqrt()tan: ƒ tan()tanh: ƒ tanh()trunc: ƒ trunc()E: 2.718281828459045LN10: 2.302585092994046LN2: 0.6931471805599453LOG10E: 0.4342944819032518LOG2E: 1.4426950408889634PI: 3.141592653589793SQRT1_2: 0.7071067811865476SQRT2: 1.4142135623730951clamp: ƒ clamp()DEG_PER_RAD: 0.017453292519943295degrees: ƒ degrees()fscale: ƒ fscale()iaddh: ƒ iaddh()imulh: ƒ imulh()isubh: ƒ isubh()RAD_PER_DEG: 57.29577951308232radians: ƒ radians()scale: ƒ scale()seededPRNG: ƒ seededPRNG()signbit: ƒ signbit()umulh: ƒ umulh()Symbol(Symbol.toStringTag): "Math"__proto__: Object

    Math.PI
    3.141592653589793

    Math.PI*2
    6.283185307179586

    Math.PI / 180 * 32 (radial 값 구하기)
    0.5585053606381855

    function degToRad(degree){
        return Math.PI / 180 * degree;
    }
    degToRad(180)
    3.141592653589793


    [메서드]
    Math.min()     -최소
    Math.max()     -최대
    Math.random()  -난수
    Math.floor()   -내림
    Math.round()   -반올림
    Math.ceil()    -올림
    Math.abs()     -절대값
    Math.pow()     -제곱
    Math.sqrt()    -제곱근
    Math.trunc()   -정수반환(소수점제거)

    Math.min(1,10)
    1
    Math.max(1,10)
    10
    Math.random()*10
    5.66405328651766
    Math.floor(Math.random()*10)
    8

    var foods_of_china = [
        '짜장면',
        '탕수육',
        '짬뽕',
        '라조기',
        '깐풍기'
        
    ];
    foods_of_china
    (5) ["짜장면", "탕수육", "짬뽕", "라조기", "깐풍기"]
    Math.floor(Math.random() * foods_of_china.length)
    3
    foods_of_china[Math.floor(Math.random() * foods_of_china.length)]
    "탕수육"

    function randomNumber(max, min){
        min = min || 0;
        return Math.floor(Math.random() * (max-min)) + min;
    }
    randomNumber(3)
    1
    randomNumber(3,6)
    5
  ```

#### 문자 객체
  ```javascript
    var city = 'lonDon';
    city
    "lonDon"
    city[0]
    "l"
    city.slice(1);
    "onDon"
    city[0].toUpperCase();
    "L"
    city.slice(1).toLowerCase();
    "ondon"
    city = city[0].toUpperCase() + city.slice(1).toLowerCase();
    "London"

    var cities = ['lonDon', 'ManCHESTer', 'BiRmiNGHAM', 'liVERpooOL'];
    for(var i=0, l=cities.length; i<l; i++){
        var city = cities[i];
        city = city[0].toUpperCase() + city.slice(1).toLowerCase();
        cities[i] = city;
    }
    "Liverpoool"
    cities
    (4) ["London", "Manchester", "Birmingham", "Liverpoool"]
  ```
</details>

<details open>
<summary>5일차 학습</summary>
<div markdown="1">

#### [ES6] 문자 ⎼ Template Literal
- 템플릿 리터럴은 내장된 표현식을 허용하는 문자열 리터럴 입니다. 여러 줄로 이뤄진 문자열과 문자 보간 기능을 사용할 수 있습니다.
```javascript
(function(global){
  'use strict';

  // ——————————————————————————————————————
  // 데이터
  // ——————————————————————————————————————
  const delivery_table_info = {
    table_class : 'table is-striped is-hoverable is-fullwidth delivery',
    caption     : '배송 슬롯(투입구) 시간별 Open/Closed 상황',
    days        : ['월', '화', '수', '목', '금'],
    contents    : [
      { time: '09:00 - 10:00', content: ['Closed', 'Open', 'Open', 'Closed', 'Closed'] },
      { time: '10:00 - 11:00', content: ['Open', 'Open', 'Open', 'Open', 'Closed']     },
      { time: '11:00 - 12:00', content: ['Closed', 'Open', 'Open', 'Open', 'Open']     },
      { time: '13:00 - 14:00', content: ['Open', 'Open', 'Open', 'Open', 'Open']       },
    ]
  };

  // ——————————————————————————————————————
  // DOM 스크립팅
  // ——————————————————————————————————————
  const demo = document.querySelector('.demo');

  
  /// ES6 Template Literals(Strings)
  /// 백틱(`), 인터폴레이션(${})을 사용하여 HTML 템플릿 코드를 수정해봅니다.
  
  // 렌더 테이블 함수
  // HTML 템플릿 + 데이터 바인딩
  function renderTable(container, data) {
    let table_template = '\
      <table class="' + data.table_class + '">\
        <caption class="a11y-hidden">' + data.caption + '</caption>\
        <thead>\
          <tr>\
            <td>&nbsp;</td>\
    ';

    const days = data.days;
    for ( let i=0, l=days.length; i<l; i++ ) {
      table_template += '\
            <th scope="col">'+ days[i] + '</th>\
      ';
    }

    table_template += '\
          </tr>\
        </thead>\
        <tbody>\
    ';

    const contents = data.contents;
    for ( let i=0, l=contents.length; i<l; i++ ) {
      const context = contents[i];
      table_template += '\
          <tr>\
            <th scope="row">' + context.time + '</th>\
      ';
      const content = context.content;
      for ( let i=0, l=content.length; i<l; i++ ) {
        table_template += '\
            <td>' + content[i] + '</td>\
        ';
      }
      table_template += '\
          </tr>\
      ';
    }

    table_template += '\
        </tbody>\
      </table>\
    ';

    container.innerHTML = table_template;
  }

  // 함수 실행
  renderTable(demo, delivery_table_info);

})(window);
```


#### [ES6] 문자 ⎼ String Addtions
- 문자 객체에 새롭게 추가된 인스턴스 메서드(Instance Methods)
  - .includes() : 텍스트가 포함되어있는지 여부를 불리언 값으로 반환합니다.
    ```javascript
    //ES5
    var players = 'messi ronaldo honaldo son'.split(' ');

    function filterWordList(words, filter) {
        var word_list = [];
        for (let i=0, l=words.length; i<l; ++i){
            var word = words[i];
            //문자 포함 여부 검증
            if(word.indexOf('naldo')> -1) {
                word_list.push(word);
            }
        }
        return word_list;
    }

    var naldos = filterWordList(players, 'naldo');
    console.log(naldos);

    VM50745:17 (2) ["ronaldo", "honaldo"]


    //ES6
    var players = 'messi ronaldo honaldo son'.split(' ');

    function filterWordList(words, filter){
        let word_list = [];
        for(let i=0, l=words.length; i<l; ++i){
            let word = words[i];
            if(word.includes('naldo')){
                word_list.push(word);
            }
        }
        return word_list;
    }

    let naldos = filterWordList(players, 'naldo');

    console.log(naldos);
    VM61209:16 (2) ["ronaldo", "honaldo"]


    //ES5
    var title = '경제 지표 예측';
    var substring = '지표';
    console.log(title.indexOf(substring) > -1); // true

    //ES6
    onst title = '경제 지표 예측';
    const substring = '지표';
    console.log(title.includes(substring)); // true
    ```
  - .starsWith() : 어떤 문자열이 특정 문자로 시작하는지 확인하여 불리언 값으로 반환합니다.
    ```javascript
      // ES5
      var kings_4 = '청룡 백호 현무 주작';
      kings_4.substr(0,2) === '백호'; //false
      kings_4.substr(6,2) === '현무'; //true

      function startsWith(word, find, start) {
          start = start || 0;
          return word.substr(start, find.length) === find;
      }
      startsWith(kings_4, '주작', 9); //true

      //ES6
      let kings_4 = '청룡 백호 현무 주작';
      kings_4.startsWith('백호');
      kings_4.startsWith('현무', 6);
    ```

  - .endsWIth() : 어떤 문자열이 특정 문자로 끝나는지 확인하여 불리언 값으로 반환합니다.
    ```javascript
    //ES5
    var season = '봄 여름 가을 겨울';
    var index = season.length - 2;
    season.substr(index, 2) === '겨울';

    function endsWith(word, find, end) {
        end = (end || word.length ) - find.length;
        var last_index = word.indexOf(find, end);
        return last_index !== -1 && last_index === end;
    }
    endsWith(season, '겨울');
    endsWith(season, '가을', 7);

    //ES6
    let season = '봄 여름 가을 겨울';

    season.endsWith('겨울');
    season.endsWIth('가을', 7);
    ```
  - .repeat() : 어떤 문자열을 특정한 개수만큼 반복하여 새로운 문자열을 반환합니다.
    ```javascript
     //ES5
      var repeat_word = '양심과 욕심';

      function repeat(word, count){
          count = count || 0;
          if(count === 0) {return ''; }
          var combine = word;
          while(--count){
              combine += word;
          }
          return combine;
      }

      repeat(repeat_word);
      repeat(repeat_word, 4);

      //ES6
      let repeat_word = '양심과 욕심';

      repeat_word.repeat();
      repeat_word.repeat(4);


      //ES5
      function repeat(string, count) {
        var strings = [];
        while(strings.length < count) {
          strings.push(string);
        }
        return strings.join('');
      }

      repeat('오라! ', 3); // '오라! 오라! 오라!'

      //ES6
      '오라! '.repeat(3); // '오라! 오라! 오라!'
    ```