### # 섹션 1

- 모달(+ 바텀시트?)
  - url로 설계
    - `웹뷰 환경`일 경우, `뒤로가기 물리버튼 대응`하기 좋을 듯
      - 핸들러의 경우, `url, history 객체로 관리하려다 실패`해서 `global state`로 구현
- `layout vs template`
  - 차이 리렌더링
- 라우트 그룹
  - `layout` 관리 용이
- css 새로운 단위
  - `dvw, dwh`
  - 그 외에도 `s~, l~`
    - https://dev.to/frehner/css-vh-dvh-lvh-svh-and-vw-units-27k4
    - 요약
      - 기존 vh(vw)의 문제점
        - 모바일 (웹)에서 문제
          - 스크롤 방향에 따른 기준 차이(주소창 때문에)
          - overflow 문제 등
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
  - 궁금한 점
    - `라우트 구조가 알아보기 힘들수도` 있겠다
    - `page 컴포넌트들의 역할이 모호`해져서 나중에 헷갈리지 않을까
  - 뎁스 문제 때문에 페러렐 라우트가 제대로 동작하지 않을 수 있다.
    - 이를 위해 해당 라우트 최상위에서 `default.tsx`, `page.tsx`를 만들어 해결해줄 수 있음
- 서버 컴포넌트 vs 클라이언트 컴포넌트
  - 기본적으로 모든 컴포넌트는 서버 컴포넌트
    - (페이지 외에도 모든 컴포넌트를 말함)
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
  - 클라이언트측에서만 동작
    - 새로고침시 x
  - 단점
    - 컨벤션이 복잡함
    - 거의 통째로 중복이 발생
      - ssr 시도시 보여줄 페이지가 필요함
      - 중복되는 양이 적지 않을 듯
    - 기능 자체가 패러렐 라우트에 종속되어있는 것 같다.
- private 폴더
  - 위에서 발생되는 것과 같은 코드 중복을 해결하기 위함
