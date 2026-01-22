# 개발 가이드라인

> **이 문서는 AI Agent 전용입니다.** 코드 작성 시 반드시 준수해야 하는 프로젝트 특화 규칙을 정의합니다.

## 프로젝트 개요

### 핵심 정보

- **프로젝트명**: 월별 독서 타임라인 (Monthly Reading Timeline)
- **목적**: Notion을 CMS로 활용한 개인 독서 기록 및 리뷰 공유 웹사이트
- **버전**: 1.0.0 (MVP)
- **현재 상태**: 스타터 템플릿 (독서 타임라인 기능 미구현)

### 기술 스택

- **Framework**: Next.js 15.5.3 (App Router + Turbopack)
- **Runtime**: React 19.1.0
- **Language**: TypeScript 5 (strict mode)
- **Styling**: TailwindCSS v4 + shadcn/ui (new-york style, RSC 지원)
- **CMS**: Notion API (@notionhq/client)
- **Forms**: React Hook Form + Zod
- **Icons**: Lucide React

### 프로젝트 목표

PRD(@docs/PRD.md)에 정의된 "월별 독서 타임라인" 기능을 구현해야 한다. 기존 스타터 템플릿의 랜딩 페이지 컴포넌트(hero, features, cta)는 제거하고, 독서 기록 UI로 대체해야 한다.

---

## 프로젝트 아키텍처

### 디렉토리 구조

```
src/
├── app/                  # Next.js App Router
│   ├── layout.tsx       # 루트 레이아웃
│   ├── page.tsx         # 홈페이지 (독서 타임라인)
│   ├── books/[id]/      # 책 상세 페이지 (향후 구현)
│   ├── loading.tsx      # 로딩 UI
│   └── error.tsx        # 에러 UI
├── components/
│   ├── ui/              # shadcn/ui 기본 컴포넌트
│   ├── layout/          # 레이아웃 컴포넌트
│   ├── book-card.tsx    # 책 카드 컴포넌트 (생성 필요)
│   ├── timeline-section.tsx  # 타임라인 섹션 (생성 필요)
│   ├── genre-filter.tsx      # 장르 필터 (생성 필요)
│   └── stats-dashboard.tsx   # 통계 대시보드 (생성 필요)
└── lib/
    ├── notion.ts        # Notion API 클라이언트 (생성 필요)
    ├── types/
    │   └── book.ts      # 책 데이터 타입 (생성 필요)
    └── utils.ts         # 공통 유틸리티
```

### 모듈 책임

**반드시 다음 위치에 파일을 생성하라:**

| 기능 | 파일 경로 | 필수 여부 |
|-----|-----------|----------|
| Notion API 클라이언트 | `src/lib/notion.ts` | 필수 |
| 책 데이터 타입 | `src/lib/types/book.ts` | 필수 |
| 책 카드 컴포넌트 | `src/components/book-card.tsx` | 필수 |
| 타임라인 섹션 | `src/components/timeline-section.tsx` | 필수 |
| 장르 필터 | `src/components/genre-filter.tsx` | 필수 |
| 통계 대시보드 | `src/components/stats-dashboard.tsx` | 필수 |
| 데이터 가공 함수 | `src/lib/book-utils.ts` | 필수 |

---

## 코드 표준

### 네이밍 규칙

#### 파일명
- **파일명**: kebab-case 사용 (예: `book-card.tsx`, `genre-filter.tsx`)
- **컴포넌트 파일**: kebab-case 또는 PascalCase 허용
- **유틸리티 파일**: kebab-case 사용 (예: `book-utils.ts`)

#### 컴포넌트명
- **컴포넌트**: PascalCase 사용 (예: `BookCard`, `TimelineSection`)
- **함수/변수**: camelCase 사용 (예: `groupBooksByMonth`, `filterByGenre`)
- **타입/인터페이스**: PascalCase 사용 (예: `Book`, `NotionResponse`)
- **상수**: UPPER_SNAKE_CASE 사용 (예: `NOTION_API_KEY`)

