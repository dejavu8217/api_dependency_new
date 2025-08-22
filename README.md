# API Dependency Visualizer 🔍

고급 API 의존성 시각화 및 리스크 분석 도구입니다. OpenAPI 사양을 기반으로 API 간의 복잡한 의존 관계를 시각화하고 분석합니다.

## 🌟 주요 기능

### 시각화 모드
- **그룹 뷰**: API를 논리적 그룹으로 분류하여 표시
- **상세 뷰**: 개별 API 엔드포인트 간의 상세 의존성 표시
- **매트릭스 뷰**: 그룹 간 의존성을 매트릭스 형태로 표시
- **계층 뷰**: 트리 구조로 API 계층 관계 표시

### 분석 기능
- **의존성 자동 감지**: CRUD 패턴, 리소스 관계, 워크플로우 등 자동 분석
- **순환 의존성 탐지**: Tarjan 알고리즘을 사용한 순환 참조 감지
- **리스크 평가**: 의존성 복잡도, 메서드 위험도, 성능 메트릭 기반 평가
- **영향도 분석**: API 변경 시 영향 범위 예측

### 데이터 지원
- OpenAPI 3.0 / Swagger 2.0 JSON/YAML 파일
- 성능 메트릭 JSON/CSV 파일
- 실시간 검색 및 필터링
- 샘플 데이터 제공

## 🚀 빠른 시작

### 온라인 데모
GitHub Pages에서 바로 사용해보세요:
```
https://[your-username].github.io/api-dependency-visualizer/
```

### 로컬 실행
```bash
# 레포지토리 클론
git clone https://github.com/[your-username]/api-dependency-visualizer.git

# 디렉토리 이동
cd api-dependency-visualizer

# 로컬 서버 실행 (Python 3)
python -m http.server 8000

# 또는 Node.js 사용
npx http-server

# 브라우저에서 열기
open http://localhost:8000
```

## 📁 프로젝트 구조

```
api-dependency-visualizer/
├── index.html              # 메인 HTML 파일
├── README.md              # 프로젝트 문서
├── styles/                # CSS 스타일시트
│   ├── main.css          # 기본 스타일
│   ├── components.css    # 컴포넌트 스타일
│   └── graph.css         # 그래프 관련 스타일
├── js/                    # JavaScript 모듈
│   ├── config.js         # 설정 및 상수
│   ├── utils.js          # 유틸리티 함수
│   ├── data-parser.js    # 데이터 파싱 로직
│   ├── graph-renderer.js # 그래프 렌더링
│   ├── interactions.js   # 사용자 인터랙션
│   └── main.js           # 메인 앱 로직
└── samples/              # 샘플 데이터
    ├── openapi.json      # 샘플 OpenAPI 스펙
    └── metrics.json      # 샘플 메트릭 데이터
```

## 📊 사용 방법

### 1. OpenAPI 파일 업로드
- "OpenAPI 업로드" 버튼을 클릭하여 OpenAPI JSON/YAML 파일 선택
- 자동으로 API 엔드포인트와 의존성을 분석

### 2. 메트릭 데이터 추가 (선택사항)
- "메트릭 업로드" 버튼으로 성능 데이터 추가
- 호출 빈도, 응답 시간, 오류율 등 시각화

### 3. 분석 및 탐색
- 노드 클릭: 상세 정보 및 의존성 확인
- 그룹 선택: 그룹 간 관계 분석
- 검색: API 이름, 경로, 그룹으로 필터링
- 뷰 전환: 다양한 시각화 모드 활용

## ⌨️ 단축키

| 단축키 | 기능 |
|--------|------|
| `ESC` | 선택 해제 |
| `Ctrl+F` | 검색 포커스 |
| `1-4` | 뷰 모드 전환 |
| `+/-` | 확대/축소 |
| `0` | 줌 초기화 |
| `F` | 화면 맞춤 |

## 🎨 의존성 타입

### 자동 감지되는 의존성
- **CRUD 관계**: Create → Read/Update/Delete
- **인증 요구**: 인증 API → 보호된 리소스
- **워크플로우**: 주문 → 결제 프로세스
- **참조 관계**: 주문 → 상품 참조
- **계층 구조**: 부모 → 자식 리소스
- **데이터 소스**: 리포트 → 데이터 API

### 색상 코드
- 🔵 **파란색**: 나가는 의존성 (Outgoing)
- 🟠 **주황색**: 들어오는 의존성 (Incoming)
- 🟣 **보라색**: 인증 관련
- 🟢 **초록색**: 정상 관계
- 🔴 **빨간색**: 순환 의존성 또는 위험

## 📈 리스크 평가 기준

### 점수 산정 (0-10점)
1. **의존성 복잡도** (0-4점)
   - 들어오는/나가는 의존성 개수
   - 로그 스케일 적용

2. **메서드 위험도** (0-4점)
   - DELETE: +1.5점
   - PUT/PATCH: +0.7점
   - 인증/보안 그룹: +2.0점
   - 결제 그룹: +1.5점

3. **성능 메트릭** (0-3점)
   - 오류율 > 5%: +1.5점
   - 응답시간 > 1000ms: +1.0점
   - 호출량 > 10000: +0.5점

### 위험 수준
- 🟢 **낮음** (0-3점): 안정적
- 🟡 **중간** (3-7점): 주의 필요
- 🔴 **높음** (7-10점): 즉각 조치 필요

## 🔧 커스터마이징

### 그룹 매핑 수정
`js/config.js`에서 그룹 매핑 규칙 수정:
```javascript
const GROUP_MAPPINGS = {
    'auth': '인증·보안',
    'user': '사용자·멤버',
    // 추가 매핑...
};
```

### 색상 테마 변경
`styles/main.css`의 CSS 변수 수정:
```css
:root {
    --color-accent-primary: #EA4D08;
    --color-accent-secondary: #667eea;
    /* 추가 색상... */
}
```

## 📝 데이터 형식

### OpenAPI 형식
```json
{
  "openapi": "3.0.0",
  "paths": {
    "/users/{id}": {
      "get": {
        "summary": "사용자 조회",
        "tags": ["user"]
      }
    }
  }
}
```

### 메트릭 형식
```json
[
  {
    "id": "GET_/users/{id}",
    "calls": 15000,
    "p95": 250,
    "errorRate": 0.01
  }
]
```

## 🌐 브라우저 지원
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## 📄 라이선스
MIT License

## 🤝 기여하기
이슈 및 PR을 환영합니다!

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📧 문의
질문이나 제안사항이 있으시면 이슈를 생성해주세요.

---
Made with ❤️ for API developers
