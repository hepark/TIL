# TIL
오늘 내가 배운 것들(Today I Learned)   

---

#### 5주차 질문
- Q. crossorigin.me 버전으로 작업시 오류가 나는데 어떤 부분이 잘못 됐는지 모르겠어요.
  ```javascript
    var api_address = 'https://yamoo9.herokuapp.com/rest/ediya-menu';

    function corsURL(url) {
      return 'https://cors-anywhere.herokuapp.com/ ' + url;
    }

    ajax.get(corsURL(api_address)).then(function(data){
      console.log(data);
    });
    ```
  - 에러사항:<br> 
  // Access to XMLHttpRequest at 'https://crossorigin.me/https://yamoo9.herokuapp.com/rest/ediya-menu' from origin 'http://127.0.0.1:5500' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
  // GET https://crossorigin.me/https://yamoo9.herokuapp.com/rest/ediya-menu net::ERR_FAILED  // ajax.js:85 

- Q. Ajax 사용시 페이지 변경을 감지하지 못해서 접근성에 위배된다고 들었는데, Ajax 사용시 따로 처리해줘야 하는 접근성 규칙이 있을까요?

- Q. 야무님 촬영해놓으신 SVG 영상강의를 이번 차수 끝나고 들어보려고 하는데, 주소를 알려주실 수 있을까요?

- Q. 코로나 떄문에 아직 모르겠지만, 다음 React 수업은 8월 22일 시작하는 거겠죠?
  
<details open>
<summary>1일차 학습</summary>
<div markdown="1">

#### [XMLHttpRequest 객체]
- 에이젝스(Ajax)란? 
  - Asynchronous Javascript And XML(json/Text/html)
  - 필요한 부분만 별도로 요청하고 응답 받아 처리
  - 페이지를 새로고침하지 않고도 필요한 데이터만 받아와서 내용을 업데이트

- XML Http Request : Ajax 통신을 위해 생성된 객체 (AJAX need XHR)
  ```javascript
  var xhr = new XMLHttpRequest;
  ```

  ```javascript
    (function(global){
      'use strict';
      //console.log('action')
      var xhr = new XMLHttpRequest();
      xhr.open('GET', '../data/ajax_desc.txt', false);
      //서버요청
      // xhr.send();
      var call_btn = document.querySelector('.call-ajax-data-button');
      var print_area = document.querySelector('.print-area');
      var callAjaxData = function(){
        xhr.send();
        //통신 요청에 따른 응답이 오면 처리
        if(xhr.status === 200 || xhr.status === 304){
          print_area.textContent = xhr.responseText;
        } else {
          console.warn("통신 실패");
        }
      }
      call_btn.addEventListener('click', callAjaxData);
    })(window);
  ```
</details>
<details open>
<summary>2일차 학습</summary>
<div markdown="1">

#### [ajax 심플 라이브러리]

</details>
<details open>
<summary>3일차 학습</summary>
<div markdown="1">

#### [크로스 도메인 보안 이슈와 해결책]
### 동일출처정책(SOP) 

- SOP는 "동일출처정책"으로 한 출처(Origin)에서 로드된 문서나 스크립트가 다른 출처의 자원과 상호작용하지 못하도록 제한하는 것.
- 이 정책에 의해 XMLHttpRequest객체로 특정 웹 페이지에 접근할 때,해당 웹페이지와 동일한 출처(SameOrigin)의 페이지에만 접근이 가능.
- 동일한 출처: 프로토콜(protocol),호스트(host),포트(port)가 동일.
- 웹페이지 스크립트는 해당 페이지와 동일한 서버에 있는 데이터만 Ajax비동기 요청하여 처리 가능.
  
### CORS:서버(Server)개발단 해결책
- Cross-OriginResourceSharing
- 서버에서 외부 요청을 허용할 경우 Ajax요청이 가능
- 요청을 받은 웹 서버가 허용할 경우에는 다른 도메인의 웹페이지 스크립트에서 데이터를 주고받을 수 있게 허용

### 프론트엔드(Client)개발단 해결책
- Chrome 브라우저 Allow-Control-Allow-origin:*플러그인 설치
- JSONP 방식으로 요청
  - GET방식의 API만 요청이 가능
  ```javascript
  function jsonp(url, callback, time_limit, log){
    var unique_id = Math.floor(Math.random() * 100000);
    var jsonpCb = 'jsonpCb_' + unique_id;
    var s = document.createElement('script');
    s.src = url + '?callback=' + jsonpCb;
    document.head.insertAdjacentElement('beforeend', s);
    window[jsonpCb] = callback;
    s.remove ? s.remove() : s.parentNode.removeChild(s);
    window.setTimeout(function(){
      log && console.info('JSONP 콜백함수: ' + jsonpCb + '를 전역에서 제거했습니다.');
      delete window[jsonpCb];
    }, (time_limit || 3) * 1000);
  }

  jsonp(api_address, functon(data){
    console.log(data);
  });

  ```
- crossorigin.me 사이트(권장)
  - 무료로 CORS를 이용할 수 있는 프록시 서비스를 제공
  - CORS를 지원하지 않는 웹사이트 리소스에 접근해야 하는 경우, URL의 시작 부분에 http://crossorigin.me를 추가하면 문제가 해결
    ```javascript
    function corsURL(url){
      return 'https://crossorigin.me/' + url
    }

    //SOP 문제 발생!
    ajax.get('http://api.worldbank.org/countries?per_page=10&incomeLevel=LIC');

    //SOP 문제 해결!
    ajax.get(corsURL('http://api.worldbank.org/countries?per_page=10&incomeLevel=LIC'));

    ```
  - 주의할점
    - 개발 과정에서만 사용할 것!
    - 파일 크기는 2MB
    -  GET 요청만 지원
    - 중요한 정보를 전송할 경우 사용하지 말 것.

#### [jQuery, Ajax 라이브러리]
- jQuery.ajax( url [, settings ] )
  ```javascript
    $.ajax(api).done(function(data, statusCode){
      console.log(statusCode, data);
    })
    .fail(function(xhr, status, error){
      console.error('오류발생');
    });

    $.ajax(api).then(function(data, statusCode){
      console.log(statusCode, data);
    });

    $.ajax({
        url: api,
        dataType: 'jspnp'
      }).then(function(data, statusCode){
      console.log(statusCode, data);
    });
  ```

</details>