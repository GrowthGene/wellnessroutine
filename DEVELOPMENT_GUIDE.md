# 6월 웰니스 루틴 큐레이션 랜딩페이지 개발 가이드

## 📋 프로젝트 개요

**목표**: 기존 4개 개별 프로모션 랜딩페이지를 하나의 통합된 큐레이션 랜딩페이지로 통합
- 파트너의 고객 매칭 의사결정 시간 단축
- 기존 고객 재구매 유도
- 4개 프로모션을 하나의 URL(`june-2026.html`)로 관리

**대상 사용자**:
- GrowthGene 파트너 (기본 공유 URL)
- 기존 고객 (파트너가 세분화 링크로 유입)

---

## 🎨 디자인 철학

### 색상 체계 (라이트 테마 · Google Stitch 영감)

```css
--bg: #F7FAF4;              /* 베이지 화이트 배경 */
--surface: #FFFFFF;         /* 카드·박스 배경 */
--surface2: #F2F7EE;        /* 뉘앙스 배경 */
--border: #E4EDE0;          /* 미묘한 경계선 */

--green-deep: #1E4D35;      /* 메인 텍스트·CTA */
--green-mid: #2D7A50;       /* FRESH 패키지 */
--green-light: #52B788;     /* 액센트 */

--gold: #E9A850;            /* OPTIMAL 패키지 */
--coral: #F4855A;           /* 강조·경고 */
--sky: #5BA4D4;             /* 정보성 */

--text: #1A2E1A;            /* 본문 */
--muted: #7A9A7A;           /* 보조 텍스트 */
```

### 타이포그래피
- **메인**: `Noto Sans KR` (본문, 한글)
- **강조**: `DM Sans` (가격, 카운트다운)

### 핵심 요소
- ✨ 깔끔하고 여유 있는 레이아웃
- 🎯 명확한 시각 계층구조
- 📱 모바일 완응형 (390px 이상)
- ⚡ 빠른 로딩 (인라인 CSS/JS, 이미지 CD 대체)

---

## 📄 파일 구조

### `june-2026.html` (현재)

**크기**: ~1,500 lines (HTML + CSS + JS inline)

**주요 섹션**:
1. **고정 카운트다운 바** (`#countdown-bar`)
   - 마감 시간: 2026-06-30 23:59:59 (한국 시간)
   - 1초마다 업데이트
   - 마감 시 "프로모션 종료" 표시

2. **고정 네비게이션** (`<nav>`)
   - 로고 + 점 아이콘
   - 앵커 링크: 진단, 패키지, 웰스파, 스킨, TRME, 쉐이크

3. **히어로 섹션** (`#hero`)
   - 제목: "내 몸에 딱 맞는 6월 웰니스 루틴"
   - Quick-jump 필 링크 5개

4. **진단 위젯** (`#guide`)
   - 8개 고민 항목 (다중선택)
   - 점수 기반 추천 로직
   - 추천 결과 카드 (애니메이션)

5. **패키지 비교** (`#packages`)
   - 3개 카드: FRESH / OPTIMAL / PREMIUM
   - 각 카드: 제품 3-4개, 총 가격, CTA 버튼
   - 호버 효과, 카드 구분 강조

6. **제품 섹션** (`#spa`, `#skin`, `#trme`, `#shake`)
   - 헤더: 아이콘 + 제목 + 마감 배지
   - 탭 인터페이스 (spa: 4개, trme: 암묵적 서브앵커)
   - 제품 그리드 (반응형)
   - 통계 칩 (`--stat-chip`)
   - **KakaoTalk 메시지 박스**:
     - 여러 타입 탭 (신규/기존/ARO 등)
     - 메시지 미리보기 (`<pre>`)
     - 📋 복사 버튼 (클립보드 API)
     - 복사 확인 토스트

7. **크로스셀 조합** (`#combos`)
   - 3개 추천 조합
   - 각: 타이틀, 설명, 총가격, 노트

8. **푸터**

---

## 🧠 핵심 JavaScript 로직

### 1. 카운트다운 타이머
```javascript
const deadline = new Date('2026-06-30T23:59:59+09:00');
// 1초마다 timerEl 업데이트
```

### 2. 패키지 추천 퀴즈 (점수 기반)
**입력**: 사용자가 선택한 고민 항목들
**프로세스**:
1. 각 고민에 `data-scores="fresh:2,optimal:1"` 식 점수 지정
2. 선택된 항목의 점수 누적
3. 가장 높은 점수 패키지 추천