#### ❌ 금지된 네이밍
- `snake_case` 파일명 금지
- `userProfile` 같은 camelCase 컴포넌트명 금지
- `Components/`, `UserSettings/` 같은 PascalCase 폴더명 금지

### 주석 및 문서화

**모든 주석과 문서는 한국어로 작성하라:**

```typescript
// ✅ 올바른 예
/**
 * Notion API에서 책 데이터를 가져옵니다
 * @param databaseId - Notion 데이터베이스 ID
 * @returns 책 목록
 */
export async function getBooks(databaseId: string): Promise<Book[]> {
  // Notion 클라이언트 초기화
  const notion = new Client({ auth: process.env.NOTION_API_KEY })
  // ...
}

// ❌ 잘못된 예 (영어 주석)
/**
 * Fetches book data from Notion API
 */
export async function getBooks(databaseId: string): Promise<Book[]> {
  // Initialize Notion client
}
```

**변수명과 함수명은 영어로 작성하라:**

```typescript
// ✅ 올바른 예
const bookList = await getBooks()
const filteredBooks = filterByGenre(books, selectedGenres)

// ❌ 잘못된 예 (한글 변수명)
const 책목록 = await getBooks()
const 필터된책 = filterByGenre(books, selectedGenres)
```

### 포맷팅

- **들여쓰기**: 2칸 (스페이스)
- **줄 끝**: LF (Unix 스타일)
- **최대 줄 길이**: 제한 없음 (Prettier 자동 포맷 사용)
- **세미콜론**: 미사용 (Prettier 설정)

### Import 순서

```typescript
// 1. 외부 라이브러리
import React from 'react'
import { Client } from '@notionhq/client'

// 2. 내부 라이브러리 (@/ 경로)
import { Button } from '@/components/ui/button'
import { cn } from '@/lib/utils'
import type { Book } from '@/lib/types/book'

// 3. 상대 경로
import './styles.css'
```

### Export 규칙

```typescript
// ✅ Named export 사용 (일반 컴포넌트/함수)
export function BookCard({ book }: BookCardProps) { }
export async function getBooks() { }

// ✅ Default export 사용 (페이지 컴포넌트만)
export default async function HomePage() { }

// ❌ 혼재 사용 금지
export function BookCard() { }
export default BookCard
```

---

## Next.js 15.5.3 특화 규칙

### Server Components 우선

**모든 컴포넌트는 기본적으로 Server Component로 작성하라:**

```typescript
// ✅ 올바른 예: Server Component
export default async function HomePage() {
  // 서버에서 직접 데이터 패칭
  const books = await getBooks()
  const groupedBooks = groupBooksByMonth(books)

  return (
    <div>
      <StatsDashboard books={books} />
      <TimelineSection groupedBooks={groupedBooks} />
    </div>
  )
}
```

**상호작용이 필요한 경우에만 'use client' 사용:**

```typescript
// ✅ 올바른 예: Client Component (최소화)
'use client'

import { useState } from 'react'

export function GenreFilter({ genres }: { genres: string[] }) {
  const [selected, setSelected] = useState<string[]>([])
  // 클라이언트 상태 관리 로직만
}
```

### async Request APIs

**params와 searchParams는 반드시 await 하라:**

```typescript
// ✅ 올바른 예: Next.js 15.5.3 방식
export default async function BookDetailPage({
  params,
  searchParams
}: {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | undefined }>
}) {
  const { id } = await params
  const query = await searchParams
  const book = await getBook(id)

  return <BookDetail book={book} />
}

// ❌ 금지: 동기식 접근
export default function BookDetailPage({ params }: { params: { id: string } }) {
  const book = getBook(params.id) // 에러 발생
}
```

### 경로 별칭 사용 필수

**항상 @/ 경로 별칭을 사용하라. 상대 경로 금지:**

