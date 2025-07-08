---
title: "프록시(Proxy)의 모든 것"
date: "2025-07-05"
tags: ["proxy", "reverse-proxy", "nextjs", "web-development"]
summary: "프록시 개념과 네트워크, 리버스, 이미지 프록시 차이·활용법 정리"
---

# 프록시(Proxy)

프록시는 클라이언트와 서버 사이에서 요청/응답을 대신 중계하는 서버  
같은 프록시라도 위치와 목적 따라 종류 다양함

---

## 📌 주요 프록시 유형 비교

| 구분            | 동작 위치         | 주요 목적                                   | 예시                       |
| --------------- | ----------------- | ------------------------------------------- | -------------------------- |
| 네트워크 프록시 | 클라이언트 앞단   | 인터넷 접속 우회, IP 은닉, 검열, 캐싱, 보안 | 회사/학교 프록시 서버, VPN |
| 리버스 프록시   | 서버 뒷단         | 로드밸런싱, SSL 종료, 트래픽 분산, 보안     | Nginx, Cloudflare, AWS ALB |
| 이미지 프록시   | (주로) API 백엔드 | 외부 이미지 CORS 우회, 캐싱, 핫링크 방지    | Next.js API, 이미지 CDN    |

---

## 1️⃣ 네트워크 프록시

- 클라이언트(사용자) 앞단에서 동작
- 내 컴퓨터가 외부와 직접 통신 X, 프록시 서버 통해서만 인터넷 접속
- IP 숨김, 트래픽/접근 통제, 캐싱, 검열 우회 목적
- 회사, 학교 프록시 설정, VPN 등이 대표 예시

---

## 2️⃣ 리버스 프록시

- 서버(서비스 제공자) 뒷단에서 동작함
- 외부 요청 먼저 받아서, 내부 서버들로 대신 전달함
- 로드밸런싱, SSL, 보안 필터, 캐시 목적
- Nginx 리버스 프록시, Cloudflare, AWS ALB 등 실제로 많이 사용

---

## 3️⃣ 이미지 프록시

- (주로) API 백엔드에서 특정 리소스(이미지 등)만 중계
- 외부 이미지 주소 노출하지 않고, 내 서버(API) 통해 이미지만 전달
- CORS 우회, 핫링크 방지, 접근 제한, 캐싱 등에 유용
- Next.js API로 외부 이미지 대신 fetch해서 프론트에 넘겨주는 경우 많음

---

## 4️⃣ 캐싱 프록시

- 중간 프록시 서버에서 요청 많은 데이터(페이지·이미지 등) 미리 저장
- 재요청 시 빠르게 응답, 서버/네트워크 트래픽 절감
- Squid, Varnish 등 대표적, 회사·ISP에서 많이 씀

---

## 🛠️ Next.js 이미지 프록시 API 예시

실제로 동작하는 Next.js 이미지 프록시 코드 예시

```typescript
// pages/api/notion-image.ts
import type { NextApiRequest, NextApiResponse } from "next";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const { url } = req.query;
  if (!url || typeof url !== "string")
    return res.status(400).end("url required");

  try {
    const notionRes = await fetch(url);

    if (!notionRes.ok) {
      return res.status(notionRes.status).send("Failed to fetch image");
    }

    // Buffer로 변환
    const arrayBuffer = await notionRes.arrayBuffer();
    const buffer = Buffer.from(arrayBuffer);

    res.setHeader("Cache-Control", "public, max-age=86400, immutable");
    res.setHeader(
      "Content-Type",
      notionRes.headers.get("content-type") || "image/jpeg"
    );
    res.send(buffer);
  } catch (e) {
    res.status(500).send("Image proxy error");
  }
}
```

---

## 📦 실제 사용 예시

프론트엔드에서 아래처럼 사용

```jsx
<img
  src="/api/notion-image?url=https://notion.so/your-image-url.jpg"
  alt="notion-image"
/>
```

- 원래 CORS 문제로 표시 안 되던 이미지도
  내 서버 프록시 통해 안전하게 표시 가능

---

## 🌟 프록시 쓸 때 주의할 점

- 남용/무단 중계하면 저작권, 트래픽 문제 생길 수 있음
- 보안상 url 파라미터 무작정 받으면 SSRF 등 취약점 생김
  → 신뢰 도메인만 허용하거나 방어 로직 필요
- 캐싱 잘 설정해서 트래픽 부담을 줄여야함
- 보안·트래픽 관리에 신경을 써야함

---

## 🚩 결론

- 프록시는 “중간에서 대신 전달” 개념이라는 공통점이 존재함
- 실무에서 위치·목적 따라 네트워크/리버스/이미지/캐싱 등 다양하게 활용
- 프록시 원리만 제대로 알아도 웹서비스 안전하고 효율적으로 만들 수 있음
- 이미지 프록시는 CORS·핫링크·캐싱 등 프론트엔드에서 실전 활용도 높음
- 웹서비스 효율·보안 향상, 인프라 관리에 필수 개념
