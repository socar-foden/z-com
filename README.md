### # 섹션 1

- 모달(+ 바텀시트?)
  - url로 설계
    - 모바일 디바이스 `뒤로가기 물리버튼 대응`하기 좋을 듯
      - 핸들러의 경우, `url, history 객체로 관리하려다 실패`해서 `global state`로 구현
    - Q. 모달 형태의 컴포넌트의 경우, 단순히 뒤로 가는(모달을 닫는) 것 뿐만 아니라, `전, 후처리가 있을 수 있는데`, 이때 처리가 난해할수도 있을 듯?
      - ex. 닫을 것인지(뒤로 갈 것인지) 물어보는 것 등
- `layout vs template`
  - 차이: 리렌더링
- 라우트 그룹
  - `layout` 관리 용이
- css 새로운 단위
  - `dvw, dwh`
  - 그 외에도 `s~, l~`
    - https://dev.to/frehner/css-vh-dvh-lvh-svh-and-vw-units-27k4
    - 요약
      - 기존 vh(vw)의 문제점
        - 모바일 (웹)에서 문제
          - ex. 스크롤 방향에 따른 기준 차이(주소창 때문에)
          - ex. overflow 문제 등
        - 2019년 새로운 방식의 단위가 제안되었고, 2021년 승인
      - `s~`
        - 뷰포트가 가장 작을 때가 기준
      - `l~`
        - 뷰포트가 가장 클 때가 기준
      - `d~`
        - 변하는 뷰포트에 맞춰 동적으로 계산해줌
        - 이론적으로는 좋게 들리지만, 성능, 안정성의 문제로 매우 제한적인 경우에만 사용 권장
- 페러렐 라우트
  - 두 페이지를 동시에 보여주고 싶을 때 (모달 같은 경우)
  - 원리는 단순
    - 동일 디렉토리의 layout에서 그냥 children과 함께 그려주기만 하면 됨
    - `@`로 시작하게끔 문법만 따르면 됨
      - `@`(슬롯)는 url에 영향을 끼치지 않음
  - 궁금한 점
    - `라우트 구조가 알아보기 힘들수도` 있겠다
    - `page 컴포넌트들의 역할이 모호`해져서 나중에 헷갈리지 않을까
  - 뎁스 문제 때문에 페러렐 라우트가 제대로 동작하지 않을 수 있다.
    - 이를 위해 해당 라우트 최상위에서 `default.tsx`, `page.tsx`를 만들어 해결해줄 수 있음
- 서버 컴포넌트 vs 클라이언트 컴포넌트
  - 기본적으로 모든 컴포넌트는 서버 컴포넌트
    - (페이지 외에도 `모든 컴포넌트`를 말함)
    - `기타`
      - `이벤트 핸들러`가 존재 or `hook을 사용`하면 보통 `클라이언트 컴포넌트`여야 한다고 생각하면 편함
      - app router를 공부하기 싫었던 주요 이유였을 듯
    - 서버 렌더링의 이점
      - https://nextjs.org/docs/app/building-your-application/rendering/server-components#benefits-of-server-rendering
        - 데이터 가져오기: 클라이언트 부담을 줄임
        - 보안
        - 캐싱
        - 클라이언트측 번들 크기가 줄어듦
          - 번들 크기
          - First Contentful Paint (FCP)
          - 검색 엔진 최적화 및 소셜 네트워크 공유 가능성
          - 스트리밍
  - 기본적으로 클라이언트 측 API(훅 등)를 사용할 수 없음
    - `"use client"` 사용
- 인터셉팅 라우트
  - 라우트를 가로채서 다른 페이지를 보여줌
  - `클라이언트측에서만 동작`
    - 새로고침시 x
  - 단점
    - 컨벤션이 복잡함
    - 거의 통째로 중복이 발생할 수 있음
      - ssr 시도시 보여줄 페이지가 필요함
      - 중복되는 양이 적지 않을 듯
    - 기능 자체가 패러렐 라우트에 종속되어있는 것 같다.
- private 폴더
  - 위에서 발생되는 것과 같은 `코드 중복을 해결하기 위함`
- Next `Router 동작 시에도 SSR, CSR을 고려해야함`
  - `기본적으로 서버 컴포넌트`이므로 `그냥 redirect 수행시, 서버측에서 수행되는 redirect`임

<hr />

### # 섹션 2

- `flex-grow`
  - `flex-item 요소`가 `flex-container 요소 내부에서 할당 가능한 공간의 정도`를 선언
  - `부모 컨테이너를 flex로 두고, 자식 요소들의 너비들을 width 속성에 %로 설정`해주는데, `flex-shrink, flex-grow, flex-basis`를 활용?
- `inherit`
  - fixed 요소에 너비 100%를 주고 싶을때
