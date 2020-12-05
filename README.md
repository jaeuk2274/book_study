# book_study
- 인상깊고 기억하고 싶은 내용만을 정리

## Clean Code

### 02.의미 있는 이름
- 의도를 분명히 밝혀라
  - theList -> gameBoard -> 배열대신 간단한 클래스
  - 4는 무슨 의미 -> FLAGGED -> isFlagged() 더 명시적인 함수
 
- 그릇된 정보를 피하라
  - 서로 흡사한 이름 주의
  - 유사한 표기 주의
 
- 의미 있게 구분하라

- 검색하기 쉬운 이름을 사용하라
  - MAX_CLASS_PER_STUDENT 는 찾을수 있지만 숫자7은 찾지 못한다.
  
- 일관성 있는 어휘, 한 개념에 한 단어
- 그러나 한 단어를 두 가지 목적으로 사용하지 마라
  - 한 개념 한 단어 add 사용 -> 같은 맥락이 아닌데도 add 사용X
  - ex. add는 기존 값에 두 개를 더하거나 이어서 새로운 값 생성 vs 집합에 값 하나를 추가 : 다른 맥락, insert, append 등
  
- 의미 있는 맥락을 추가하라
  - 주소 관련 변수들, 읽어보면 알지만 -> addr 접두어 추가해서 더 명확히 -> 아예 주소 클래스 생성 (컴파일러조차 분명해짐)
  - 맥락 같은 변수들, if count 0,1,else -> 클래스로 생성,생성자,make(),noLetter(),OneLetter(),manyLetter()


### 03.함수
- 작게, 더 작게 만들어라
  - if/else/while 에 들어가는 블록은 한 줄. 거기서 함수 호출. 바깥을 함수가 작아질 뿐더러 블록 안의 호출 함수명을 적절히 지으면 코드를 이해하기 월등히 쉬움.
  - 들여쓰기 수준은 1,2단 넘어서면 안됨. 함수는 읽고 이해하기 쉬워진다.
  
- 한 가지만 해라, 한 가지를 잘 해야 한다, 그 한 가지만을 해야 한다

- 함수 당 추상화 수준은 하나로!

- 위에서 아래로 코드 읽기:내려가기 규칙
  - 위에서 아래로 읽으면 함수 추상화 수준이 한번에 한 단계씩 낮아진다.
핵심은 짧으면서도 '한 가지'만 하는 함수

- Switch 문 (p47)
  - 잘못된 방법 : 일반적으로 사용되던 분기 case 문당 함수 
    - 새 직원유형 추가시 함수 길어지며, 수정해야한다. 
    - 한 가지 작업만 수행하지 않는다. 
    - 구조가 동일한 함수가 여러 개 계속 생성된다. 
  - switch 문을 추상 팩토리에 숨기고, case 별 파생 클래스의 인스턴스 생성 -> 각 함수는 Employee 인터페이스를 거치며 다형성으로 인해 실제 파생 클래스의 함수가 실행된다.
  
- 함수의 인수
  - 이상적인 인수는 0개, 가능한 3개 이상은 피하자, 4개 이상은 절대 사용하면 안된다.
  - 테스트 관점에서도 인수가 3개를 넘어가면 인수마다 유효한 값으로 모든 조합을 구성해 테스트하기 부담

- 플래그 인수는 추하다
  - 함수로 부울값을 넘기는 것 -> 함수가 대놓고 여러 가지를 처리한다고 공표하는 것. 참이면 이걸 하고, 거짓이면 저걸 한다는 거니까.
  - render(true) 헷갈리기 십상 -> renderForSuite(), renderForSingleTest()
  
- 인수 객체
  - 인수가 2,3개 필요하다면 일부를 독자적인 클래스 변수로 선언할 가능성 검토
  - 변수를 묶어 넘기려면 이름을 붙여야 하므로 개념을 표현하게 됨.
  
- 부수 효과를 일으키지 마라
  - 함수에서 한 가지를 하겠다 해놓고 남몰래 다른 것을 한다는 것
  - 함수 이름만 보고 호출했다, 부수적인 효과 때문에 낭패
  - 정말 필요하다면 함수 이름에 분명히 명시할 것
  
- 명령과 조회를 분리하라

- 오류 코드보다 예외를 사용하라

- Try/Catch 도 추하다. 정상 동작과 오류 처리 동작을 뒤섞는다.
  - 별도 함수로 뽑아내는 편이 좋다. / 정상 동작과 오류 처리 동작의 분리
  
- 오류 처리도 '한 가지' 작업이다  

- Error.java 의존성 자석
  - 오류 코드를 반환한다는 건 오류 코드를 정의한다는 것인데.. enum Error{...}
  - 다른 클래스에서 import 해 사용하므로 Error 이 변한다면 전부 다시 컴파일,배치
  - 그래서 새 오류코드 대신 기존 오류 코드를 재사용하게 된다 
  - 결론 : 오류 코드 대신 예외를 사용하면 새 예외는 Exception 클래스에서 파생된다.
  
- 반복하지 마라

### 04.주석

- 주석은 나쁜 코드를 보완하지 못한다.
  - 나쁜 코드에 주석을 달지 마라, 새로 짜라.
  
- 코드로 의도를 표현하라.
```java
//직원에게 복지 혜택을 받을 자격이 있는지 검사한다
if(emp.flag && emp.age > 60 && HOURLY_FLAG) 
->
if(emp.isEligibleForFullBenefits())
```

