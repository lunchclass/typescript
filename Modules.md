# Modules
전역 스코프 분리
캡슐화
재 사용성 높이기
의존성 관리

### 변수, 인터페이스, 클래스 등을 묶어서 캡슐화
- C#/Java의 네임 스페이스/패키지와 같은 개념
- 서브시스템으로 코드를 조직화
- module 키워드로 생성

### Internal 모듈과 External 모듈로 구분됨
- export 키워드로 모듈 내에 선언된 멤버를 모듈 밖에서도 public하게 사용 가능하도록 설정

### 하나의 모듈을 서로 다른 파일로 분리 가능
### 컴파일 후 하나로 통합 가능
### 외부 라이브러리로 사용 가능, 하나의 스코프로 묶음 가능

Link : http://www.typescriptlang.org/docs/handbook/modules.html

# Common JS 와 AMD
http://d2.naver.com/helloworld/12864
### CommonJS
서버 사이드 Javascript의 주요 쟁점
1. 서로 호환되는 표준 라이브러리가 없다.
2. 데이터베이스에 연결되는 표준 인터페이스가 없다.
3. 다른 모듈을 삽입하는 표준적인 방법이 없다.
4. 코드를 패키징해서 배포하고 설치하는 방법이 필요하다.
5. 의존성 문제까지 해결하는 공통 패키지 모듈 저장소가 필요하다.

핵심은 모듈화 
스코프, 정의, 사용

비동기 모듈 로드 문제

### AMD
두 줄기의 표준화 움직임
CommonJS와 AMD의 비교
AMD의 모듈 명세
define() 함수
전역 변수와 define.amd 프로퍼티
AMD로 정의한 모듈 예시

AMD의 장점 
Require JS
모듈화와 HTML5
