# TIL
오늘 내가 배운 것들(Today I Learned)   


## 자바스크립트
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