```typescript
// ✅ 올바른 예
import { Button } from '@/components/ui/button'
import { getBooks } from '@/lib/notion'
import { cn } from '@/lib/utils'

// ❌ 금지: 상대 경로
import { Button } from '../../../components/ui/button'
import { getBooks } from '../../lib/notion'
```

### Turbopack 최적화

**빌드 및 개발 서버 실행 시 --turbopack 플래그 사용:**

```bash
# ✅ 올바른 예
npm run dev    # 이미 --turbopack 설정됨
npm run build  # 이미 --turbopack 설정됨

# ❌ 금지: 플래그 제거
next dev       # --turbopack 없이 실행 금지
```

---

## 컴포넌트 구현 표준

### 컴포넌트 위치 결정

| 컴포넌트 타입 | 위치 | 조건 |
|------------|------|------|
| shadcn/ui 기본 컴포넌트 | `src/components/ui/` | shadcn CLI로 추가 |
| 독서 기록 전용 컴포넌트 | `src/components/` (루트) | 프로젝트 특화 컴포넌트 |
| 레이아웃 컴포넌트 | `src/components/layout/` | header, footer, container |
| 페이지 전용 컴포넌트 | `src/app/[page]/components/` | 해당 페이지에서만 사용 |

### 컴포넌트 크기 제한

- **최대 줄 수**: 300줄
- **300줄 초과 시**: 하위 컴포넌트로 분할

### Props 타입 정의

**모든 Props는 interface로 정의하라:**

```typescript
// ✅ 올바른 예
interface BookCardProps {
  book: Book
  onClick?: () => void
  className?: string
}

export function BookCard({ book, onClick, className }: BookCardProps) {
  return (
    <Card className={cn("book-card", className)} onClick={onClick}>
      <img src={book.coverImage} alt={book.title} />
      <h3>{book.title}</h3>
      <p>{book.author}</p>
    </Card>
  )
}

// ❌ 금지: 인라인 타입
export function BookCard({ book, onClick }: {
  book: Book
  onClick?: () => void
}) {
  // ...
}
```

### 'use client' 최소화

**다음 경우에만 'use client' 사용:**

1. useState, useEffect 등 React Hook 사용
2. onClick, onChange 등 이벤트 핸들러 사용
3. 브라우저 API (localStorage, window) 사용
4. Context Provider 사용

**서버에서 처리 가능한 로직은 Server Component에 유지:**

```typescript
// ✅ 올바른 예: 서버에서 데이터 가공
export default async function HomePage() {
  const books = await getBooks()
  const stats = calculateStats(books) // 서버에서 계산

  return (
    <div>
      {/* 서버 컴포넌트 */}
      <StatsDashboard stats={stats} />

      {/* 클라이언트 컴포넌트 (필터링만) */}
      <GenreFilter genres={getAllGenres(books)} />
    </div>
  )
}

// ❌ 금지: 불필요한 클라이언트 컴포넌트
'use client'

export function HomePage() {
  const [books, setBooks] = useState([])

  useEffect(() => {
    fetchBooks().then(setBooks) // 서버에서 해야 할 일을 클라이언트에서
  }, [])
}
```

---

## Notion API 통합 규칙

### 환경 변수 보안

**환경 변수는 반드시 서버 전용으로 사용하라:**

```typescript
// ✅ 올바른 예: Server Component에서만 사용
// src/lib/notion.ts
import { Client } from '@notionhq/client'

export function getNotionClient() {
  return new Client({
    auth: process.env.NOTION_API_KEY // 서버에서만 접근 가능
  })
}

// ❌ 금지: 클라이언트 컴포넌트에서 접근
'use client'

export function BookList() {
  const apiKey = process.env.NOTION_API_KEY // 보안 위험!
}
```

**필수 환경 변수:**
- `NOTION_API_KEY`: Notion Integration API Key
- `NOTION_DATABASE_ID`: 독서 기록 데이터베이스 ID

### Notion 데이터 구조

**Notion 데이터베이스는 다음 속성을 포함한다:**

