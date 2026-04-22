---
name: web-slide-presenter
description: "순수 HTML, CSS, JavaScript만으로 웹 기반 슬라이드 프레젠테이션을 생성하는 스킬. 외부 라이브러리 없이 바닐라 JS로 고품질 프레젠테이션을 만든다. 다음 상황에서 반드시 이 스킬을 사용할 것: 사용자가 'PT', 'PPT', '슬라이드', '프레젠테이션', '발표자료', 'slide', 'presentation', 'deck'을 언급하면서 HTML/웹 기반 결과물을 원할 때. '슬라이드 HTML로 만들어줘', '웹 프레젠테이션', '브라우저에서 보는 발표자료' 등의 요청에도 트리거한다. .pptx 파일이 아닌 웹 기반 슬라이드를 원하는 모든 경우에 사용."
---

# Web Slide Presenter

순수 HTML + CSS + JS로 브라우저 전체화면 프레젠테이션을 만드는 스킬.
유튜브 녹화, 수업 발표 등 **모니터 전체화면**에서 사용하는 것이 전제.

이 스킬은 구현 프레임워크를 강제하지 않는다. 구현 방식은 자유롭게 선택하되, 아래의 필수 제약과 디자인 원칙을 따른다.

---

## 필수 제약 (이것만 지켜라)

### 1. 전체화면, 스크롤 없음
- 모든 슬라이드는 `100vw × 100vh` 안에 완결
- `overflow: hidden` 필수. 스크롤바가 보이면 안 됨
- **슬라이드 콘텐츠는 화면 중앙 정렬**이 기본:
```css
.slide { display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 5vh 8vw; }
```
- 콘텐츠가 넘치면 **슬라이드를 쪼개라** (폰트를 줄이지 마라 — 가독성이 최우선)
- **vw와 vh를 섞어서 쓸 때 주의**: 16:9 화면에서 `1vw ≈ 1.78vh`이다. 같은 요소에 width를 vw로, height를 vh로 제한하면 비율이 깨진다. 특히 원(circle), 정사각형 등 **가로세로 비율이 중요한 요소**는 vw 또는 vh 중 **한 단위만** 써라. `max-width: Xvh`처럼 다른 단위로 제한을 거는 것도 같은 문제를 일으킨다.

### 2. 순수 바닐라 JS
- 외부 라이브러리, CDN 금지 (웹폰트 CDN은 허용)
- Reveal.js 등 프레젠테이션 라이브러리 사용 금지

### 3. 키보드 네비게이션
- 최소: →/↓/Space = 다음, ←/↑ = 이전
- `keydown` 이벤트, `e.preventDefault()`로 브라우저 기본 동작 차단
- 입력 필드(input, textarea)에서는 키보드 네비게이션 비활성화

### 4. 단일 HTML 파일
- HTML + CSS + JS를 하나의 파일에 담는다
- 이유: 공유가 쉽고, 파일 하나만 열면 바로 작동한다
- CSS는 `<style>`, JS는 `<script>` 태그로 인라인

---

## AI스러운 디자인을 피하는 법

Claude가 프레젠테이션을 만들 때 빠지기 쉬운 함정이 있다. 아래 패턴을 의식적으로 피해야 결과물이 "사람이 만든 것 같은" 자연스러운 느낌을 갖는다.

### 금지 패턴

| 패턴 | 왜 안 되는가 | 대안 |
|------|-------------|------|
| 블랙 배경 + 퍼플/그린/블루 네온 강조 | AI가 만든 모든 슬라이드가 이렇게 생김. 즉시 AI 티가 남 | 아래 색상 팔레트 참고 |
| 이모지/이모티콘 남발 (🚀💡🔥📊) | 유치하고 아마추어해 보임. 진지한 발표에 부적합 | 텍스트, SVG 아이콘, 또는 아이콘 없이 디자인 |
| 모든 카드에 그라데이션 보더 | 하나도 강조 안 됨. 전부 반짝이면 아무것도 안 반짝이는 것 | 강조는 1-2개만. 나머지는 절제 |
| 모든 슬라이드 동일 레이아웃 | 지루함. "제목 + 3카드" 반복 | 슬라이드마다 다른 구성 |
| CSS 변수만으로 전체 테마 통일 | 깔끔해 보이지만 단조로움 | 슬라이드별 미세한 배경/톤 변화 |
| 둥근 모서리 카드 나열 | AI 프레젠테이션의 대표적 클리셰 | 다양한 레이아웃: 풀블리드, 비대칭, 타이포그래피 중심 |

