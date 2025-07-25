---
title: "삼성 인터넷 브라우저에서 다크모드/회색 배경, Lottie 색상 깨짐 현상 정리"
date: "2025-07-07"
tags: ["samsung-internet", "darkmode", "web-development", "front-end", "issue"]
summary: "삼성인터넷의 어둡게 보기(스마트 다크모드) 옵션이 웹 색상과 Lottie, SVG 등에 미치는 영향 및 실전 해결 방법 정리"
---

# 삼성인터넷 “어둡게 보기” 옵션이 웹 색상·Lottie에 미치는 영향

모든 프론트엔드 개발자가 한 번쯤 겪는 **삼성인터넷 회색/어둡게 보기 트랩**  
PC/카카오톡 인앱 브라우저/크롬 등에서는 의도한 대로 나오지만  
**삼성인터넷(특히 어둡게 보기, 스마트 다크모드 켤 때)**만 색상이 회색/탁하게 나오는 현상 많음

---

## 📲 카카오톡 인앱 vs 삼성인터넷 기본 브라우저 차이

- **카카오톡 인앱 브라우저:**  
  크롬(WebView) 기반, 시스템 다크모드/필터 영향 적음  
  Tailwind, Nextra, Lottie 등 밝은 색상 CSS 그대로 반영

- **삼성인터넷 브라우저:**  
  자체 **라이트/다크모드, 스마트 다크모드, 배경 최적화** 등  
  웹 CSS를 무시하고 **필터·회색·반전 오버레이 강제 적용**  
  특히 **Canvas, SVG, Lottie**는 더 강하게 색상 변환됨  
  → 라이트모드여도 전체적으로 회색·탁한 색상, Lottie/SVG 색상 깨짐

---

## 🟠 이럴 때 해결 방법

### 1. 삼성인터넷 “스마트 다크모드/어둡게 보기” OFF

- 브라우저 메뉴에서 “다크모드”, “페이지 다크모드”, “스마트 다크모드” 꺼보기
- 대부분 이걸 끄면 Lottie/배경색 정상

### 2. `<meta name="theme-color" content="#fff">` 명시

- Next.js `_document.js`나 `<Head>`에 추가

  ```html
  <meta name="theme-color" content="#fff" />
  ```

- 일부 브라우저에서는 기본 배경을 흰색으로 강제 가능

### 3. 사용자 안내

- “삼성인터넷 ‘어둡게 보기’ 기능은 사이트 색상을 왜곡할 수 있습니다, 색상 이상 시 옵션 해제 바랍니다.”
- 자주 사용하는 서비스라면 팝업/툴팁 등으로 안내

### 4. 완벽 일치는 불가

- 삼성인터넷이 CSS 위에 자체 필터 덮어쓰기 때문
- **Lottie, SVG, Canvas 색상까지 100% 일치는 불가능**

---

## 🟢 결론/요약

- 삼성인터넷(및 일부 브라우저)은 시스템/브라우저 옵션에 따라 **웹 배경, SVG, Lottie 색을 강제 보정**함
- **어둡게 보기 OFF하면** 정상 색상
- **이건 코드/개발자 잘못 아님!** (모든 웹사이트 동일하게 겪는 현상)
- `<meta name="theme-color">`로 완화만 가능, 완벽 통일은 불가