| 속성명 | Notion 타입 | TypeScript 타입 | 필수 여부 |
|-------|------------|----------------|----------|
| 책 제목 | Title | string | 필수 |
| 저자 | Text | string | 필수 |
| 표지 이미지 | Files & media | string (URL) | 필수 |
| 읽은 날짜 | Date | string (ISO 8601) | 필수 |
| 별점 | Select | 1-5 | 필수 |
| 장르 | Multi-select | string[] | 필수 |
| 한줄평 | Text | string | 선택 |
| 상세 리뷰 | Rich text | string | 선택 |
| 상태 | Select | 'completed' \| 'reading' \| 'paused' | 선택 |

**src/lib/types/book.ts에 다음 타입을 정의하라:**

```typescript
export interface Book {
  id: string
  title: string
  author: string
  coverImage: string
  finishedDate: string // ISO 8601 format
  rating: 1 | 2 | 3 | 4 | 5
  genres: string[]
  oneLiner?: string
  review?: string
  status?: 'completed' | 'reading' | 'paused'
}
```

### Notion API 함수 구현

**src/lib/notion.ts에 다음 함수들을 구현하라:**

```typescript
// Notion 클라이언트 초기화
export function getNotionClient(): Client

// 전체 책 목록 조회 (완독 상태만)
export async function getBooks(): Promise<Book[]>

// Notion 응답을 Book 타입으로 변환
export function parseNotionBook(page: any): Book

// 특정 책 상세 조회 (향후 구현)
export async function getBook(id: string): Promise<Book>
```

### 데이터 캐싱

**ISR (Incremental Static Regeneration) 사용:**

```typescript
// ✅ 올바른 예: 1시간마다 재검증
export const revalidate = 3600 // 초 단위

export default async function HomePage() {
  const books = await getBooks()
  return <Timeline books={books} />
}
```

---

## 기능별 구현 표준

### 월별 타임라인

**src/lib/book-utils.ts에 월별 그룹핑 함수 작성:**

```typescript
export interface GroupedBooks {
  yearMonth: string // 'YYYY-MM' 형식
  label: string // '2026년 1월'
  books: Book[]
  count: number
}

/**
 * 책 목록을 월별로 그룹핑합니다
 * @param books - 책 목록
 * @returns 월별로 그룹핑된 책 목록 (최신순)
 */
export function groupBooksByMonth(books: Book[]): GroupedBooks[]
```

**src/components/timeline-section.tsx 컴포넌트 구현:**

```typescript
interface TimelineSectionProps {
  yearMonth: string
  label: string
  books: Book[]
  count: number
}

export function TimelineSection({ yearMonth, label, books, count }: TimelineSectionProps) {
  return (
    <section className="timeline-section">
      <h2>{label} ({count}권)</h2>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {books.map(book => (
          <BookCard key={book.id} book={book} />
        ))}
      </div>
    </section>
  )
}
```

### 책 카드

**src/components/book-card.tsx 구현:**

```typescript
interface BookCardProps {
  book: Book
  onClick?: () => void
  className?: string
}

export function BookCard({ book, onClick, className }: BookCardProps) {
  return (
    <Card className={cn("book-card hover:shadow-lg transition", className)} onClick={onClick}>
      {/* 표지 이미지 */}
      <Image src={book.coverImage} alt={book.title} width={200} height={300} />

      {/* 제목 */}
      <h3 className="text-lg font-bold">{book.title}</h3>

      {/* 저자 */}
      <p className="text-muted-foreground">{book.author}</p>

      {/* 별점 */}
      <div className="flex items-center gap-1">
        {Array.from({ length: book.rating }).map((_, i) => (
          <Star key={i} className="w-4 h-4 fill-yellow-400 text-yellow-400" />
        ))}
      </div>

      {/* 장르 태그 */}
      <div className="flex flex-wrap gap-2">
        {book.genres.map(genre => (
          <Badge key={genre} variant="secondary">{genre}</Badge>
        ))}
      </div>

      {/* 한줄평 */}
      {book.oneLiner && (
        <p className="text-sm text-muted-foreground italic">{book.oneLiner}</p>
      )}

      {/* 읽은 날짜 */}
      <time className="text-xs text-muted-foreground">
        {new Date(book.finishedDate).toLocaleDateString('ko-KR')}
      </time>
    </Card>
  )
}
```

