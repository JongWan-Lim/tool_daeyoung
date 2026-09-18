# 철물점 홈페이지 (행복철물 예시)

`index.html` 파일 하나로 완성된 정적 홈페이지입니다. 빌드 과정이 없어서
Vercel에 그대로 올리면 바로 배포되고, 유지비용은 0원입니다.

## 1. 파일 구성

```
index.html   ← 이 파일 하나면 배포 가능 (CSS/JS 모두 내장)
README.md    ← 지금 보고 있는 안내 문서
```

사진을 추가하기 시작하면 `images/` 폴더를 만들어 그 안에 넣고,
`index.html`에서 경로만 연결해주면 됩니다 (아래 3번 참고).

## 2. 내용(텍스트) 수정하는 방법

`index.html`을 열어서 `<script id="site-config">` 안에 있는 `CONFIG` 객체만
수정하면 화면 전체에 자동으로 반영됩니다. HTML 구조는 건드릴 필요가 없습니다.

```js
const CONFIG = {
  storeName: "행복철물",              // 실제 상호명
  tagline: "...",                    // 한 줄 소개 문구
  phone: "010-0000-0000",            // 실제 전화번호 (하이픈 포함, 자동으로 tel: 링크에 반영됨)
  phoneDisplay: "010-0000-0000",     // 화면에 보여줄 전화번호 표기
  address: "...",                    // 실제 주소
  naverMapUrl: "https://map.naver.com/p/search/행복철물", // 아래 4번 참고
  hours: [ ... ],                    // 영업시간 목록
  badges: [ ... ],                   // "믿고 찾아주시는 이유" 3개 카드
  products: [ ... ],                 // 취급 품목 카드들 (자유롭게 추가/삭제 가능)
  extras: [ ... ],                   // "알아두면 좋은 정보" 목록
};
```

배열 항목은 `{ icon: "🔧", name: "...", desc: "..." }` 형태만 지키면
개수를 자유롭게 늘리거나 줄일 수 있습니다.

## 3. 사진 추가하는 방법

지금은 사진이 없어서 빗금 무늬 "사진 자리" 박스로 채워져 있습니다.
아버님이 사진을 찍어 보내주시면:

1. `index.html`과 같은 위치에 `images` 폴더를 만들고 사진 파일을 넣습니다.
   (예: `images/store-front.jpg`)
2. 대표 사진은 히어로 영역의 아래 코드를 찾아서

   ```html
   <div class="photo-placeholder hero-photo">
     <span class="icon">📷</span>
     <span>매장 대표 사진 자리<br>(사진 촬영 후 교체)</span>
   </div>
   ```

   다음과 같이 바꿉니다.

   ```html
   <img class="hero-photo" src="images/store-front.jpg" alt="행복철물 매장 전경">
   ```

3. 취급 품목 카드 사진은 JS의 `product-grid` 렌더링 부분에서
   `<div class="photo-placeholder">...</div>` 를
   `<img src="images/공구.jpg" alt="공구">` 형태로 바꾸면 됩니다.
   (품목이 8개면 사진도 8장까지 준비하면 좋고, 일부만 있어도 됩니다.)

사진 용량이 크면 로딩이 느려지니, 카카오톡/네이버로 전송받은 사진 정도의
용량(보통 1MB 이하)이면 충분합니다.

## 4. Vercel로 배포하기 (무료)

컴공이시니 아래는 표준 순서만 정리해둘게요.

### 방법 A. GitHub 연동 (추천 — 나중에 수정하기 편함)

1. GitHub에 새 저장소를 만들고 `index.html`(과 `images/` 폴더)을 push합니다.
2. https://vercel.com 에서 GitHub 계정으로 로그인 (무료 Hobby 플랜).
3. "Add New… → Project"에서 방금 만든 저장소를 Import.
4. Framework Preset은 **Other**(정적 사이트)로 두고 Build Command는 비워둔 채 Deploy.
5. 배포가 끝나면 `프로젝트명.vercel.app` 형태의 무료 도메인이 생깁니다.
6. 이후 내용을 수정하면 GitHub에 push할 때마다 자동으로 재배포됩니다.

### 방법 B. Vercel CLI로 바로 배포 (제일 빠름)

```bash
npm i -g vercel      # 최초 1회
cd 이 폴더 경로
vercel               # 로그인 후 질문에 기본값으로 엔터 몇 번이면 배포됨
vercel --prod        # 실제 서비스용 URL로 배포
```

두 방법 모두 정적 파일이라 무료 Hobby 플랜 한도 안에서 유지비 0원으로 운영됩니다.
커스텀 도메인(예: `happyhardware.co.kr`)을 따로 구매하지 않는 한 계속 무료입니다.

## 5. 네이버 지도에서 클릭할 수 있게 등록하기

배포된 URL(`https://프로젝트명.vercel.app`)이 생기면, 이걸 네이버 스마트플레이스에
등록해야 지도 검색 결과에서 클릭할 수 있게 됩니다.

1. https://smartplace.naver.com 접속 후 사장님(아버님) 계정으로 로그인
   (아직 스마트플레이스 등록이 안 되어 있다면 먼저 업체 등록 필요).
2. 내 업체 관리 → 정보 수정으로 이동.
3. "홈페이지" 또는 "SNS/홈페이지" 입력란에 Vercel 배포 URL을 붙여넣기.
4. 저장 후 검토 기간(보통 1~수일)이 지나면 지도/검색 결과에 홈페이지 링크가 노출됩니다.

## 6. `naverMapUrl` 값 채우는 방법

1. 네이버 지도 앱/웹에서 실제 가게를 검색해서 상세 페이지를 엽니다.
2. 주소창 URL을 그대로 복사해서 `CONFIG.naverMapUrl` 값에 붙여넣습니다.
   (예: `https://map.naver.com/p/entry/place/123456789`)
3. 홈페이지의 "네이버 지도에서 길찾기/보기" 버튼이 정확한 매장 위치로 연결됩니다.

궁금한 점이나 디자인/구성을 더 바꾸고 싶은 부분이 있으면 언제든 말씀해주세요.
