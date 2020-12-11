# book_study
- 인상깊고 기억하고 싶은 내용 정리

- [Clean Code](#Clean-Code)
- Refactoring
- Effective Java


#Clean Code
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
- 클래스는 작아야 한다
  - 클래스 이름이 모호하다면, 클래스 책임이 너무 많아서이다.
  
- 단일 책임 원칙
  - 규모가 큰 시스템일수록 논리가 많고 복잡하다. 이런 복잡성을 다루려면 체계적인 정리가 필수다.
  - 그래야 변경을 가할 때 직접 영향이 미치는 컴포넌트만 이해해도 충분하다.
  - 큰 클래스 몇 개가 아니라 작은 클래스 수십개가 더 바람직하다.
  - 맡은 책임이 하나며, 변경할 이유가 하나며, 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.
  
- 응집도
  - 응집도를 유지하면 작은 클래스 여럿이 나온다.(p179) 코드 참조
  - 1쪽->3쪽 늘어났다. 1.좀 더 길고 서술적인 변수 사용 2.코드에 주석을 추가하는 수단으로 함수/클래스 선언 3.가독성 높이고자 공백 추가, 형식 맞춤
  
- 변경하기 쉬운 클래스

- 변경으로부터 격리

### 11.시스템

- 시스템 제작과 시스템 사용을 분리하라

- 확장

- 테스트 주도 시스템 아키텍처 구축
  - 코드 수준에서 아키텍처 관심사를 분리할 수 있다면, 진정한 테스트 주도 아키텍처 구조
  - 그때그때 새로운 기술을 채택해 단순한 아키텍처를 복잡한 아키텍처로 키워갈 수도 있다
  
### 12.창발성

- 창발적 설계로 깔끔한 코드를 구현하자
  - 모든 테스트를 실행한다
  - 중복을 없앤다
  - 프로그래머 의도를 표현한다
  - 클래스와 메서드 수를 최소로 줄인다
  
- 리팩터링
  - 코드를 정리하면서 시스템이 깨질까 걱정할 필요가 없다. 테스트 케이스가 있으니까!
  
- 단 몇줄이라도 중복을 제거하겠다는 의지
  - 템플릿 메서드 패턴(p219)
  - 메서드별 휴가 일수, 각 나라별 법정일수 만족하는 확인코드, 급여대장 적용(국가별 법정 일수 제외하면 두 메서드는 거의 동일하다) 
  - -> 패턴 적용해 국가별 법정 일수 사용
  
- 표현하라
  - 좋은 이름을 선택한다
  - 함수와 클래스 크기를 가능한 줄인다
  - 표준 명칭을 사용한다. 디자인 패턴은 의사소통과 표현력 강화가 주요 목적이다.
  - 클래스가 COMMAND나 VISTOR와 같은 표준 패턴을 사용해 구현된다면 클래스 이름에 패턴 이름을 넣어준다.
  - 단위 테스트 케이스를 꼼꼼히 작성한다.

- 하지만 표현력을 높이는 가장 중요한 방법은 '노력'이다.
- 나중에 코드를 읽을 사람을 고려하자. 나 자신이 될 수도 있다.

### 13.동시성

- 동시성의 미신과 오해
  - 동시성은 항상 성능을 높여준다 -> 때로 성능을 높여준다.
  - 동시성을 구현해도 설계는 변하지 않는다 -> 단일 스레드 시스템과 다중 스레드 시스템은 설계가 판이하게 다르다.
    - 일반적으로 무엇과 언제를 분리하면 시스템 구조가 크게 바뀐다
  - 실제로 컨테이너가 어떻게 동작하는지, 어떻게 동시 수정, 데드락 같은 문제를 피할수 있는지 알아야만 한다.
  
- 동시성은 부하 유발, 복잡하다 
- 동시성 버그는 재현하기 어려워 일회성 문제로 넘겨 무시하기 쉽다.  
- 둥시성을 구현하려면 근본적인 설계 전략을 재고해야 한다.

- 동시성 방어 원칙
  - 단일 책임 원칙, 동시성 코드는 다른 코드와 분리하라
  - 따름 정리 : 자료 범위를 제한하라
    - 코드 내 임계영역, synchronized 키워드 보호, 이런 임계영역의 수를 줄이는 기술이 중요.
    - 자료를 캠슐화, 공유 자료를 최대한 줄여라
  - 자료 사본을 사용하라
  - 스레드는 가능한 독립적으로
    - 독자적 스레드로, 가능한 다른 프로세스에서, 돌려도 괜찮도록 자료를 독립적인 단위 분할

- 라이브러리를 이해하라
  - 일부 클래스 라이브러리는 스레드에 안전하지 못하다
  - 언어가 제공하는 클래스를 검토하라
    - java.util.concurrent, java.util.concurrent.atomic, java.util.concurrent.locks, ConcurrentHashMap 등

- 실행 모델을 이해하라
  - 기본 용어 : 한정된 자원, 상호 배제, 기아, 데드락, 라이브락
  - 생산자-소비자, 읽기-쓰기, 식사하는 철학자들
  
- 동기화하는 메서드 사이에 존재하는 의존성을 이해하라
  - 공유 클래스 하나에 동기화된 메서드가 여럿이라면 구현이 올바른지 다시 한번 확인하길 바란다.
  
  
## 14.점진적인 개선
- 다시 코드 전체적으로 살펴볼것(좀 더 발전한 후에 다시 보면 더 많은게 보일 것 같다)

## 15.Junit 들여다보기
- 의도를 명확히 표현하려면 조건문을 캡슐화
- 부정문은 긍정문보다 이해하기 약간 더 어렵다

## 17.냄새와 휴리스틱
- G14.기능욕심
  - 클래스 메서드는 자기 클래스의 변수와 함수에 관심을 가져야지, 다른 클래스의 변수,함수에 관심을 가져서는 안된다
  
- G15.선택자 인수
  - 목적을 기억하기 어려울 뿐 아니라, 각 선택자 인수가 여러 함수를 하나로 조합한다.
  - 큰 함수를 여럿으로 쪼개지 않으려는 게으름의 소산
  
- G16.모호한 의도

- G19.서술적 변수
  - 프로그램 가독성을 높이는 가장 효과적인 방법
    - 계산을 여러 단계로 나누고 중간 값으로 서술적인 변수 이름을 사용하는 방법
    - 계산을 몇 단계로 나누고 중간값에 좋은 변수 이름만 붙여도 순식간에 읽기 쉬운 모듈로 탈바꿈한다.
    
- G20.이름과 기능이 일치하는 함수
  - date.add(5) -> 5일을 더하는가? 5주?5시간? 현재 인스턴스를 변경하나? 새로운 인스턴스를 반환하나?
  - addDaysTo(5) / increaseByDays(5) / daysLater(5) / daysSince(5) ...

- G28.조건을 캡슐화하라
  - if (shouldBeDeleted(timer)) O
  - if (timer.hasExpired() && !timer.isRecurrent()) X

- G29.부정 조건은 피하라

- G30.함수는 한 가지만 해야 한다
  - 직원 목록을 루프로 돌며, 각 직원의 월급일을 확인하고, 해당 직원에게 월급을 지급한다
  - 다 각자의 함수로 구현

- N7.이름으로 부수 효과를 설명하라
  - getOos() -> 단순히 oos만 가져오지 않고, 기존에 oos 가 없으면 생성한다
  - -> 그러므로 createOrReturnOos()
  
- T2.커버리지 도구를 사용하라  
  
  
  
  