### 장르 필터

**src/components/genre-filter.tsx 구현 (Client Component):**

```typescript
'use client'

import { useState } from 'react'
import { Badge } from '@/components/ui/badge'

interface GenreFilterProps {
  genres: string[]
  onFilterChange: (selected: string[]) => void
}

export function GenreFilter({ genres, onFilterChange }: GenreFilterProps) {
  const [selected, setSelected] = useState<string[]>([])

  const toggle = (genre: string) => {
    const newSelected = selected.includes(genre)
      ? selected.filter(g => g !== genre)
      : [...selected, genre]

    setSelected(newSelected)
    onFilterChange(newSelected)
  }

  return (
    <div className="flex flex-wrap gap-2">
      <Badge
        variant={selected.length === 0 ? 'default' : 'outline'}
        className="cursor-pointer"
        onClick={() => {
          setSelected([])
          onFilterChange([])
        }}
      >
        전체 ({genres.length})
      </Badge>

      {genres.map(genre => (
        <Badge
          key={genre}
          variant={selected.includes(genre) ? 'default' : 'outline'}
          className="cursor-pointer"
          onClick={() => toggle(genre)}
        >
          {genre}
        </Badge>
      ))}
    </div>
  )
}
```

### 통계 대시보드

**src/components/stats-dashboard.tsx 구현:**

```typescript
interface StatsDashboardProps {
  totalCount: number
  thisMonthCount: number
}

export function StatsDashboard({ totalCount, thisMonthCount }: StatsDashboardProps) {
  return (
    <div className="grid grid-cols-2 gap-4 mb-8">
      <Card className="p-6">
        <BookOpen className="w-8 h-8 mb-2" />
        <h3 className="text-2xl font-bold">{totalCount}권</h3>
        <p className="text-muted-foreground">총 독서량</p>
      </Card>

      <Card className="p-6">
        <Calendar className="w-8 h-8 mb-2" />
        <h3 className="text-2xl font-bold">{thisMonthCount}권</h3>
        <p className="text-muted-foreground">이번 달</p>
      </Card>
    </div>
  )
}
```

---

## 스타일링 규칙

### TailwindCSS v4 사용

**유틸리티 클래스 우선 사용:**

```typescript
// ✅ 올바른 예
<div className="flex items-center justify-between p-4 bg-card rounded-lg border">
  <h2 className="text-2xl font-bold">제목</h2>
</div>

// ❌ 금지: 인라인 스타일
<div style={{ display: 'flex', padding: '16px' }}>
  <h2 style={{ fontSize: '24px' }}>제목</h2>
</div>
```

### shadcn/ui 컴포넌트 추가

**새 UI 컴포넌트는 shadcn CLI로 추가:**

```bash
# ✅ 올바른 예
npx shadcn@latest add badge
npx shadcn@latest add card
npx shadcn@latest add dialog

# ❌ 금지: 수동 복사
```

### cn() 유틸리티 사용

**클래스 병합 시 cn() 사용:**

```typescript
import { cn } from '@/lib/utils'

// ✅ 올바른 예
<Card className={cn("base-styles", isPrimary && "primary-styles", className)}>

// ❌ 금지: 문자열 연결
<Card className={`base-styles ${isPrimary ? 'primary-styles' : ''} ${className}`}>
```

---

## 다중 파일 조정 규칙

### 문서 일관성 유지

**다음 파일들을 함께 업데이트해야 하는 경우:**