- a tag active 상태 -> 스타일링
  - url로 분리되는 탭이라면, 컴포넌트 내부 상태를 활용하는 것보다 active 상태를 활용하는 것이 맞을 듯
- `useSelectedLayoutSegment`, `useSelectedLayoutSegments`
  - 바로 하위의 `디렉토리명(들) 값`을 가져올 수 있음
- 미디어 쿼리 `prefers-color-scheme`
  - 다크모드 스타일링 가능
    ```css
    @media (prefers-color-scheme: dark) {
      .theme-a.adaptive {
        background: #753;
        color: #dcb;
        outline: 5px dashed #000;
      }
    }
    ```
- `server action`
  - `서버에서 실행`되는 비동기 함수
    - 서버, 클라이언트 측에서 실행되는 `form에 대한 전송, 데이터 변형`을 처리할 수 있다.
  - 'use client', 'use server' 모두 nextjs app router에서 나온 개념이라기 보다는 `react에서 나온 개념`
  - 컨벤션
    - 클라이언트 컴포넌트
      - `모듈 수준`의 지시만 가능
    - 서버 컴포넌트
      - `모듈 수준`, `인라인 수준`의 지시 모두 가능
  - 결과적으로는 단순 함수이기 때문에, 꼭 `form에만 국한되는 기능은 아님`
    - 결국 목적은, `서버에서 먼저 실행되는 함수를 만들어`, 해당 로직이 `서버에서 실행됨으로써 얻는 이득을 취하기 위함`. (`pre-loading, 캐싱, 클라이언트 리소스 절약` 등)
      - `server component의 이점`과 비슷
    - 패턴과 모범사례
      - https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#fetching-data-on-the-server
    - \*\* 아직 `실험적인 기능`
- react `useOptimistic` 훅
  - `optimistic UI (낙관적인 UI)`
    - `보편적으로 서버와의 통신이 정상적일 확률은 99% 이상,` `따라서 간결해야하거나 중요성이 떨어지는 UI는 낙관적으로` 구현하는 것이 낫다.
      - (ex. `좋아요`, `토글 라디오 버튼` 등)
      - (ex. `빠르게 여러 번` 누르는 등)
    - https://www.smashingmagazine.com/2016/11/true-lies-of-optimistic-user-interfaces/
- `inlay hints`
  - 각 언어, 툴에 대한 `보조 힌트`를 제공하는 기능
    ![inlay-hints](/images/inlay-hints.png)
  - vscode에서 기본적으로 제공
- `dayjs`
  - `ios 사파리` 날짜 포맷 관련 이슈
    - 사파리에서는 정확히 `ISO 8601` format만 지원
      - `생각보다 평범한 포맷`도 Date 객체가 invalid date로 판단하는 경우가 많다
        - ex. `YYYY.MM.DD` -> `'invalid date'`
    - `내부적으로 Date 객체를 사용하는 dayjs도 마찬가지 해당 이슈 발생`. 다른 라이브러리들은 모르겠음
  - https://stackoverflow.com/questions/4310953/invalid-date-in-safari
- search 페이지 -> SEO 관련
  - `path` vs `query params`
    - `/search/냉장고` vs `/search?search=냉장고`
    - path의 형태가 유리
      - query 형태의 페이지들을 모두 별도의 새로운 페이지로 인식 -> `중복된 검색결과가 불필요하게 노출`
      - 위 내용이 `크롤러에게도 동일하게 불리하게 작용`
        - `크롤링 자체가 비효일적`
        - `메인 페이지 indexing`에 혼동
      - `query params`가 많이 붙을수록 해당 url은 신뢰할 수 없는 형태로 바뀌게 된다.
        - 하나의 의미를 지닌 path로 요약
          ![path_vs_query_params](/images/path_vs_query_params.png)
        - `CTR(Click-through Rate: 클릭률)` 저하
        - 특히나 카카오, 페이스북 등 `소셜에서 공유될 때` 신뢰도에 큰 영향을 끼침
    - query parmas를 사용하더라도 아래 내용을 지키면 좋음
      - `불필요한 query 제거`
      - `빈 값을 가진 query 제거`
      - `key를 한번만` 사용
        - X: `?key1=a&key2=b`
        - O: `?key=a-b`
      - 하나의 앱에서는 페이지가 달라도 `항상 일관된 순서의 key 정렬`
    - https://www.searchenginejournal.com/technical-seo/url-parameter-handling/
  - `a tag` vs `onClick`
    - 구글은 인식을 잘 하는 것으로 확인되지만, 기타 다른 검색엔진은 불확실함
    - onClick -> a tag만 변경하는 작업을 했는데, 색인률이 많이 높아지긴 했었다고 함
    - 참고
      - https://webmasters.stackexchange.com/questions/113525/seo-impact-of-onclick-handlers-in-place-of-links
- `onClickCapture` 핸들러 존재