### 색상: 자연스러운 팔레트 선택법

나쁜 예 (AI 클리셰):
```
배경: #0a0a1a (거의 검정)
강조: #6c5ce7 (보라), #00cec9 (청록), #e94560 (핑크)
```

좋은 예 — 주제에 맞춰 팔레트를 선택:

**비즈니스/투자 피치**: 
```
배경: #f8f9fa 또는 #ffffff (라이트) / #1a1b23 (다크)
메인: #1a56db (신뢰의 파랑)
보조: #111827 (텍스트), #6b7280 (서브텍스트)
강조: 하나만. #e63946 (긴급) 또는 #059669 (성장)
```

**교육/튜토리얼**:
```
배경: #fefefe
메인: #1e293b (진한 남색)
코드배경: #f1f5f9
강조: #0ea5e9 (시원한 블루) — 하이라이트 용도로만
```

**크리에이티브/디자인**:
```
배경: #fffbf0 (따뜻한 화이트)
메인: #1c1917
강조: #d97706 (앰버) 또는 주제에 맞는 단일 색상
```

**원칙**: 강조색은 **하나**. 2개 이상 쓰면 산만해진다. 배경은 순수 블랙(#000)을 피하고, 아주 약간 색감이 있는 어두운 색이나 밝은 색을 쓴다.

### 타이포그래피 — 교실 뒷자리에서도 읽혀야 한다

이 프레젠테이션은 모니터/빔프로젝터 전체화면으로 교실이나 강의실에서 사용된다. 교실 맨 뒤 학생도 글자를 읽을 수 있어야 한다. **글자가 작은 것보다 촌스럽게 큰 게 100배 낫다.**

**최소 폰트 크기 기준 (vw 단위 사용 권장)**:

| 요소 | 최소 크기 | 권장 크기 | 비고 |
|------|----------|----------|------|
| 슬라이드 제목 (h1) | 4vw | 5~7vw | 화면의 주인공. 크면 클수록 좋다 |
| 섹션 제목 (h2) | 3vw | 3.5~4.5vw | |
| 본문 텍스트 | 2vw | 2~2.5vw | **절대 1.5vw 이하로 내려가지 마라** |
| 불릿/리스트 항목 | 1.8vw | 2~2.5vw | |
| 부가 정보/캡션 | 1.4vw | 1.5~1.8vw | 꼭 필요한 경우만 |
| 코드 블록 | 1.6vw | 1.8~2vw | 모노스페이스는 같은 크기여도 작아 보임 |

**px이나 rem 대신 vw를 써라.** 1920px 모니터든 800px 태블릿이든 화면 대비 동일한 비율로 보인다.

**한국어 단어 줄바꿈 방지**: 한국어는 기본적으로 글자 단위로 줄바꿈되어 "백인 우월주" / "의" 처럼 단어 중간에서 잘린다. 이를 방지하기 위해 CSS에 반드시 아래를 추가:
```css
* { word-break: keep-all; overflow-wrap: break-word; }
```
`word-break: keep-all`은 한국어 단어 단위로 줄바꿈하고, `overflow-wrap: break-word`는 정말 긴 단어가 넘칠 때만 글자 단위로 자른다. 이 두 줄은 모든 슬라이드에 필수.

- **메인 폰트**: `'Pretendard'`로 통일. 제목, 본문, UI 요소 모두 Pretendard 사용
  - Google Fonts CDN: `https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css`
- **서브 폰트 (감성/설명용)**: `'Gowun Batang'` — 따뜻하고 감성적인 바탕체. **항상 font-weight: 700 (볼드)로 고정**. 가는 획이 화면에서 깨지는 걸 방지
  - Google Fonts CDN: `@import url('https://fonts.googleapis.com/css2?family=Gowun+Batang:wght@700&display=swap');`
  - CSS: `.serif { font-family: 'Gowun Batang', serif; font-weight: 700; }`
  - **Gowun Batang을 쓰는 곳**: 설명문, 대화체/구어체 텍스트, 인용문, 부제, 캡션, "~입니다", "~해요" 같은 따뜻한 문장. 사람이 말하는 듯한 톤의 텍스트는 전부 Gowun Batang
  - **Pretendard를 쓰는 곳**: 제목, 숫자/데이터, 키워드, 버튼, 라벨 등 딱딱하고 정보 전달 위주의 텍스트
- 제목은 크고 볼드하게(700-800), 본문은 가볍게(400-500)
- 자간(letter-spacing): 제목은 -0.02em~-0.03em (타이트하게), 본문은 기본값
- **한 슬라이드에 문장이 많으면 글자를 줄이지 말고 슬라이드를 나눠라**

---

## 슬라이드 유형별 디자인 패턴

각 슬라이드를 "제목 + 불릿 포인트" 일색으로 만들지 마라. 아래 패턴을 섞어서 시각적 리듬을 만들어라.

### 타이틀 슬라이드
- 제목을 **크게**. 화면의 40-60%를 제목이 차지해도 됨
- 부제는 작고 가벼운 색으로
- 배경에 미세한 그라데이션이나 기하학적 장식 가능
- 로고/브랜딩이 있으면 좌상단이나 중앙에

### 데이터/숫자 슬라이드
- 핵심 숫자를 **크게** (font-size 4rem 이상)
- 숫자 옆에 간결한 설명
- 비교 시: 2단 또는 3단 그리드
- **막대 차트 구현 시 검증된 구조** (height %가 안 먹히는 문제 방지):

```html
<div class="bar-chart">
  <div class="bar-col">
    <div class="bar" style="height: 15vh;"></div>
    <span class="bar-val">$120K</span>
    <span class="bar-label">Q1</span>
  </div>
  <!-- 반복 -->
</div>
```

```css
.bar-chart {
  display: flex;
  align-items: flex-end;  /* 막대가 아래에서 위로 자람 */
  gap: 2vw;
  height: 35vh;           /* 차트 영역 높이를 vh로 고정 */
  padding: 0 2vw;
  border-bottom: 2px solid #ddd;
}
.bar-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.bar {
  width: 60%;
  background: #1a56db;
  border-radius: 4px 4px 0 0;
  /* height는 vh 단위로 inline style 지정. % 쓰면 안 먹힘 */
}
.bar-val { font-size: 1.4vw; font-weight: 700; margin-bottom: 0.5vh; }
.bar-label { font-size: 1.2vw; color: #888; margin-top: 0.8vh; }
```

핵심: `.bar`의 높이를 `%`가 아닌 **vh 단위**로 inline style에 직접 지정한다 (예: `style="height: 8vh;"`, `style="height: 20vh;"`). flex 자식에 `height: %`는 부모에 명시적 height가 있어도 계산이 꼬이는 경우가 많다.

### 프로세스/흐름 슬라이드
- 수평 또는 수직 흐름도
- 단계 번호(01, 02, 03)를 크게
- 화살표(→)나 라인으로 연결
- 각 단계에 짧은 설명
- **순차 등장 필수**: 같은 레벨의 항목(단계, 카드, 리스트 등)은 프래그먼트로 하나씩 순서대로 올라와야 한다. 한꺼번에 다 보여주면 발표 효과가 없다

### 비교 슬라이드
- 2단 레이아웃: Before/After, 우리/경쟁사
- 시각적으로 "좋은 쪽"이 돋보이게 (색상, 크기 차이)

### 인용/하이라이트 슬라이드
- 한 문장을 화면 가득 채워라
- 큰 따옴표, 큰 폰트
- 배경색 변화로 시각적 전환

### 코드 슬라이드
- 코드 블록에 macOS 스타일 창 헤더 (빨강/노랑/초록 점)
- 줄 번호 표시
- 구문 강조는 HTML에 직접 `<span>` 마크업으로 — JS 후처리보다 품질이 높음
- 줄별 하이라이팅이 필요하면 각 줄을 `<span class="code-line">`으로 감싸고 CSS로 배경색 토글

### 팀/인물 슬라이드
- 원형 또는 사각 아바타 자리 (실제 이미지 없으면 이니셜 + 배경색)
- 이름, 직함, 이전 경력을 간결하게
- 그리드 배치 (2x2 또는 1x4)

### 시장규모 슬라이드
- **동심원(concentric circles) 금지**. CSS로 동심원을 만들면 안쪽 원이 바깥 원 텍스트를 가리고, 화면 비율에 따라 겹침 문제가 반복적으로 발생한다. 절대 쓰지 마라.
- 대신 아래 대안 중 하나를 써라:
  - **가로 막대 그래프**: TAM/SAM/SOM을 width 비율로 표현. 가장 안정적이고 직관적
  - **3단 카드**: 각각 독립된 카드에 숫자 크게 + 레이블. 깔끔하고 깨지지 않음
  - **세로 스택**: 위에서 아래로 TAM → SAM → SOM 순서, 점점 작은 숫자와 진한 색
- 숫자를 크게(3vw+), 레이블을 작게

### 로드맵/타임라인 슬라이드
- 수평 타임라인: flexbox로 단계 나열, 라인으로 연결
- 현재 시점 강조 (색상 또는 크기)
- 날짜/분기 위에, 내용 아래에
- **점과 선 정렬이 깨지기 쉽다.** 이 문제가 반복적으로 발생하므로, 아래 구조를 **정확히 그대로** 사용해라. 변형하지 마라.

구조 핵심: 타임라인을 **상단(라벨) / 중단(선+점) / 하단(설명)** 3행으로 나눈다. 점과 선은 같은 행에 있으므로 절대 어긋나지 않는다.

```html
<div class="timeline">
  <!-- 행 1: 날짜 라벨 -->
  <div class="tl-labels">
    <div class="tl-label">2025 H1</div>
    <div class="tl-label">2025 H2</div>
    <div class="tl-label current">2026 H1</div>
    <div class="tl-label">2027</div>
  </div>
  <!-- 행 2: 선 + 점 (같은 행이므로 무조건 정렬됨) -->
  <div class="tl-track">
    <div class="tl-line"></div>
    <div class="tl-dot"></div>
    <div class="tl-dot"></div>
    <div class="tl-dot current"></div>
    <div class="tl-dot"></div>
  </div>
  <!-- 행 3: 설명 -->
  <div class="tl-descs">
    <div class="tl-desc">MVP 출시</div>
    <div class="tl-desc">PMF 검증</div>
    <div class="tl-desc">스케일업</div>
    <div class="tl-desc">글로벌 확장</div>
  </div>
</div>
```

```css
.timeline { width: 90%; margin: 0 auto; }
.tl-labels, .tl-descs {
  display: flex; justify-content: space-between; text-align: center;
}
.tl-labels > *, .tl-descs > * { flex: 1; }
.tl-label { font-size: 1.8vw; font-weight: 700; color: #333; padding-bottom: 2vh; }
.tl-label.current { color: #059669; }

.tl-track {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 4vh;
  margin: 1vh 0;
  padding: 0 1vw;          /* 양 끝 패딩 → 점이 선과 정확히 일치 */
}
.tl-line {
  position: absolute;
  left: 1vw; right: 1vw;   /* 선도 동일한 패딩만큼 안쪽 */
  top: 50%;
  transform: translateY(-50%);
  height: 3px;
  background: #ddd;
  z-index: 0;
}
.tl-dot {
  width: 1.8vw; height: 1.8vw;
  border-radius: 50%;
  background: #1a56db;
  border: 3px solid #fff;
  z-index: 1;
  flex-shrink: 0;
}
/* 라벨/설명도 동일한 패딩 */
.tl-labels, .tl-descs { padding: 0 1vw; }
.tl-dot.current {
  width: 2.4vw; height: 2.4vw;
  background: #059669;
  box-shadow: 0 0 0 4px rgba(5,150,105,0.3);
}

.tl-desc { font-size: 1.4vw; color: #666; padding-top: 2vh; }
```

이 구조에서 점과 선은 `.tl-track` 안에서 `align-items: center`로 정렬되므로 **절대 어긋나지 않는다.** 라벨과 설명은 별도 행이라 점 위치에 영향을 주지 않는다.

---

## 빌드 효과 (프래그먼트)

한 슬라이드 안에서 요소를 **하나씩 등장**시키면 발표의 흐름을 제어할 수 있다. 구현 방법은 자유지만, 기본 패턴:

```html
<div class="fragment">첫 번째</div>
<div class="fragment">두 번째</div>
```

```css
.fragment { opacity: 0; transform: translateY(20px); transition: all 0.4s ease; }
.fragment.visible { opacity: 1; transform: translateY(0); }
```

- "다음" 키를 누르면 프래그먼트가 먼저 순서대로 등장
- 모든 프래그먼트가 등장한 후에야 다음 슬라이드로 이동
- "이전" 키를 누르면 프래그먼트를 역순으로 숨김

### 프래그먼트를 써야 하는 곳과 쓰지 말아야 하는 곳

**반드시 써라:**
- 같은 레벨의 항목 나열 (카드 3개, 리스트 항목, 프로세스 단계 등) → 하나씩 순서대로 등장
- 숫자/데이터 슬라이드의 핵심 수치 → 하나씩 드러내며 설명
- 비교 슬라이드의 양쪽 → 한쪽 먼저, 다른 쪽 다음

**쓰지 마라:**
- 제목 + 부제만 있는 간단한 슬라이드
- 이미 충분히 간결한 슬라이드 (요소 1~2개)

---

## 전환 효과

슬라이드 간 전환은 심플하게:
- **fade** (opacity 전환) — 가장 무난하고 프로페셔널
- **slide** (translateX 이동) — 흐름이 있는 내러티브에 적합

```css
.slide {
  position: absolute;
  inset: 0;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.5s ease, transform 0.5s ease;
}
.slide.active { opacity: 1; visibility: visible; }
```

전환 중 추가 입력을 막기 위한 간단한 락:
```javascript
let locked = false;
function navigate(direction) {
  if (locked) return;
  locked = true;
  // ... 전환 로직 ...
  setTimeout(() => locked = false, 500); // 전환 시간과 동일
}
```

---

## 추가 기능 (선택사항)

아래 기능들은 요청이 있을 때만 구현한다. 기본적으로는 깔끔한 프레젠테이션이 우선.

- **프로그레스 바**: 하단에 얇은 라인. height 2-3px. 슬라이드 진행률에 따라 width 변화
- **슬라이드 번호**: 우하단에 작게. 발표 시 현재 위치 파악용
- **발표자 노트**: `<aside class="notes">` 요소로 HTML에 포함. CSS로 `display: none`. S키로 별도 창 열어서 PostMessage로 동기화
- **터치 스와이프**: touchstart/touchend로 스와이프 감지. 모바일 대응
- **URL 해시**: `#/슬라이드번호`로 특정 슬라이드 링크 공유
- **자동 재생**: 설정된 간격으로 자동 전진
- **풀스크린**: F키로 `document.requestFullscreen()` 토글
- **인쇄**: `@media print`로 슬라이드별 페이지 분리

---

## 콘텐츠 구성 가이드

사용자가 주제만 제시하면:

1. **표지** — 제목 크게. 부제, 날짜, 발표자
2. **문제 정의** (해당 시) — 청중이 공감할 수 있는 문제 제시
3. **본문** — 주제당 1~2 슬라이드. 핵심 메시지 중심
4. **시각 자료** — 텍스트만 나열하지 마라. 다이어그램, 숫자, 비교표 활용
5. **요약** — 핵심 3가지
6. **마무리** — Q&A 또는 CTA

**글자 수 원칙**: 슬라이드 하나에 문장 3~5개 이하. 한 문장 당 15단어 이내. 읽는 게 아니라 보는 것. 내용이 많으면 **폰트를 줄이지 말고 슬라이드를 쪼개라.**

**언어 원칙**: 사용자가 한국어로 요청하면 슬라이드 콘텐츠도 **한국어**로 작성한다. 섹션 라벨, 제목, 본문, 캡션 모두 한국어. 고유명사, 기술 용어, 회사명 등 영어가 자연스러운 경우만 영어 허용 (예: "FastAPI", "Series A", "SaaS"). "Problem", "Solution", "Traction" 같은 섹션 라벨을 영어로 쓰지 마라 — "문제", "솔루션", "성과"로 써라.

---

## 체크리스트 (완성 전 확인)

- [ ] 모든 슬라이드가 전체화면에서 스크롤 없이 완결되는가
- [ ] 키보드(←→ Space)로 네비게이션이 되는가
- [ ] 이모지를 남발하지 않았는가
- [ ] 블랙+네온 조합이 아닌, 주제에 맞는 색상을 사용했는가
- [ ] 모든 슬라이드가 동일 레이아웃이 아닌가 (시각적 다양성)
- [ ] 외부 라이브러리를 사용하지 않았는가
- [ ] 단일 HTML 파일로 동작하는가
