# 📊 이미지 처리 모델을 활용한 게이미피케이션</br>

제가 직접 구현한 스크립트를 정리한 저장소 입니다. (DB.py 파일)

전체 시연 영상 및 결과물은 👉 [노션 포트폴리오](https://magical-rate-172.notion.site/I-Son-56c028578c5f44ba86fa2b6d245c4b27) 에서 확인하실 수 있습니다.</br>

이 스크립트는 JSON 파일에서 사용자 및 게임 데이터를 읽어 PostgreSQL 데이터베이스에 저장하고, 파일이 수정될 경우 자동으로 해당 데이터를 반영하는 시스템입니다.

---
## 📌 주요 기능

| 기능 | 설명 |
|------|------|
| ✅ JSON → PostgreSQL 자동 로딩 | `users.json`, `game1.json`, `game2.json` 데이터를 DB에 적재 |
| ✅ 중복 방지 로직 적용 | 기존 데이터 존재 시 삽입 생략 |
| ✅ 파일 자동 감지 | Watchdog을 활용해 JSON 파일 변경 시 자동 갱신 |
| ✅ 로그 조건 필터링 | 특정 조건에 따라 파일별 로깅 처리 가능 |
| ✅ 스레드 기반 동작 | Flask 서버와 파일 감시 기능을 병렬로 실행

---
## 🛠️ 사용 기술

- **Flask** –  웹 프레임워크
- **SQLAlchemy** – ORM 및 PostgreSQL 연동
- **Watchdog** – 파일 시스템 이벤트 감지
- **PostgreSQL** – 관계형 데이터베이스
- **Threading** – 병렬 처리를 위한 파이썬 표준 모듈

---
## ⚙️ 데이터 모델

- `client_info`: 사용자 정보 (ID, 이름)
- `game1`: 사용자 위치 + 평가 기록 (좌표, 라벨, 시간 등)
- `game2`: 게임 점수 및 스테이지 기록
- `game3`: (정의됨, 아직 사용 안됨)

---

## 📂 주요 스크립트 설명

 ### 1️⃣ `DB.py`  
  **전체 데이터베이스 설계 및 JSON → DB 적재를 관리하는 메인 로직**  
  - Flask + SQLAlchemy를 이용해 PostgreSQL DB 연동
  - 테이블 스키마 정의: `client_info`, `game1`, `game2`, `game3`
  - `client_info.json` → 사용자 정보 삽입
  - `game1.json`, `game2.json`, `game3.json` → 각 게임별 데이터 삽입
  - JSON 파싱, 중복 검사, DB 삽입 및 예외 처리
  - Watchdog 기반 파일 변경 감지 → JSON 변경 시 자동 적재 기능 구현
  - 쓰레드 기반 실시간 파일 감시 기능 탑재

### 2️⃣`client_info.json`  
  **사용자 기본 정보 JSON**
  - 각 사용자 ID 및 닉네임 포함
  - DB의 `client_info` 테이블에 매핑됨

### 3️⃣`game1.json`  
  **게임1 기록 데이터**
  - 각 기록: 시간, 좌표(x, y), 라벨(label), 평가(evaluation), 페이지 정보 포함
  - DB의 `game1` 테이블에 적재됨

### 4️⃣`game2.json`  
  **게임2 기록 데이터**
  - 각 기록: 시간, 좌표(x, y), 스코어(score), 스테이지(stage), 평가(evaluation), 페이지 정보 포함
  - DB의 `game2` 테이블에 적재됨

### 5️⃣`game3.json` 
  **게임3 기록 데이터**
  - 각 기록: 시간, 좌표(x, y), 라벨(label), 평가(evaluation) 포함
  - DB의 `game3` 테이블에 적재됨
---
## 🔒 참고 사항

> 해당 프로젝트는 팀 공동 소유이며,  
> 위 명시된 코드는 본인이 단독 또는 주도적으로 작성한 내용입니다.</br>
> Json 파일은 모두 예시 파일이며 실제 데이터와 같은 형식으로 작성되었습니다.</br>
> 개인정보나 민감한 설정 정보는 포함되어 있지 않습니다.