| 주 파일 | 연관 파일 | 업데이트 규칙 |
|--------|----------|-------------|
| `CLAUDE.md` | `README.md` | 프로젝트 설명 변경 시 README도 업데이트 |
| `docs/PRD.md` | `docs/ROADMAP.md` | 요구사항 변경 시 로드맵도 수정 |
| `package.json` | `README.md` | 의존성 추가 시 README 설치 가이드 업데이트 |
| `components.json` | `src/components/ui/*` | shadcn 설정 변경 시 UI 컴포넌트 영향 확인 |

### 타입 정의 업데이트

**src/lib/types/book.ts 수정 시 다음 파일도 확인:**

1. `src/lib/notion.ts` - parseNotionBook 함수
2. `src/components/book-card.tsx` - Props 타입
3. `src/lib/book-utils.ts` - 유틸리티 함수 타입

### 환경 변수 추가

**새 환경 변수 추가 시:**

1. `.env.local` 파일에 추가
2. `.env.example` 파일에 템플릿 추가
3. `README.md`의 "환경 설정" 섹션에 문서화

---

## 개발 워크플로우 표준

### Phase 기반 개발

**반드시 다음 순서로 개발하라 (@docs/ROADMAP.md 참조):**

#### Phase 1: Notion 연동 (필수 선행)
1. Notion Integration 생성 및 API Key 발급
2. 환경 변수 설정 (.env.local)
3. `src/lib/notion.ts` 작성
4. `src/lib/types/book.ts` 작성
5. Notion 데이터 조회 테스트

#### Phase 2: 공통 모듈 (필수 선행)
1. `src/lib/book-utils.ts` 작성 (groupBooksByMonth 등)
2. shadcn/ui 컴포넌트 추가 (badge, card, skeleton)
3. 레이아웃 컴포넌트 (header, footer, container)

#### Phase 3: 핵심 기능
1. `src/app/page.tsx` - 홈페이지 구현
2. `src/components/book-card.tsx` - 책 카드
3. `src/components/timeline-section.tsx` - 타임라인
4. `src/components/genre-filter.tsx` - 필터
5. `src/components/stats-dashboard.tsx` - 통계

#### Phase 4: UX 향상
1. 로딩 스켈레톤 (src/app/loading.tsx)
2. 에러 처리 (src/app/error.tsx)
3. 빈 상태 처리
4. 접근성 개선

#### Phase 5: 배포 준비
1. 반응형 디자인 테스트
2. SEO 메타 데이터
3. 프로덕션 빌드 테스트
4. 환경 변수 검증

### 검증 명령어

**각 단계 완료 후 다음 명령어 실행:**

```bash
# 타입 체크
npm run typecheck

# 린트 체크
npm run lint

# 포맷 체크
npm run format:check

# 통합 검증
npm run check-all

# 빌드 테스트
npm run build
```

---

## 금지 사항

### ❌ 절대 하지 말아야 할 것

#### 1. Pages Router 사용 금지

```typescript
// ❌ 금지
pages/index.tsx
pages/api/books.ts

// ✅ 사용
src/app/page.tsx
src/app/api/books/route.ts
```

#### 2. 클라이언트에서 환경 변수 접근 금지

```typescript
// ❌ 금지
'use client'

export function Component() {
  const apiKey = process.env.NOTION_API_KEY // 보안 위험!
}

// ✅ 사용
export async function getData() {
  const apiKey = process.env.NOTION_API_KEY // 서버에서만
}
```

#### 3. 동기식 params/searchParams 접근 금지

```typescript
// ❌ 금지
export default function Page({ params }: { params: { id: string } }) {
  const id = params.id // Next.js 15.5.3에서 에러
}

// ✅ 사용
export default async function Page({
  params
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
}
```

#### 4. 상대 경로 import 금지

```typescript
// ❌ 금지
import { Button } from '../../../components/ui/button'

// ✅ 사용
import { Button } from '@/components/ui/button'
```

#### 5. 불필요한 'use client' 사용 금지

