# Firebase 설정 가이드

커피퀘스트 앱에서 PC/모바일 간 데이터 연동을 위한 Firebase 설정 방법입니다.

## 1. Firebase 프로젝트 생성

### 1-1. Firebase Console 접속
https://console.firebase.google.com/ 접속 후 Google 계정으로 로그인

### 1-2. 프로젝트 생성
1. **"프로젝트 추가"** 클릭
2. 프로젝트 이름 입력 (예: `coffee-quest`)
3. Google Analytics 설정 (선택사항, 나중에 설정 가능)
4. **"프로젝트 만들기"** 클릭
5. 프로젝트 생성 완료될 때까지 대기

---

## 2. Realtime Database 설정

### 2-1. Realtime Database 만들기
1. 좌측 메뉴에서 **"빌드" → "Realtime Database"** 클릭
2. **"데이터베이스 만들기"** 클릭
3. 위치 선택:
   - **추천**: `asia-southeast1` (싱가포르 - 한국과 가까움)
   - 다른 옵션: `us-central1` (미국)
4. **"다음"** 클릭

### 2-2. 보안 규칙 설정
다음 두 옵션 중 선택:

**옵션 1: 테스트 모드 (빠른 시작, 보안 약함)**
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
- 장점: 즉시 사용 가능
- 단점: 누구나 데이터 접근 가능
- 권장: 개인 사용 또는 테스트용

**옵션 2: PIN 기반 보안 (추천)**
```json
{
  "rules": {
    "users": {
      "$pin": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```
- 장점: PIN 코드로 데이터 분리
- 단점: 설정 필요
- 권장: 장기 사용

5. 원하는 옵션 선택 후 **"사용 설정"** 클릭

---

## 3. 웹 앱 설정

### 3-1. 앱 등록
1. 프로젝트 개요 페이지에서 **웹 아이콘** (`</>`) 클릭
2. 앱 닉네임 입력 (예: `Coffee Quest App`)
3. **"Firebase Hosting 설정"** 체크 해제 (GitHub Pages 사용 중)
4. **"앱 등록"** 클릭

### 3-2. Firebase 설정 정보 복사
다음과 같은 설정 코드가 표시됩니다:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project.firebaseio.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:xxxxxxxxxxxxx"
};
```

이 정보를 **메모장에 복사**해두세요!

---

## 4. 코드에 Firebase 설정 적용

### 4-1. index.html 파일 열기
GitHub에서 `index.html` 파일을 편집 모드로 엽니다.

### 4-2. Firebase 설정 교체
파일에서 다음 부분을 찾으세요 (약 680번째 줄):

```javascript
// ==========================================
// Firebase 설정 (여기에 본인의 설정을 넣으세요!)
// ==========================================
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

이 부분을 복사한 Firebase 설정으로 **통째로 교체**합니다.

### 4-3. 예시
**변경 전:**
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    // ...
};
```

**변경 후:**
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyBxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    authDomain: "coffee-quest-12345.firebaseapp.com",
    databaseURL: "https://coffee-quest-12345.firebaseio.com",
    projectId: "coffee-quest-12345",
    storageBucket: "coffee-quest-12345.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:xxxxxxxxxxxxx"
};
```

### 4-4. 저장
GitHub에서 **"Commit changes"** 클릭하여 저장합니다.

---

## 5. 앱에서 연동 확인

### 5-1. 앱 새로고침
- PC와 모바일 모두에서 앱을 강력 새로고침 (`Ctrl+Shift+R` 또는 `Cmd+Shift+R`)
- 또는 시크릿/비공개 모드로 접속

### 5-2. PIN 코드 확인
1. 앱 우측 상단 **⚙️ (설정)** 버튼 클릭
2. "내 PIN 코드" 섹션에 **4자리 숫자** 표시됨 (예: 7482)
3. 이 코드를 다른 기기에서 입력하면 연동됩니다!

### 5-3. 연동 테스트
1. **PC**에서 설정 버튼을 눌러 PIN 코드 확인 (예: 7482)
2. **모바일**에서 앱 접속
3. 설정 → "다른 기기 연동" → PIN 코드 입력 (7482)
4. "연동하기" 클릭
5. 상단에 "✅ 동기화 완료" 메시지 표시
6. PC에서 음료 사용 → 즉시 모바일에도 반영!

---

## 6. 문제 해결

### ❌ "Firebase 설정이 필요합니다" 메시지가 계속 뜨는 경우
- Firebase 설정을 올바르게 복사했는지 확인
- 특히 `apiKey`가 `"YOUR_API_KEY"`가 아닌 실제 값인지 확인
- 파일을 저장하고 GitHub에 업로드했는지 확인
- 캐시를 완전히 지우고 새로고침

### ❌ "동기화 실패" 메시지
- Firebase Console → Realtime Database → 규칙 탭 확인
- 규칙이 올바르게 설정되었는지 확인
- 인터넷 연결 확인

### ❌ 데이터가 동기화되지 않음
- 두 기기 모두 같은 PIN 코드를 사용하는지 확인
- 두 기기 모두 인터넷에 연결되어 있는지 확인
- Firebase Console → Realtime Database → 데이터 탭에서 데이터가 저장되는지 확인

### ✅ 정상 작동 확인 방법
Firebase Console → Realtime Database → 데이터 탭에서 다음과 같은 구조가 보여야 합니다:

```
users/
  └─ 7482/
      ├─ balance: 45000
      ├─ month: 0
      ├─ year: 2025
      └─ history/
          └─ 0/
              ├─ name: "아메리카노"
              ├─ price: 4000
              ├─ temp: "hot"
              └─ ...
```

---

## 7. 보안 강화 (선택사항)

더 안전하게 사용하려면 Firebase 보안 규칙을 강화하세요:

```json
{
  "rules": {
    "users": {
      "$pin": {
        ".read": "$pin.length === 4",
        ".write": "$pin.length === 4",
        ".validate": "newData.hasChildren(['balance', 'month', 'year', 'history'])"
      }
    }
  }
}
```

이 규칙은:
- PIN이 정확히 4자리인 경우만 접근 허용
- 데이터 구조가 올바른 경우만 저장 허용

---

## 완료! 🎉

이제 PC와 모바일에서 데이터가 실시간으로 동기화됩니다!

**참고:** Firebase 무료 플랜(Spark)은 다음 한도를 제공합니다:
- 동시 연결: 100개
- 저장 용량: 1GB
- 데이터 전송: 10GB/월

개인 사용으로는 충분합니다! 😊
