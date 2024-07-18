# QuickService

**QuickService Web Page v1.0**

![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/main.png)
[딜리메이트](https://github.com/juseungpark97/Semiproject)


**개발기간**: 2024.06 ~ 2024.07

## 웹개발팀 인원

- 박주승, 이주빈, 오민혁, 정인석, 이은호, 강경호

## 프로젝트 소개

**QuickService**는 빠르고 효율적인 서비스 제공을 목표로 하는 웹 애플리케이션입니다. 

### 주요 기능

- 주문 생성
- 채팅방(고객, 라이더 1:1 채팅방)
- 목록 조회/상세 조회
- 회원가입/고객 및 라이더 로그인 (아이디/비밀번호 찾기)
- 파일 첨부
- 고객 문의
- 신고하기
- 긴급배송

### 사용된 기술 스택

- **프론트엔드**:
  - HTML5
  - CSS3
  - JavaScript
- **백엔드**:
  - Spring Framework(Version: 3.9.18.RELEASE)
- **데이터베이스**:
  - Oracle Database 21c Express Edition
- **개발 환경**:
  - Java(TM) 플랫폼: 11.0.21, 0.2
  - Oracle IDE: 23.1.1.345.2114
- **서버**:
  - Tomcat v9.0 Server

### 사용한 API
  - 카카오맵API
  - 카카오모빌리티API

### 담당 기능

- 회원가입
  ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/signup.png)
```javascript
function formatPhoneNumber(input) {
    // 입력 값에서 숫자 이외의 모든 문자를 제거
    let cleaned = input.value.replace(/\D/g, ''); 
    let formatted = ''; // 형식화된 전화번호를 저장할 변수

    // 입력된 숫자의 길이가 3보다 크고 7 이하인 경우
    if (cleaned.length > 3 && cleaned.length <= 7) {
        // 첫 세 자리 숫자 뒤에 하이픈을 추가
        formatted = cleaned.replace(/(\d{3})(\d+)/, '$1-$2'); 
    } 
    // 입력된 숫자의 길이가 7보다 큰 경우
    else if (cleaned.length > 7) {
        // 첫 세 자리 숫자와 그 다음 네 자리 숫자 뒤에 하이픈을 추가
        formatted = cleaned.replace(/(\d{3})(\d{4})(\d+)/, '$1-$2-$3'); 
    } 
    // 입력된 숫자의 길이가 3 이하인 경우
    else {
        // 그대로 할당
        formatted = cleaned; 
    }

    // 입력 값에 형식화된 전화번호를 할당
    input.value = formatted; 
}

function cleanPhoneNumber() {
    // 'phone' 아이디를 가진 입력 요소를 선택
    let phoneInput = document.getElementById('phone'); 
    // 입력 값에서 모든 하이픈을 제거
    phoneInput.value = phoneInput.value.replace(/-/g, ''); 
}

// cleanPhoneNumber를 따로 사용한 이유는 DB에 Phone컬럼 크기가 11이기 때문이다.
// 하이픈이 들어가게 되면 11을 초과하므로 DB에 올바르게 들어갈 수 없기 때문에 그것을 없애주면서 사용자가 보기에 편한 로직을 짰다.
<button class="tooltip" id="" onclick="cleanPhoneNumber()">회원가입</button>
```
  
- 주문 생성, 카카오 맵 API
  ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/주문생성.png)
- 채팅방(고객, 라이더 1:1 채팅방)
  ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/채팅방.png)



## 연락처
프로젝트 관련 문의는 다음 이메일로 연락해주세요: [parkjuseung0313@gmail.com]

