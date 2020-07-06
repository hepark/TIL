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