```typescript
// ❌ 금지: 서버에서 처리 가능
'use client'

export function BookList({ books }: { books: Book[] }) {
  return books.map(book => <BookCard key={book.id} book={book} />)
}

// ✅ 사용: Server Component
export function BookList({ books }: { books: Book[] }) {
  return books.map(book => <BookCard key={book.id} book={book} />)
}
```

#### 6. snake_case 파일명 금지

```bash
# ❌ 금지
book_card.tsx
genre_filter.tsx

# ✅ 사용
book-card.tsx
genre-filter.tsx
```

#### 7. 영어 주석 금지

```typescript
// ❌ 금지
// Fetch books from Notion
const books = await getBooks()

// ✅ 사용
// Notion에서 책 목록을 가져옵니다
const books = await getBooks()
```

#### 8. 거대한 컴포넌트 금지

```typescript
// ❌ 금지: 500줄 이상의 컴포넌트
export function GiantComponent() {
  // 너무 많은 로직과 JSX
}

// ✅ 사용: 하위 컴포넌트로 분할
export function HomePage() {
  return (
    <>
      <Header />
      <Timeline />
      <Footer />
    </>
  )
}
```

#### 9. 일반 지식 문서화 금지

```markdown
<!-- ❌ 금지: 일반 React 지식 -->
React는 UI 라이브러리입니다. useState로 상태를 관리할 수 있습니다.

<!-- ✅ 사용: 프로젝트 특화 규칙 -->
이 프로젝트에서 상태 관리가 필요한 컴포넌트는 GenreFilter입니다.
useState를 사용하여 선택된 장르 목록을 관리하세요.
```

#### 10. 기존 스타터 컴포넌트 유지 금지

**다음 컴포넌트는 독서 타임라인에 불필요하므로 제거하라:**
- `src/components/sections/hero.tsx`
- `src/components/sections/features.tsx`
- `src/components/sections/cta.tsx`
- `src/app/login/page.tsx` (독서 기록은 공개 사이트)
- `src/app/signup/page.tsx`

---

## AI 의사결정 기준

### 우선순위 판단

**충돌하는 요구사항이 있을 경우 다음 우선순위를 따르라:**

1. **보안** > 기능 > 성능 > 편의성
2. **Server Component** > Client Component
3. **@/ 경로** > 상대 경로
4. **TypeScript 타입 안전성** > 동적 타입
5. **shadcn/ui 컴포넌트** > 커스텀 컴포넌트

### 예제 결정 트리

```
새 기능 구현 필요
│
├─ 데이터 패칭 필요? → YES → Server Component 사용
│                    → NO ↓
│
├─ 상호작용 필요? → YES → 'use client' 추가
│                 → NO → Server Component 유지
│
├─ UI 컴포넌트 필요? → YES → shadcn/ui에 있는지 확인
│                            ├─ YES → npx shadcn add [component]
│                            └─ NO → 커스텀 컴포넌트 작성
│
└─ 파일 위치 결정
   ├─ UI 컴포넌트 → src/components/ui/
   ├─ 독서 기록 컴포넌트 → src/components/
   ├─ 유틸리티 → src/lib/
   └─ 타입 → src/lib/types/
```

---

## 참고 문서

**더 자세한 가이드는 다음 문서를 참조하라:**

- `@/docs/PRD.md` - 제품 요구사항 정의
- `@/docs/ROADMAP.md` - 개발 로드맵 및 Phase별 작업
- `@/docs/guides/project-structure.md` - 상세 프로젝트 구조
- `@/docs/guides/component-patterns.md` - 컴포넌트 패턴
- `@/docs/guides/nextjs-15.md` - Next.js 15.5.3 전문 가이드
- `@/docs/guides/forms-react-hook-form.md` - 폼 처리 가이드 (필요시)

---

**이 문서는 AI Agent가 코드를 작성할 때 반드시 따라야 하는 규칙을 정의합니다.**
**일반적인 개발 지식이 아닌, 이 프로젝트만의 특화된 규칙에 집중하세요.**
