# 스마트 조퇴·외출증 시스템

이 프로젝트는 학교 교직원을 위한 스마트 조퇴·외출증 자동 발급 시스템입니다. 기존 종이 기반의 조퇴·외출증 처리 과정을 디지털화하여 행정 업무를 간소화하고, 기록 누락을 방지하기 위해 제작되었습니다.

## 주요 기능
- **Google 계정 로그인**: Firebase Authentication을 연동하여 학교 교직원 구글 계정(`@sen.go.kr`, `@school.kr` 등)으로 안전하게 로그인할 수 있습니다.
- **조퇴·외출증 자동 발급**: 학생 이름, 학급, 신청 유형, 시간, 사유 등을 입력하면 즉시 발급 번호와 함께 디지털 패스를 생성합니다.
- **실시간 데이터베이스 연동**: Firebase Firestore를 사용하여 발급된 기록이 실시간으로 클라우드에 저장되고 조회됩니다.
- **AI 도우미**: 사용 방법이나 입력 항목에 대한 기본적인 질문에 응답할 수 있는 간단한 챗봇 인터페이스를 제공합니다.

## 실행 방법
이 프로젝트는 별도의 빌드 과정이 필요 없는 순수 HTML/CSS/JS 기반의 정적 웹 애플리케이션입니다. 

1. 소스 코드를 다운로드 받거나 Clone 합니다.
2. 로컬 서버(VS Code Live Server 등)를 이용해 `index.html` 파일을 실행합니다. 
   *(주의: Firebase 모듈을 사용하므로 `file://` 프로토콜 대신 `http://` 로컬 서버 환경에서 실행해야 정상 작동합니다.)*

## Firebase 설정 방법
현재 코드는 Firebase 연동이 준비되어 있습니다. 본인의 Firebase 프로젝트와 연결하려면 아래 과정을 진행하세요.

1. [Firebase Console](https://console.firebase.google.com/)에서 새 프로젝트를 생성합니다.
2. **Authentication** 메뉴에서 **Google 로그인**을 활성화합니다.
3. **Firestore Database** 메뉴에서 데이터베이스를 생성합니다.
4. 프로젝트 설정(기어 아이콘)에서 '웹 앱'을 추가합니다.
5. 앱 추가 후 나타나는 `firebaseConfig` 객체의 값(API Key 등)을 복사합니다.
6. `index.html` 파일의 `<script type="module">` 내부에 있는 `firebaseConfig` 변수를 본인의 값으로 교체합니다.

## Firestore 보안 규칙 예시
테스트를 위해 Firestore 보안 규칙을 아래와 같이 임시로 설정할 수 있습니다. 운영 환경에서는 반드시 인증된 사용자만 접근 가능하도록 규칙을 수정해야 합니다.

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```