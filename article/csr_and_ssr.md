## 클라이언트사이드 vs. 서버사이드 렌더링
* CSR과 기존 SSR 방식의 차이점을 비교해보려 한다. 

### CSR (Client-side Rendering)
* 처음 서버로부터 모든 페이지를 가져온 후, 사용자의 액션에 따라 라우팅만 해주거나, 서버로부터 json 등의 갱신되어야 하는 데이터만 가져와 화면을 바꿔준다.
* 렌더링을 위한 서버 자원이 절약되며, 서버측에 요청하여 페이지 전체가 다시 로드되는 것보다 빠르다는 장점이 있다.
* SEO에 취약하다는 단점이 있었으나, 이 또한 최근 구글의 검색엔진에서는 개선되었다. 

### SSR (Server-side Rendering)
* 전통적인 렌더링 방식. 사용자의 액션에 의해 서버가 페이지를 갱신한다. 
* 아무래도 서버에 요청 후 전체 페이지를 렌더링하기에 새로고침이 발생하고, 모든 리소스를 불러오기에 효율성이 나쁘다.
* SEO에 좋다. 
