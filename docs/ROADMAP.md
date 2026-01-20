# 📍 월별 독서 타임라인 - 개발 로드맵

> **프로젝트**: 월별 독서 타임라인 (Monthly Reading Timeline)
> **버전**: 1.0.0 (MVP)
> **기술 스택**: Next.js 15.5.3 + React 19 + Notion API

---

## 🎯 개발 전략

이 로드맵은 **의존성 기반 개발 순서**를 따릅니다. 각 단계는 다음 단계의 기반이 되며, 건너뛸 수 없는 순차적 구조입니다.

```
Phase 1 → Phase 2 → Phase 3 → Phase 4 → Phase 5
  기반      구조      핵심      확장      완성
```

---

## Phase 1: 프로젝트 초기 설정 (골격 구축)

### 📦 작업 내용

#### 1.1 개발 환경 설정
- [ ] Next.js 프로젝트 기본 구조 검증
- [ ] 필요한 패키지 설치 확인
  - `@notionhq/client` 설치
  - 환경 변수 설정 (`.env.local`)
- [ ] Git 설정 및 `.gitignore` 검증

#### 1.2 Notion 통합 설정
- [ ] Notion Integration 생성
  - [Notion Developers](https://developers.notion.com/) 접속
  - New Integration 생성
  - API Key 발급받기
- [ ] Notion Database 생성
  - PRD의 데이터베이스 구조에 따라 속성 설정
  - Database ID 복사
- [ ] Integration을 Database에 연결
  - Database 우측 상단 `•••` → `+ Add connections` → Integration 선택
- [ ] 환경 변수 설정
  ```env
  NOTION_API_KEY=secret_xxxxxxxxxxxxx
  NOTION_DATABASE_ID=xxxxxxxxxxxxx
  ```

#### 1.3 기본 API 연결 테스트
- [ ] Notion API 클라이언트 초기화 코드 작성
- [ ] Database 조회 테스트
- [ ] 샘플 데이터 10-15개 입력
- [ ] 데이터 정상 조회 확인

### 🤔 왜 이 순서인가?

프로젝트의 **기반 인프라**를 먼저 구축해야 합니다. Notion API 연동 없이는 데이터를 가져올 수 없고, 데이터 없이는 UI를 개발할 수 없습니다. 이 단계는 모든 후속 작업의 전제 조건입니다.

**의존성 관계**:
- Phase 2는 Notion API 연결이 필요 → Phase 1 완료 필수
- 샘플 데이터가 있어야 UI 개발 시 실제 데이터로 테스트 가능

### ⏱️ 예상 소요 시간

**2-3시간** (Notion 계정이 있고 Next.js 프로젝트가 세팅된 경우)

### ✅ 완료 기준

- [ ] `npm run dev` 실행 시 프로젝트가 정상 작동
- [ ] Notion Database에 샘플 데이터 10개 이상 존재
- [ ] 간단한 API 테스트 코드로 Notion 데이터 조회 성공
- [ ] 환경 변수가 `.env.local`에 올바르게 설정됨
- [ ] `.env.local`이 `.gitignore`에 포함되어 있음

---

## Phase 2: 공통 모듈/컴포넌트 개발 (구조 구축)

### 📦 작업 내용

#### 2.1 TypeScript 타입 정의
- [ ] 책 데이터 인터페이스 정의 (`types/book.ts`)
  ```typescript
  interface Book {
    id: string;
    title: string;
    author: string;
    coverImage: string;
    finishedDate: string;
    rating: number;
    genres: string[];
    oneLiner?: string;
    review?: string;
  }
  ```
- [ ] Notion API 응답 타입 정의
- [ ] 필터 상태 타입 정의

#### 2.2 데이터 레이어 구현
- [ ] Notion API 유틸리티 함수 작성 (`lib/notion.ts`)
  - `getDatabase()`: 전체 데이터 조회
  - `parseNotionBook()`: Notion 데이터 → Book 타입 변환
- [ ] 데이터 가공 유틸리티
  - `groupBooksByMonth()`: 월별 그룹핑
  - `filterBooksByGenres()`: 장르 필터링
  - `calculateStats()`: 통계 계산
- [ ] 캐싱 전략 구현
  - ISR (Incremental Static Regeneration) 설정
  - `revalidate` 옵션 설정 (예: 3600초)

#### 2.3 공통 UI 컴포넌트 개발
- [ ] 레이아웃 컴포넌트
  - `Header`: 사이트 제목, 부제
  - `Container`: 메인 컨테이너
  - `Footer`: 푸터 (선택 사항)
- [ ] 기본 UI 컴포넌트
  - `Badge`: 장르 태그용
  - `Card`: 책 카드 기반
  - `Skeleton`: 로딩 스켈레톤
- [ ] shadcn/ui 컴포넌트 추가
  ```bash
  npx shadcn@latest add badge
  npx shadcn@latest add card
  npx shadcn@latest add skeleton
  ```

### 🤔 왜 이 순서인가?

공통 모듈은 **재사용 가능한 구조**를 만드는 단계입니다. 타입 정의와 데이터 레이어를 먼저 구축하면:
1. 타입 안정성 확보 → 개발 중 오류 조기 발견
2. 데이터 흐름 표준화 → 일관된 데이터 처리
3. 공통 컴포넌트 재사용 → 중복 코드 방지

**의존성 관계**:
- Phase 3의 핵심 기능은 데이터 레이어와 공통 컴포넌트에 의존
- 타입이 없으면 데이터 구조가 불명확하여 버그 발생 위험

### ⏱️ 예상 소요 시간

**4-6시간**

- 타입 정의: 1시간
- 데이터 레이어: 2-3시간
- 공통 컴포넌트: 1-2시간

### ✅ 완료 기준

- [ ] TypeScript 컴파일 에러 0개
- [ ] `getDatabase()` 함수가 Notion 데이터를 올바르게 파싱
- [ ] `groupBooksByMonth()` 함수가 월별로 데이터를 정확히 그룹핑
- [ ] 공통 컴포넌트가 Storybook 또는 별도 페이지에서 정상 렌더링
- [ ] 데이터 가공 함수에 대한 간단한 테스트 코드 작성 (선택 사항)

---

## Phase 3: 핵심 기능 개발 (MVP 구현)

### 📦 작업 내용

#### 3.1 메인 페이지 구현
- [ ] 홈 페이지 라우트 생성 (`app/page.tsx`)
- [ ] Server Component로 데이터 fetching
  ```typescript
  async function HomePage() {
    const books = await getDatabase();
    const groupedBooks = groupBooksByMonth(books);
    // ...
  }
  ```
- [ ] 로딩 상태 (`app/loading.tsx`)
- [ ] 에러 처리 (`app/error.tsx`)

#### 3.2 책 카드 컴포넌트
- [ ] `BookCard` 컴포넌트 개발 (`components/book-card.tsx`)
  - 표지 이미지 (Next.js Image 최적화)
  - 제목, 저자
  - 별점 표시 (⭐ 아이콘)
  - 장르 태그 (Badge 컴포넌트 활용)
  - 한줄평
  - 읽은 날짜
- [ ] 카드 호버 효과 (데스크톱)
- [ ] 카드 클릭 이벤트 준비

#### 3.3 월별 타임라인 UI
- [ ] `TimelineSection` 컴포넌트 개발
  - 월별 헤더 (예: "2026년 1월 (3권)")
  - 그리드 레이아웃 (Responsive)
    - 데스크톱: 3단
    - 태블릿: 2단
    - 모바일: 1단
- [ ] 최신 월부터 역순 정렬
- [ ] 스크롤 성능 최적화

#### 3.4 장르 필터링 기능
- [ ] 필터 UI 컴포넌트 (`components/genre-filter.tsx`)
  - 장르 버튼 그룹
  - 다중 선택 지원
  - "전체 보기" 버튼
  - 선택된 필터 개수 표시
- [ ] 클라이언트 상태 관리
  - React `useState`로 선택된 장르 관리
  - 필터 적용 시 렌더링 업데이트
- [ ] URL 쿼리 파라미터 동기화 (선택 사항)
  ```
  /?genres=소설,기술
  ```
- [ ] 필터 애니메이션 (Fade in/out)

#### 3.5 통계 대시보드
- [ ] `StatsDashboard` 컴포넌트 개발
  - 총 독서량 (전체 책 수)
  - 이번 달 독서량 (현재 월 기준)
  - 선택된 필터에 따른 동적 업데이트
- [ ] 시각적 강조 (아이콘, 색상)

#### 3.6 책 상세 정보
- [ ] 상세 페이지 또는 모달 선택
  - **옵션 A**: Dynamic Route (`app/books/[id]/page.tsx`)
  - **옵션 B**: 모달 (Parallel Routes + Intercepting Routes)
- [ ] 상세 정보 레이아웃
  - 큰 표지 이미지
  - 모든 기본 정보 표시
  - Rich Text 렌더링 (상세 리뷰)
- [ ] 닫기/뒤로가기 버튼

### 🤔 왜 이 순서인가?

이 단계는 **사용자가 실제로 사용하는 핵심 기능**을 구현합니다. MVP의 필수 요구사항을 충족하는 최소한의 기능 세트입니다.

**개발 순서 논리**:
1. **메인 페이지 먼저**: 데이터 흐름의 진입점
2. **책 카드**: 가장 작은 단위의 UI 블록 (재사용)
3. **타임라인**: 카드를 조합하는 상위 컴포넌트
4. **필터**: 기존 UI에 상호작용 추가
5. **통계**: 필터와 연동되는 계산 로직
6. **상세 정보**: 카드 클릭 후 보여줄 확장 기능

**의존성 관계**:
- 타임라인은 책 카드에 의존
- 필터는 타임라인과 상호작용
- 통계는 필터와 데이터 레이어에 의존

### ⏱️ 예상 소요 시간

**12-16시간**

- 메인 페이지: 2시간
- 책 카드: 3-4시간
- 타임라인 UI: 2-3시간
- 필터링: 2-3시간
- 통계: 1-2시간
- 상세 정보: 2-3시간

### ✅ 완료 기준

- [ ] 메인 페이지에 모든 책이 월별로 그룹핑되어 표시됨
- [ ] 책 카드가 디자인대로 정확히 렌더링됨
- [ ] 장르 필터 선택 시 해당 장르의 책만 표시됨
- [ ] 통계가 실시간으로 업데이트됨
- [ ] 책 카드 클릭 시 상세 정보가 올바르게 표시됨
- [ ] Next.js Image 최적화가 적용되어 이미지 로딩이 빠름
- [ ] 모든 기능이 TypeScript 타입 체크를 통과함

---

## Phase 4: 추가 기능 개발 (UX 향상)

### 📦 작업 내용

#### 4.1 로딩 상태 개선
- [ ] 스켈레톤 UI 구현
  - `BookCardSkeleton` 컴포넌트
  - `TimelineSkeleton` 컴포넌트
- [ ] Suspense Boundary 설정
  ```tsx
  <Suspense fallback={<TimelineSkeleton />}>
    <Timeline books={books} />
  </Suspense>
  ```
- [ ] 로딩 애니메이션 (Pulse effect)

#### 4.2 에러 핸들링 강화
- [ ] 전역 에러 바운더리 (`app/error.tsx`)
- [ ] Notion API 에러 처리
  - 네트워크 오류
  - API 키 만료
  - Database 접근 권한 오류
- [ ] 사용자 친화적 에러 메시지
- [ ] 재시도 버튼 제공

#### 4.3 빈 상태 (Empty State) 처리
- [ ] 데이터가 없을 때 표시할 UI
  - "아직 읽은 책이 없습니다" 메시지
  - Notion 데이터베이스 링크 제공
- [ ] 필터 결과가 없을 때
  - "해당 장르의 책이 없습니다" 메시지
  - 필터 초기화 버튼

#### 4.4 접근성 개선
- [ ] 시맨틱 HTML 적용
  - `<article>`, `<section>`, `<time>` 등
- [ ] ARIA 속성 추가
  - `aria-label`, `role` 등
- [ ] 키보드 네비게이션 지원
  - Tab 키로 카드 간 이동
  - Enter/Space로 카드 열기
- [ ] 스크린 리더 테스트

#### 4.5 성능 최적화
- [ ] 이미지 최적화
  - Next.js Image의 `priority`, `loading="lazy"` 활용
  - WebP 포맷 사용
- [ ] 번들 크기 최적화
  - Dynamic Import 적용 (모달 등)
  - Tree Shaking 확인
- [ ] 폰트 최적화
  - `next/font` 활용
  - Font Subsetting

### 🤔 왜 이 순서인가?

핵심 기능이 완성된 후 **사용자 경험을 향상**시키는 단계입니다. 이 단계는 필수는 아니지만, 프로덕션 품질을 위해 권장됩니다.

**왜 Phase 3 이후인가?**:
- 스켈레톤은 실제 UI가 완성된 후 만들어야 정확
- 에러 처리는 정상 흐름이 완성된 후 추가
- 성능 최적화는 측정 가능한 기준선(baseline)이 필요

### ⏱️ 예상 소요 시간

**4-6시간**

- 로딩 상태: 1-2시간
- 에러 핸들링: 1-2시간
- 빈 상태: 1시간
- 접근성: 1시간
- 성능 최적화: 1시간

### ✅ 완료 기준

- [ ] 데이터 로딩 중 스켈레톤이 표시됨
- [ ] Notion API 오류 시 사용자 친화적 에러 메시지 표시
- [ ] 데이터가 없을 때 안내 메시지 표시
- [ ] 키보드만으로 모든 기능 사용 가능
- [ ] Lighthouse Accessibility 점수 90+ 달성
- [ ] 이미지 로딩이 지연 로딩(lazy loading)으로 최적화됨

---

## Phase 5: 최적화 및 배포 (프로덕션 준비)

### 📦 작업 내용

#### 5.1 반응형 디자인 완성
- [ ] 모바일 레이아웃 최적화 (< 768px)
  - 1단 그리드
  - 터치 친화적 UI (버튼 크기 44px+)
  - 모달 대신 전체 페이지로 상세 정보 표시
- [ ] 태블릿 레이아웃 (768px - 1023px)
  - 2단 그리드
  - 필터 상단 고정
- [ ] 데스크톱 레이아웃 (1024px+)
  - 3단 그리드
  - 카드 호버 효과
- [ ] 다양한 디바이스 테스트
  - iOS Safari
  - Android Chrome
  - 데스크톱 Chrome, Firefox, Safari

#### 5.2 SEO 최적화
- [ ] 메타 데이터 설정 (`app/layout.tsx`)
  ```typescript
  export const metadata: Metadata = {
    title: '월별 독서 타임라인',
    description: '나의 독서 여정을 기록합니다',
    // ...
  };
  ```
- [ ] Open Graph 이미지 설정
  - `opengraph-image.tsx` 또는 정적 이미지
- [ ] 구조화된 데이터 (선택 사항)
  - JSON-LD for Books
- [ ] Sitemap 생성 (`app/sitemap.ts`)
- [ ] `robots.txt` 설정

#### 5.3 프로덕션 빌드 테스트
- [ ] 빌드 검증
  ```bash
  npm run build
  npm run start
  ```
- [ ] 빌드 에러 해결
- [ ] 번들 크기 분석
  ```bash
  npm run build -- --analyze
  ```
- [ ] 불필요한 의존성 제거

#### 5.4 성능 측정 및 최적화
- [ ] Lighthouse 테스트 (모바일/데스크톱)
  - Performance: 90+
  - Accessibility: 90+
  - Best Practices: 90+
  - SEO: 90+
- [ ] Core Web Vitals 확인
  - LCP (Largest Contentful Paint) < 2.5초
  - FID (First Input Delay) < 100ms
  - CLS (Cumulative Layout Shift) < 0.1
- [ ] 최적화 적용
  - 필요시 이미지 크기 조정
  - 필요시 폰트 로딩 전략 변경

#### 5.5 환경 변수 및 보안
- [ ] 환경 변수 검증
  - `.env.local`에 모든 필수 변수 존재
  - `.env.example` 파일 생성 (템플릿)
- [ ] API 키 노출 방지 확인
  - 클라이언트 코드에 `NOTION_API_KEY` 노출되지 않음
  - Server Component에서만 사용
- [ ] CORS 설정 (필요시)

#### 5.6 배포
- [ ] Vercel 배포
  - GitHub 연동
  - 환경 변수 설정 (Vercel Dashboard)
  - 자동 배포 설정
- [ ] 배포 후 검증
  - 프로덕션 URL 접속
  - 모든 기능 정상 작동 확인
  - Notion 데이터 정상 로드 확인
- [ ] 도메인 연결 (선택 사항)

#### 5.7 문서화
- [ ] README.md 업데이트
  - 프로젝트 소개
  - 설치 방법
  - 환경 변수 설정
  - 배포 방법
- [ ] 코드 주석 정리
- [ ] 주요 함수/컴포넌트 JSDoc 추가 (선택 사항)

### 🤔 왜 이 순서인가?

마지막 단계는 **프로덕션 환경으로의 전환**입니다. 모든 기능이 완성된 후:
1. 반응형 디자인으로 모든 디바이스 지원 확보
2. SEO로 검색 엔진 노출 준비
3. 성능 최적화로 사용자 경험 극대화
4. 배포로 실제 사용자에게 제공

**왜 마지막인가?**:
- 반응형은 모든 UI가 완성된 후 테스트 가능
- SEO는 콘텐츠 구조가 확정된 후 최적화
- 성능은 전체 앱이 완성된 후 측정 의미 있음
- 배포는 모든 준비가 완료된 후 실행

### ⏱️ 예상 소요 시간

**6-8시간**

- 반응형 디자인: 2-3시간
- SEO: 1시간
- 빌드 테스트: 1시간
- 성능 측정: 1시간
- 배포: 1-2시간
- 문서화: 1시간

### ✅ 완료 기준

- [ ] 모든 화면 크기(모바일/태블릿/데스크톱)에서 UI가 정상 작동
- [ ] `npm run build` 실행 시 에러 없이 빌드 성공
- [ ] Lighthouse 점수 모두 90+ 달성 (모바일/데스크톱)
- [ ] Vercel 프로덕션 배포 성공 및 접속 확인
- [ ] Notion 데이터가 프로덕션 환경에서 정상 로드됨
- [ ] README.md에 설치 및 배포 가이드 작성 완료

---

## 📊 전체 일정 요약

| Phase | 단계 | 예상 시간 | 누적 시간 |
|-------|------|-----------|-----------|
| 1 | 프로젝트 초기 설정 | 2-3시간 | 2-3시간 |
| 2 | 공통 모듈/컴포넌트 | 4-6시간 | 6-9시간 |
| 3 | 핵심 기능 개발 | 12-16시간 | 18-25시간 |
| 4 | 추가 기능 개발 | 4-6시간 | 22-31시간 |
| 5 | 최적화 및 배포 | 6-8시간 | **28-39시간** |

**총 예상 시간**: 28-39시간 (약 4-5일, 하루 7-8시간 작업 기준)

---

## 🎯 핵심 체크포인트

각 Phase 완료 시 반드시 확인할 사항:

### Phase 1 체크포인트
```bash
# Notion API 테스트
node -e "
const { Client } = require('@notionhq/client');
const notion = new Client({ auth: process.env.NOTION_API_KEY });
notion.databases.query({ database_id: process.env.NOTION_DATABASE_ID })
  .then(() => console.log('✅ Notion 연결 성공'))
  .catch((e) => console.error('❌ 연결 실패:', e.message));
"
```

### Phase 2 체크포인트
```bash
# TypeScript 컴파일 에러 확인
npm run type-check  # 또는 npx tsc --noEmit

# 데이터 레이어 테스트
npm run dev  # 개발 서버에서 데이터 로드 확인
```

### Phase 3 체크포인트
- [ ] 메인 페이지 방문 → 책 목록 표시 확인
- [ ] 장르 필터 클릭 → 필터링 작동 확인
- [ ] 책 카드 클릭 → 상세 정보 표시 확인

### Phase 4 체크포인트
```bash
# Lighthouse 테스트 (Chrome DevTools)
# 1. Chrome DevTools 열기 (F12)
# 2. Lighthouse 탭 클릭
# 3. "Analyze page load" 실행
```

### Phase 5 체크포인트
```bash
# 프로덕션 빌드 테스트
npm run build
npm run start

# 빌드 성공 확인
✓ Compiled successfully
✓ Collecting page data
✓ Generating static pages
```

---

## 🚨 주의사항

### 순서를 지켜야 하는 이유

1. **Phase 1 → Phase 2**: 데이터 소스 없이 데이터 레이어를 만들 수 없음
2. **Phase 2 → Phase 3**: 타입과 공통 컴포넌트 없이 핵심 기능을 구현하면 리팩토링 필요
3. **Phase 3 → Phase 4**: 정상 흐름이 완성되지 않은 상태에서 에러 처리는 불완전
4. **Phase 4 → Phase 5**: 최적화는 측정 가능한 기준선이 필요하며, 배포 전 반응형 완성 필수

### 건너뛰면 안 되는 작업

- [ ] TypeScript 타입 정의 (Phase 2)
- [ ] 데이터 레이어 구현 (Phase 2)
- [ ] 에러 핸들링 (Phase 4)
- [ ] 반응형 디자인 (Phase 5)
- [ ] 프로덕션 빌드 테스트 (Phase 5)

### 선택 사항 (시간 부족 시 생략 가능)

- 접근성 개선 (Phase 4) - 기본적인 시맨틱 HTML은 유지
- 구조화된 데이터 (Phase 5) - SEO에 추가 도움이지만 필수는 아님
- JSDoc 주석 (Phase 5) - TypeScript로 타입이 명확하면 생략 가능

---

## 🔮 Post-MVP 확장 계획

MVP 완료 후 추가할 수 있는 기능들 (우선순위 순):

### Phase 6: 검색 및 정렬 (4-6시간)
- [ ] 책 제목/저자 검색 기능
- [ ] 정렬 옵션 (별점순, 날짜순, 제목순)
- [ ] 검색 결과 하이라이팅

### Phase 7: 다크 모드 (2-3시간)
- [ ] 다크 모드 테마 추가
- [ ] 테마 토글 버튼
- [ ] 시스템 설정 자동 감지

### Phase 8: 소셜 공유 (3-4시간)
- [ ] Open Graph 메타 태그 동적 생성
- [ ] Twitter Card 지원
- [ ] 공유 버튼 (Facebook, Twitter, 링크 복사)

### Phase 9: 고급 통계 (4-6시간)
- [ ] 월별/연도별 독서 추이 차트
- [ ] 장르별 분포 차트
- [ ] 평균 별점, 최다 독서 월 등

---

## 📝 변경 이력

| 날짜 | 버전 | 변경 내용 | 작성자 |
|------|------|-----------|--------|
| 2026-01-20 | 1.0.0 | 초기 로드맵 작성 | - |

---

## 🆘 도움이 필요한 경우

- **Notion API 문제**: [Notion API 문서](https://developers.notion.com/)
- **Next.js 문제**: [Next.js 문서](https://nextjs.org/docs)
- **shadcn/ui 문제**: [shadcn/ui 문서](https://ui.shadcn.com/)
- **배포 문제**: [Vercel 문서](https://vercel.com/docs)

---

**💡 Tip**: 각 Phase 완료 시마다 Git 커밋을 만들어 진행 상황을 추적하세요!

```bash
git commit -m "✅ Phase 1 완료: 프로젝트 초기 설정"
git commit -m "✅ Phase 2 완료: 공통 모듈/컴포넌트 개발"
# ...
```