- 좋은 주석
  - 정말로 좋은 주석은, 주석을 달지 않을 방법을 찾아낸 주석이다.
  - 법적인 주석
  - 정보를 제공하는 주석
  - 의도를 설명하는 주석
  - 의미를 명료하게 밝히는 주석
  - 결과를 경고하는 주석 -> ex.여유 시간이 충분하지 않다면 실행하지 마십시오./스레드에 안전하지 못하다/각 인스턴스를 독립적으로 생성해야 한다.
  - 앞으로 할 일 TODO 주석
  - 공개 API에서 Javadocs
  - 중요성을 강조하는 주석
  
- 나쁜 주석
  - 오해의 여지가 있는 주석
  - 의무적으로 다는, 있으나 마나, 너무 당연한 주석
  - 이력을 기록하는 주석 -> 이제는 형상관리
  - 함수나 변수로 표현할 수 있으면 주석을 달지 마라
  - 주석처리한 코드
  - 비공개 코드에서 Javadocs
  - 오해의 여지가 있는 주석길
  - 오해의 여지가 있는 주석
  
### 05.형식 맞추기  
  
- 개념은 빈 행으로 분리해라

- 세로 밀집도
- 수직 거리
  - 함수 연관, 동작 방식을 이해하려고 이 함수, 저함수 오가며 위 아래로 뺑뺑이 돈다
  - 서로 밀집한 개념은 세로로 가까이 둬야 한다.
  - 연관성 깊은 개념이 멀리 떨어져 있으면 이리 저리 뒤지게 된다.
  
- 변수는 사용하는 위치에 최대한 가까이 선언한다

- 개념적 유사성
  - 친화도가 높은 요인 : 한 함수가 다른 함수 호출하는 직접적인 종속성, 변수와 변수를 사용하는 함수.
  - 그 외에 비슷한 동작을 수행하는 일군의 함수 예시 코드
  - 명명법이 똑같고, 기본 기능이 유사하고 간단.
  
- 팀의 규칙

### 06.객체와 자료 구조

- 자료 추상화
  - 자료를 세세하게 공개하기 보다는 추상적인 개념으로 표현,제공
  - 개발자는 객체가 포함하는 자료를 표현할 가장 좋은 방법을 심각하게 고민해야 한다.
  - 아무 생각 없이 조회/설정 함수를 추가하는 방법이 가장 나쁘다.
  
- *자료/객체 비대칭 (p120)
  - 절차적 코드와 객체적 코드 / 상호보완적
  - 절차적 코드 : 기존 자료 구조를 변경하지 않으며 새 함수를 추가가 쉽다.
  - 객체적 코드 : 기존 함수를 변경하지 않으며 새 클래스를 추가하기 쉽다.
  - 절차적 코드 : 새로운 자료 구조를 추가하기 어렵다 -> 모든 함수를 고쳐야 한다.
  - 객체적 코드 : 새로운 함수를 추가하기 어렵다 -> 모든 클래스를 고쳐야 한다.
  - 분별 있는 프로그래머는 모든 것이 객체라는 생각이 미신임을 잘 안다. 때로는 단순한 자료 구조와 절차적 코드가 적합할 수 있다.
  
- 디미터 법칙

- 자료 전달 객체
  - Data Transfer Object (DTO)
  - 활성 레코드 : 활성 레코드에 비즈니스 규칙 메서드를 추가해 자료 구조를 객체로 취급하는 경우가 흔하다. 하지만 이건 잡종구조
  - 활성 레코드는 자료 구조로 취급하며, 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성한다
  - (좀 더 알아볼 것)
  
  
### 07.오류 처리
- 오류 코드보다 예외를 사용하라(p131)

- 예외에 의미를 제공하라

- 호출자를 고려해 예외 클래스를 정의하라(p137)
  - 외부 API를 감싸면 외부 라이브러리와 프로그램 사이의 의존성이 크게 줄어든다
  - 감싸기 클래스를 사용하면 특정 업체가 설계한 API 방식에 발목 잡히지 않는다.
  
- 정상 흐름을 정의하라
  - 특수 사례 패턴, 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식. -> 클라이언트 코드가 예외상황을 처리할 필요가 없어짐
  
- null을 반환하지 마라
  - 지금까지 null을 체크하는 코드를 수없이 봐왔다. 일거리를 늘리며 호출자에게 문제를 떠넘긴다
  - null도 반환한다 -> 빈 리스트를 반환한다 -> 더 나아가 자바에는 Collections.emptyList()가 있다. 

- null을 전달하지도 마라
  - 반환하는 것도 나쁘지만 전달하는건 더 나쁘다

### 08.경계
- 경계 살피고 익히기
  - 학습 테스트
  
- 아직 존재하지 않는 코드를 사용하기
  - 어댑터 패턴
  
### 09.단위 테스트

- TDD의 법칙 세 가지

- 깨끗한 테스트 코드 유지하기

- 이중 표준

- 개념 당 assert 문 수를 줄이고 테스트 함수 하나는 개념 하나만 테스트하라

- F.I.R.S.T
  - fast 빠르게
  - independent 독립적으로
  - repeartable 반복가능하게
  - self-validating 자가검증하는 : 성공과 실패 결과를 내야 한다. 로그를 읽게 만들어서는 안된다
  - timely 적시에 

### 10.클래스


  
  
 
 
 
 
