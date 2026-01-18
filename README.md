# 📚 월별 독서 타임라인 (Monthly Reading Timeline)

Notion을 CMS로 활용한 개인 독서 기록 및 리뷰 공유 웹사이트입니다.

## ✨ 주요 기능

- 📅 **월별 타임라인**: 읽은 책을 월별로 그룹핑하여 시각적으로 표시
- 📖 **책 정보 카드**: 표지, 제목, 저자, 별점, 장르, 리뷰 등 상세 정보 제공
- 🏷️ **장르 필터링**: 원하는 장르의 책만 선택하여 확인
- 📊 **독서 통계**: 총 독서량, 이번 달 독서량 등 통계 대시보드
- 📱 **반응형 디자인**: 모바일, 태블릿, 데스크톱 모든 화면에 최적화

## 🛠️ 기술 스택

- **Framework**: Next.js 15.5.3 (App Router)
- **Language**: TypeScript 5
- **Runtime**: React 19.1.0
- **Styling**: Tailwind CSS v4 + shadcn/ui
- **CMS**: Notion API
- **Icons**: Lucide React
- **Development**: ESLint + Prettier + Husky

## 🚀 시작하기

### 사전 요구사항

- Node.js 18.17 이상
- npm 또는 yarn
- Notion 계정 및 API Key

### 설치 방법

1. 저장소 클론

```bash
git clone https://github.com/YOUR_USERNAME/notion-cms-project.git
cd notion-cms-project
```

2. 의존성 설치

```bash
npm install
```

3. 환경 변수 설정

`.env.local` 파일을 생성하고 다음 내용을 추가합니다:

```env
NOTION_API_KEY=your_notion_api_key
NOTION_DATABASE_ID=your_notion_database_id
```

#### Notion API 설정 방법

1. [Notion Integrations](https://www.notion.so/my-integrations) 페이지에서 새 Integration 생성
2. Integration Token (API Key) 복사
3. Notion에서 독서 기록 데이터베이스 생성
4. 데이터베이스를 Integration과 연결 (Share 메뉴에서)
5. 데이터베이스 ID를 URL에서 복사 (https://notion.so/workspace/{database_id}?v=...)

### 개발 서버 실행

```bash
npm run dev
```

브라우저에서 [http://localhost:3000](http://localhost:3000)을 열어 결과를 확인하세요.

### 빌드 및 프로덕션

```bash
# 프로덕션 빌드
npm run build

# 프로덕션 서버 실행
npm start
```

## 📖 Notion 데이터베이스 구조

Notion에서 다음 구조로 데이터베이스를 생성하세요:

| 속성명 | 타입 | 설명 |
|--------|------|------|
| 책 제목 | Title | 책 이름 (필수) |
| 저자 | Text | 저자명 (필수) |
| 표지 이미지 | Files & media | 책 표지 URL (필수) |
| 읽은 날짜 | Date | 완독 날짜 (필수) |
| 별점 | Select | ⭐⭐⭐⭐⭐ ~ ⭐ (필수) |
| 장르 | Multi-select | 소설, 에세이, 자기계발, 기술 등 (필수) |
| 한줄평 | Text | 짧은 요약 (선택) |
| 상세 리뷰 | Rich text | 자세한 독서 후기 (선택) |

자세한 내용은 [PRD 문서](docs/PRD.md)를 참조하세요.

## 📁 프로젝트 구조

```
notion-cms-project/
├── docs/                  # 프로젝트 문서
│   ├── PRD.md            # 제품 요구사항 정의서
│   └── guides/           # 개발 가이드 문서
├── src/
│   ├── app/              # Next.js App Router
│   ├── components/       # React 컴포넌트
│   ├── lib/              # 유틸리티 함수
│   └── styles/           # 글로벌 스타일
├── public/               # 정적 파일
└── ...
```

## 📚 문서

- [PRD (제품 요구사항 정의서)](docs/PRD.md)
- [프로젝트 구조 가이드](docs/guides/project-structure.md)
- [스타일링 가이드](docs/guides/styling-guide.md)
- [컴포넌트 패턴](docs/guides/component-patterns.md)

## 🔧 개발 스크립트

```bash
# 개발 서버 실행 (Turbopack)
npm run dev

# 프로덕션 빌드
npm run build

# 프로덕션 서버 실행
npm start

# 린트 검사
npm run lint

# 코드 포맷팅
npm run format

# 모든 검사 실행 (권장)
npm run check-all
```

## 🎨 주요 라이브러리

- **UI Components**: [shadcn/ui](https://ui.shadcn.com/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
- **Icons**: [Lucide React](https://lucide.dev/)
- **Notion API**: [@notionhq/client](https://github.com/makenotion/notion-sdk-js)

## 📄 라이선스

MIT License

## 🤝 기여하기

이슈와 PR을 환영합니다!

## 📮 문의

프로젝트에 대한 질문이나 제안사항이 있으시면 Issue를 열어주세요.