**점수 규칙**:
```
☀️ 피부 건조함/열감        → fresh:2
🧖 각질/거친 피부결        → fresh:2
✨ 피부 탄력/노화          → optimal:2, premium:1
💪 바디 라인 관리          → premium:3
🍳 아침 식습관             → optimal:2, premium:1
⚖️ 뱃살/체중 관리          → optimal:1, premium:2
🌟 전체 웰니스             → premium:4
🏠 홈 디바이스             → premium:4
```

### 3. 탭 전환
```javascript
switchTab(event, section, tab)
// #spa-tab-all, #spa-tab-galvanic 등 표시/숨김
switchKakaoTab(event, section, tab)
// #spa-kakao-new, #spa-kakao-existing 등
```

### 4. KakaoTalk 메시지 복사
```javascript
copyMsg(textId, confirmId)
// navigator.clipboard.writeText() 또는 fallback
// 2초 확인 토스트 표시
```

### 5. 부드러운 스크롤
```javascript
scrollToSection(hash)
// offsetTop - 104px (고정 바 보정)
// 모든 a[href^="#"] 이벤트 리스너 등록
```

---

## 🔗 파트너용 앵커 링크 맵

| URL | 설명 | 대상 고객 |
|-----|------|---------|
| `june-2026.html` | 기본 공유 링크 | 모든 고객 |
| `#guide` | 진단 위젯 | 고민이 명확하지 않은 고객 |
| `#packages` | 3개 패키지 비교 | 패키지별 선택 원하는 고객 |
| `#spa` | 웰스파 iO 루틴 | 바디 케어 관심 고객 |
| `#spa-consumables` | 소모품만 필요 | 기존 웰스파 사용자 |
| `#spa-aro` | ARO 파워유저 | ARO 전환 대상 고객 |
| `#skin` | 얼티밋 케어 (피부) | 피부 케어 관심 고객 |
| `#trme` | TRME 트리플 트라이얼 | 체형 관리 경험자 |
| `#trme-new` | TRME 처음 | 체형 관리 미경험자 |
| `#trme-dormant` | TRME 휴면 고객 | 이전 경험 있으나 중단 |
| `#shake` | TRME 쉐이크 VIP | 영양 보충 관심 고객 |
| `#combos` | 크로스셀 조합 | 여러 제품 함께 구매 고객 |

---

## 🖼️ 제품 이미지 (현재: CSS 그래디언트)

각 제품마다 `.pv-xxx` 클래스로 색상 그래디언트 대체 구성:

| 클래스 | 제품 | 현재 그래디언트 | 교체 방법 |
|--------|------|--------------|---------|
| `.pv-pad` | 패드 핑크 바이옴 | `linear-gradient(135deg, #FFD6E0, #FFA8C0)` | `<img src="NuSkin CDN URL">` |
| `.pv-mask-w` | 마스크 워터풀 | `linear-gradient(135deg, #D0F0FF, #80CCFF)` | `<img>` |
| `.pv-mask-t` | 마스크 트루페이스 | `linear-gradient(135deg, #E8D5FF, #C090FF)` | `<img>` |
| `.pv-cleanser` | 루미스파 클렌저 | `linear-gradient(135deg, #D5F5E3, #82E0AA)` | `<img>` |
| `.pv-ultimate` | 얼티밋 케어 | `linear-gradient(135deg, #FDE8D8, #F4A580)` | `<img>` |
| `.pv-shake` | TRME 쉐이크 | `linear-gradient(135deg, #FDF2E9, #FCCB8F)` | `<img>` |
| `.pv-wellspa` | 웰스파 iO | `linear-gradient(135deg, #E8F8F5, #76D7C4)` | `<img>` |
| `.pv-galvanic` | 갈바닉 젤 | `linear-gradient(135deg, #EBF5FB, #AED6F1)` | `<img>` |
| `.pv-trme` | TRME | `linear-gradient(135deg, #F5E6FF, #D4A8FF)` | `<img>` |

**이미지 교체 방법**:
```html
<!-- 현재 -->
<div class="pv pv-pad">🌸</div>

<!-- 교체 후 -->
<img src="https://nsk.dl.cdn.cloudn.co.kr/product/pad-pink-biome-s.jpg" 
     alt="패드 핑크 바이옴" 
     style="width:100%; height:100%; object-fit:cover; border-radius:10px;">
```

---

## 💬 KakaoTalk 메시지 구조

**4가지 메시지 타입**:

### 웰스파 섹션 (3개 메시지)
- `msg-spa-new`: 신규 구매 고객용
- `msg-spa-existing`: 소모품 재구매 고객용
- `msg-spa-aro`: ARO 전환 대상용

