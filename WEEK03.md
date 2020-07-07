# TIL
오늘 내가 배운 것들(Today I Learned)   

---

<details open>
<summary>1일차 학습</summary>
<div markdown="1">

  #### [객체/상속]
  - 객체 생성
    - {} 또는 new Object()
      ```javascript
        var o = {};
        o.name ="객체";
        "객체"
        o
        {name: "객체"}
        delete o.name
        true
        o
        {}
      ```
    
  - 객체 병합(Mixins) : 여러 개의 객체를 합쳐 새로운 객체를 반환하는 헬퍼 함수
    ```javascript
      var mixin = function() {
        var mixin_obj = {};
        for ( var i=0, l=arguments.length; i<l; ++i) {
          var o = arguments[i];
          for ( var key in o ) {
            var value = o[key];
            if (o.hasOwnProperty(key)) { mixin_obj[key] = value; }
          }
        }
        return mixin_obj;
      };
    ```
    ```javascript
      var car = { 
        type: 'normal', 
        wheels: 4, 
        handle: 1,
        mirrors: { side: 2, back: 1 },
        engine: '3000cc',
        weight: '313kg',
        booster: false
      };

      var extend_car_features = { 
        type: 'super',
        wheels: 6,
        booster: true,
        engine: '4497cc',
        weight: '452kg',
        maximum: {
          power: '2248 horse-power, 21800 rpm',
          torque: '194kgm, 17100 rpm',
          speed: '695km + alpha'
        }
      };

      var super_car = mixin(car, extend_car_features);

      // console.log(super_car);
    ```

  - Object.create() 
    - 객체 및 속성(property)을 갖는 새 객체를 만듭니다.
    ```javascript
    var o = Object.create(car);
    o
    {}__proto__: type: "normal"wheels: 4handle: 1mirrors: {side: 2, back: 1}engine: "3000cc"weight: "313kg"booster: false__proto__: Object
    o.wheels
    4
    o.wheels = 5;
    5
    car.wheels
    4
    delete o.wheels
    true
    o
    {}
    o.wheels
    4
    ```

  - Object.defineProperty(obj, property, descripter)
    - 객체에 직접 새로운 속성을 정의하거나 이미 존재하는 객체를 수정한 뒤 그 객체를 반환
    ```javascript
      Object.defineProperty(o, 'name', {value: 'mouse'});
      {name: "mouse"}
      o
      {name: "mouse"}

      delete o.name  // 삭제 불가능
      false

      o.name = "lion";  // 수정 불가능
      o.name
      "mouse"

      foo(3);
    ```
  - 데이터 기술(Data descriptors)
    - writable: false,     : 할당 연산자(=)를 통한 값 변경 가능 여부
    - enumerable: false,   : 객체의 속성으로 열거 가능 여부
    - configurable: false, : 객체의 속성 제거 가능 여부
    - value: undefined,    : 객체 속성 값 설정

    ```javascript
      Object.defineProperty(o, 'use', {
        value: 'office',
        writable: true
      });
      {name: "mouse", use: "office"}
      o.name = "phone";
      "phone"
      o
      {name: "mouse", use: "office"}
      o.use
      "office"
      o.use = "home";
      "home"
      o
      {name: "mouse", use: "home"}

      Object.defineProperty(o, 'getUse', {
          value: function(){ return this.use;},
          configurable: true,
          enumerable: true,
      });
      {name: "mouse", use: "home", getUse: ƒ}
      o
      {name: "mouse", use: "home", getUse: ƒ}
      o.getUse();
      "home"
      o.use = "school";
      "school"
      o.getUse();
      "school"
      for (var p in o) (console.log[p]);
      undefined
      for (var p in o) (console.log(p));
      VM11304:1 getUse
    ```
---------------------------------------

- 데이터 접근 기술(accessor descriptors)
  - 속성의 값을 얻는 목적으로 사용되는 getter용 함수로 함수의 반환 값이 객체 속성 값이 됩니다.
    - value, get 을 동시에 사용하면 오류가 발생됩니다.
    - get: undefined
  - 속성의 값을 설정하기 위한 setter용 함수로 오직 하나의 인자를 받으며, 속성의 값으로 할당합니다.
    - set: undefined,

- 객체 확장 차단 : 새로운 속성을 추가하지 못함. (지우는 것은 가능)
  - Object.preventExtensions()
  - Object.isExtensible()
