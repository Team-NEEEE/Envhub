<div align="center">

# 🔒 EnvHub

### ✨ Vault보다 가볍고, 메모장보다 안전한 환경변수 관리 도구 ✨
</div>

---

## 📋 목차

- [📖 프로젝트 소개](#-프로젝트-소개)
- [🎯 타겟 및 포지셔닝](#-타겟-및-포지셔닝)
- [✨ 주요 기능](#-주요-기능)
---

# 📖 프로젝트 소개

> [!IMPORTANT]
> **EnvHub**는 프로젝트 내 `.env` 파일을 팀 단위로 안전하게 관리하고, 환경(dev/staging/prod)별 변수를 중앙에서 동기화하며, 변경 이력 추적과 접근 제어를 제공하는 웹 기반 환경변수 관리 플랫폼입니다.

기존 .env 파일의 수동 공유, 슬랙/카톡 전송, Git 커밋 실수 등으로 발생하는 보안 사고와 운영 비효율을 해결하며, 팀 협업뿐 아니라 개인 로컬 PC의 API 키·시크릿을 암호화하여 저장하는 **Personal Vault** 기능도 함께 제공합니다.

### 🚨 문제 정의 및 해결
- **비보안적 공유 방지**: 메신저를 통한 평문 비밀번호/API 키 유출 및 신규 팀원 온보딩 시 반복되는 공유 요청 해결
- **환경별 혼란 제거**: dev, staging, prod 등 분산된 변수를 중앙에서 관리하여 배포 장애 예방
- **변경 이력 및 롤백**: 수정자 및 시점을 추적하고, 잘못된 변경 시 이전 상태로 복구 가능
- **개인 API 키 관리**: 셸이나 메모장에 방치되는 개인 키를 체계적으로 암호화하여 관리
- **보안 사고 예방**: `.gitignore` 누락으로 인한 GitHub API 키 노출 사고 원천 차단

---

# 🎯 타겟 및 포지셔닝

**"Vault보다 가볍고, 메모장보다 안전한 환경변수 관리 도구"**

- **SSAFY / 부트캠프 프로젝트 팀**: 6명 내외의 규모에서 별도의 인프라 관리 없이 간편하게 환경변수를 공유하고 싶은 팀
- **사이드 프로젝트 / 소규모 스타트업**: Enterprise급 Vault는 과도하고, 단순 유틸리티만으로는 협업이 부족한 팀
- **주니어 개발자**: 환경변수 관리의 모범 사례를 실천하고 간결한 플랫폼을 원하는 개발자

---

# ✨ 주요 기능

### 1. 프로젝트 및 환경 관리
- 프로젝트 단위 생성 및 dev / staging / production 등 다중 환경 지원
- 웹 UI를 통한 직관적인 키-값(Key-Value) CRUD 및 환경 간 **Diff 시각화** (가상 스크롤링 지원)

### 2. 강력한 CLI (Go 기반)
- 크로스 플랫폼 단일 바이너리 제공
- `envhub pull / push`: 로컬 `.env` 파일과 서버 간 손쉬운 동기화
- `envhub run`: 디스크에 `.env` 파일을 생성하지 않고, TLS 채널을 통해 메모리에 직접 시크릿을 격리 주입하는 **제로 디스크(Zero-disk)** 실행

### 3. Personal Vault (개인 시크릿 금고)
- 팀 프로젝트와 분리된 개인 전용 보관소
- OS 기본 키체인(macOS Keychain, Windows Credential Manager, Linux Secret Service)과 연동하여 오프라인에서도 안전하게 관리

### 4. 강력한 보안 (3계층 Envelope Encryption)
- 서버 메모리에만 상주하는 MK(Master Key)로 DEK를 래핑하여 데이터베이스 필드 레벨 암호화 (AES-256-GCM) 적용
- 클라이언트-서버 간 TLS 1.3 통신 의무화
- Git pre-commit hook을 통한 `.env` 커밋 스캔 및 유출 방지

### 5. 감사 로그 및 역할 기반 접근 제어 (RBAC)
- PostgreSQL `pgaudit` 확장 모듈을 활용하여 모든 DDL/DML 변경 사항 자동 기록 및 롤백 지원
- Owner, Admin, Developer, Viewer 권한 분리로 환경별 접근/수정 제어 및 민감 변수 동적 마스킹

### 6. 외부 연동 및 호환성
- `.env`, JSON, YAML, Excel/CSV 스마트 파싱 지원 (퍼지 매칭을 통한 자동 칼럼 맵핑)
- CI/CD 환경 (GitHub Actions, GitLab CI) 자동 주입 플러그인 제공
- **MCP 서버 연동**: AI 에이전트(Claude, Cursor 등) 연동 시 OAuth 2.1 기반 단기 토큰 및 명시적 사용자 승인(Human-in-the-loop) 파이프라인 제공

---

# 🏗️ 시스템 아키텍처

```text
┌──────────┐  ┌──────────┐  ┌──────────┐
│  Web UI  │  │   CLI    │  │MCP Server│
│ (React)  │  │  (Go)    │  │(OAuth2.1)│
└────┬─────┘  └────┬─────┘  └────┬─────┘
     │             │             │
     └──────┬──────┘─────────────┘
            │
     ┌──────▼───────┐
     │  Spring Boot │
     │  API Server  │
     └──────┬───────┘
            │
   ┌────────┼────────┐
   ▼        ▼        ▼
PostgreSQL  Redis   Nginx
```
---

[deploy-url]: #
[deploy-shield]: https://img.shields.io/badge/-envhub.com-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white