### 스킨 섹션 (2개 메시지)
- `msg-skin-new`: 신규 고객
- `msg-skin-cross`: 콜라겐 크로스셀

### TRME 섹션 (3개 메시지)
- `msg-trme-exp`: 경험자
- `msg-trme-new2`: 미경험자
- `msg-trme-dormant`: 휴면 고객

### 쉐이크 섹션 (3개 메시지)
- `msg-shake-diet`: 다이어트 중
- `msg-shake-busy`: 바쁜 직장인
- `msg-shake-combo`: TRME 조합

**메시지 특징**:
- 한글 + 이모지 섞음
- 짧고 간결함 (150~200자)
- 고객 상황별 맞춤형
- 클릭 1회 → 복사 → 카톡 붙여넣기

---

## 📱 모바일 대응 (390px 이상)

**주요 변경사항**:
- 카드 3열 → 1열
- 네비게이션 링크 숨김 (모바일)
- 패키지 그리드 반응형
- 터치 인터페이스 최적화 (버튼 최소 44px)

**테스트 뷰포트**:
```
390×844 (iPhone SE 사이즈)
768×1024 (태블릿)
1400×900 (데스크톱)
```

---

## 🚀 배포 및 라이브

**Git 브랜치**: `claude/promotion-landing-pages-strategy-vYiz8`

**배포 방식**: Netlify 자동 배포 (푸시 시 자동)

**라이브 URL**: (Netlify 설정 후)
```
https://[site].netlify.app/june-2026.html
또는
https://growthgene.com/june-2026.html
```

**OG Meta 태그**:
- `og:title`: "6월 웰니스 루틴 큐레이션"
- `og:description`: "나에게 맞는 6월 웰니스 패키지를 찾아보세요. FRESH · OPTIMAL · PREMIUM"
- `og:type`: "website"
→ 카카오톡 링크 미리보기 최적화

---

## ✅ 검증 체크리스트

### 기능성
- [ ] 카운트다운 타이머 작동 확인
- [ ] 퀴즈 점수 계산 정확성 (12개 조합 모두)
- [ ] 탭 전환 작동
- [ ] KakaoTalk 메시지 복사 작동
- [ ] 앵커 링크 스크롤 위치 정확

### 디자인
- [ ] 라이트 테마 색상 일관성
- [ ] 타이포그래피 크기 계층구조
- [ ] 호버 효과 시각적 피드백
- [ ] 카드 섀도우 일관성

### 반응형
- [ ] 390px 뷰포트 가로 스크롤 없음
- [ ] 태블릿 레이아웃 (768px)
- [ ] 데스크톱 3열 그리드

### 성능
- [ ] 페이지 로딩 시간 < 2초
- [ ] 이미지 대체 (그래디언트) 로딩 즉시
- [ ] 스크롤 성능 부드러움

### SEO / 소셜
- [ ] og:title, og:description 설정
- [ ] 카카오톡 미리보기 정상 표시
- [ ] 메타 charset UTF-8

---

## 🔧 향후 개선 사항

### Phase 2 (이미지 CDN 통합)
- NuSkin Korea CDN URL 획득 후 제품 이미지 교체
- `.pv-xxx` 그래디언트 → `<img>` 태그 변환

### Phase 3 (분석 & 최적화)
- Google Analytics 통합 (클릭 추적)
- A/B 테스트 (패키지별 클릭율)
- 유입 경로 분석 (앵커 링크 사용량)

### Phase 4 (파트너 대시보드)
- 파트너별 공유 링크 생성
- 고객 유입 통계
- 메시지 성과 분석

### Phase 5 (동적 콘텐츠)
- 제품 가격 CMS 연동
- 마감일 동적 업데이트
- 재고 상태 표시

---

## 📚 참고 파일

- **계획 파일**: `/root/.claude/plans/6-recursive-wall.md`
- **기존 페이지**: `/home/user/wellnessroutine/index.html` (Dark 테마)
- **이 가이드**: `/home/user/wellnessroutine/DEVELOPMENT_GUIDE.md`

---

## 👤 담당자 연락처

- **기술**: 이 파일의 JavaScript/CSS 코드 참조
- **콘텐츠**: 제품명, 가격, 메시지는 `june-2026.html` 내 `<pre id="msg-xxx">` 태그 수정
- **배포**: Git `claude/promotion-landing-pages-strategy-vYiz8` 브랜치 푸시

---

**마지막 업데이트**: 2026-06-02
**상태**: ✅ Phase 1 완료 (라이트 테마 + 퀴즈 + 3단계 패키지)