- 객체 밀봉(시얼링, Sealing) : 객체를 밀봉하면 새로운 속성을 추가할 수 없고, 모든 속성을 설정 불가능 상태로 만들어줍니다. 하지만 쓰기 가능한 속성의 값은 밀봉 후에도 변경할 수 있습니다.
  - Object.seal()
  - Object.isSealed()
- 객체 동결(프리징, Freezing) : 객체의 속성을 지우거나, 바꿀 수 없습니다. 밀봉 + 속성 값 변경 차단
  - Object.freeze()
  - Object.isFrozen()

</div>
</details>

<details open>
<summary>2일차 학습</summary>
<div markdown="1">

  #### [생성자/프로토타입]
  - 생성자함수
    - new 연산자 뒤에 생성자 함수를 실행하면, 내장 객체 또는 사용자 정의 객체 인스턴스를 생성
    - 첫글자를 대문자로 작성, 일반 함수와 구분하지만 기능적인 차이는 없음
    - 생성된 이후, 객체의 속성을 추가하게 되면 각 객체는 자신만의 속성을 가진다.
    - return을 명시하지 않아도 this를 반환한다.
    - 하지만 제작자는 return 을 명시함으로 이러한 설정을 덮어쓸 수 있다.
    ```javascript
      var main_tab = new Tab('.main__tab');
    ```

  - 프로토타입(Prototype)
    - JavaScript의 모든 함수 객체는 정의와 동시에 자동으로 함수의 프로토타입(Prototype) 객체를 참조하는 속성 .prototype을 가지게 됨
    -  Tab.prototype 객체에 추가된 속성은 공통 속성이 된다.
    ```javascript
    console.log('Tab 함수에 연결된 프로토타입 객체 참조 속성:', Tab.prototype);
    ```
  ### [객체지향 프로그래밍]
  - OOP: Object Oriented Programming
    ```javascript
      (function(){
        'use strict';
        /**
         * @constructor: Animal
         * 생성자 함수: `동물`
         */
        function Animal() {
          'use strcit';
        }

        // @prototype: Animal
        // 프로토타입 객체: `동물`
        Animal.prototype.type  = '동물';
        Animal.prototype.brain = true;
        Animal.prototype.legs  = 0;
        Animal.prototype.run   = function(){};

        /**
         * @constructor: Pet
         * 생성자 함수: `애완동물`
         */
        function Pet() {
          'use strcit';
        }

        // @prototype: Pet
        // 프로토타입 객체: `애완동물`
        Pet.prototype.type  = '애완동물';
        Pet.prototype.brain = true;
        Pet.prototype.legs  = 4;
        Pet.prototype.fleas = 0;
        Pet.prototype.run   = function(){};

        /**
         * @constructor: Cat
         * 생성자 함수: `고양이`
         */
        function Cat(name, gender, color) {
          'use strcit';
          this.name   = name;
          this.color  = color;
          this.gender = gender;
        }

        // @prototype: Cat
        // 프로토타입 객체: `고양이`
        Cat.prototype.type   = '고양이';
        Cat.prototoype.brain = true;
        Cat.prototoype.legs  = 4;
        Cat.prototoype.fleas = 4;
        Cat.prototype.run    = function(){};
        
        }());
      ```
  ### [객체지향 프로그래밍 용어 풀이]
  - Class -객체 속성(Properties)을 정의합니다. ( 예: 설계 도면 )
  - Object - Class의 인스턴스(Instance) 입니다. ( 예: 설계 도면을 통해 구현된 실제 제품 )
  - Property - 객체의 속성을 말합니다. ( 예: color 등과 같은 명사 형태 )
  - Method - 객체의 기능을 말합니다.( 예: walk() 등과 같은 동사 형태 )
  - Constructor - 인스턴스 생성 순간에 호출 실행되는 메서드입니다.
  - Inheritance - Class는 다른 Class로 부터 속성들을 상속받을 수 있습니다. (Super Class ⇒ Sub Class)
  - Encapsulation - Class는 해당 객체의 속성, 메서드 만 정의할 수 있습니다. (외부 접근 불가)
  - Abstraction - 복잡한 상속, 메서드, 객체 속성의 결합은 반드시 현실 모델을 시뮬레이션할 수 있어야 합니다.
  - Polymorphism - 다른 Class 들이 같은 메서드나 속성으로 정의될 수 있습니다.
</details>