# java-calculator-precourse

## 🚀 구현할 기능 목록 

### ⌨️ 입력

- [x] 덧셈할 문자열을 입력받는 기능을 구현한다.
  - [x] 덧셈 문자열 입력 안내 메시지를 출력한다.
  - [x] 사용자로부터 덧셈할 문자열을 입력받는다.
  - [x] 입력된 문자열을 검증한다.
    - [x] 숫자가 하나라도 포함되어있지 않은 경우, 예외를 반환한다.
    - [x] 기본 구분자 중 하나라도 포함되어있지 않은 경우, 예외를 반환한다.
  - [x] 입력된 문자열을 파싱 및 추가 검증한다.
    - [x] 입력값에서 편리하게 숫자를 추출하기 위해 전처리를 진행한다.
      - [x] 문자열에 커스텀 구분자가 포함되어 있는지 확인한다.
      - [x] 문자열 앞 부분에 커스텀 구분자가 있는 경우, 이를 따로 저장한다.
      - [x] 커스텀 구분자를 뺸 문자열을 입력값으로 최종 저장한다.
    - [x] 문자열로부터 숫자들을 추출한다.
      - [x] 주어진 문자열에 구분자가 포함되어 있는지 확인한다.
      - [x] 만약 구분자가 포함되어 있는 경우, 구분자를 기준으로 문자열을 분리한다.
      - [x] 구분자를 통해 분리된 값이 빈 문자열일 경우, 결과로 0을 반환한다.
      - [x] 구분자를 통해 분리된 값이 숫자가 아닐 경우, 예외를 반환한다.
      - [x] 구분자를 통해 분리된 값이 음수일 경우, 예외를 반환한다.
    - [x] 최종적으로 분리된 숫자들을 리스트에 저장한다.

### 🖥 출력

- [x] 덧셈 결과를 출력한다.
  - [x] "결과"라는 문자열와 덧셈 결과를 ":" 구분자를 통해 구분한다.

### ⚙️ 로직

- [x] 리스트에 저장된 숫자들을 더하는 기능을 구현한다.
  - [x] 리스트에 저장된 숫자들을 모두 더한 결과를 반환한다.

---

## 🛠️ 리팩토링 목록

### 1️⃣ 모든 예외를 처리하지 못하는 있는 현 검증 로직 → 더 많은 예외에 대비하기 위해, 더 많은 검증 메서드 구현

- [x] 커스텀 구분자에 실제 이스케이프 문법("\")이 포함된 경우에 대한 예외 처리를 추가한다.
- [x] 커스텀 구분자에 숫자가 포함되는 경우에 대한 예외 처리를 추가한다.
- [x] 커스텀 구분자가 빈 문자열("")일 경우에 대한 예외 처리를 추가한다.
- [x] 구분자에 의해 분리된 값이 숫자가 아닌 경우에 대한 예외 처리를 추가한다.
- [ ] 구분자에 의해 분리된 값이 소수일 경우("."을 포함)에 대한 예외 처리를 추가한다.
  - [ ] 기존 구분자 목록에 점(".")이 있을 경우, 소수 입력이 불가하도록 예외 처리를 추가한다.
  - [ ] 기존 구분자 목록에 점(".")이 없을 경우, 소수 입력이 가능하도록 예외 처리를 추가한다.
    - [ ] 구분자에 의해 분리된 값에 점(".")이 포함되어 있더라도 예외를 반환하지 않도록 수정한다.

### 2️⃣ 입력값 검증과 파싱 로직이 혼용되는 문제 발생 → 확실하게 책임을 분리

- [x] 입력값 검증과 관련된 메서드들은 모두 `InputValidator`에서 관리하도록 이동시킨다.
- [x] 입력값 파싱 과정에서 쓰이는 검증 메서드들은 `InputParser` 내에서 `InputValidator`를 참조하여 사용하도록 수정한다.

### 3️⃣ 매직 넘버 및 리터럴 사용으로 인한 추후 유지보수에서의 어려움 예상 → 클래스 및 상수로서 관리

- [x] 에러 메시지를 Enum을 통해 상수로 관리하도록 수정한다.
- [ ] 구분자 목록을 Enum을 통해 상수로 관리하도록 수정한다.

### 4️⃣ 숫자 값들이 각 객체와 메서드를 거치며 변경될 여지가 있음 → 불변 개념 적용

- [ ] 숫자 값들을 변경할 수 없도록 `final` 키워드를 추가한다.
- [ ] 숫자 리스트를 전달할 떄, 불변 리스트로 전달될 수 있도록 List.of 또는 unmodifiableList를 적용한다.

### 5️⃣ 정직하게 기본 요구사항에만 집중한 코드 → 추후 확장 시 더 많은 요구사항을 수용할 수 있도록 구조 변경

- [ ] `Calculator` 클래스의 덧셈 연산이 소수 덧셈을 지원하도록 수정한다.
- [ ] 기본 구분자가 변경될 경우를 대비하여, 구분자 목록을 동적으로 관리할 수 있도록 수정한다.
- [ ] 여러 개의 커스텀 구분자를 입력받을 수 있도록 수정한다.

### 6️⃣ 테스트 케이스가 단 2개밖에 없는 상황 → 더 많은 테스트 케이스를 작성하여 테스트 용이성 향상

- [ ] 각 예외 상황에 대한 테스트 케이스를 추가한다.
- [ ] 그 외에 각 메서드들이 정상적으로 동작하는지 확인할 수 있도록 테스트 케이스를 추가한다.

### 7️⃣ 구분자를 단순 필드 변수로 관리중 → 구분자 관리를 위한 별도 클래스 생성

- [x] `Delimiters` 클래스를 생성하여, 구분자 목록을 관리하도록 수정한다.

### 8️⃣ 커스텀 구분자에 대한 처리를 `InputParser` 클래스에서 전담하고 있는 상황 → `CustomDelimiterProcessor` 클래스 생성을 통한 책임 분리

- [ ] 기존 `InputParser` 클래스에 있던 커스텀 구분자 처리 로직을 `CustomDelimiterProcessor` 클래스로 이동시킨다.