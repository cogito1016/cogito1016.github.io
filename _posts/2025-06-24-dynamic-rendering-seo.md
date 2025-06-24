---
layout: single
title: "동적 라우팅 기반 웹사이트가 Google 검색 상단에 노출되는 원리 – converter.net 사례 분석"
categories: "seo"
tag: ["seo", "dynamic-rendering", "google-bot", "search"]
toc: true
---

오늘 동료분께서 `1500000000 microseconds to seconds`라는 검색어를 구글에 검색했을 때, `https://converter.net/time/1500000000-microseconds-to-seconds`가 **최상단**에 노출되는 걸 보고 동작방식에 의문이 들어 질문을 해주셨다.

질문 핵심은 다음과 같다.
- 이 URL은 정적 페이지가 아니다.
- 수많은 변환 조합 중 하나일 뿐이다.
- 그런데도 검색 결과 맨 위에 노출된다.

반면, `9098357 microseconds to seconds` 같은 비슷한 URL은 **검색 결과에 전혀 나오지 않는다**.  
**무엇이 이 차이를 만들었을까?**    
이 글에서는 converter.net이 어떤 구조로 동작하며, 어떤 SEO 전략을 통해 구글 검색 상단에 노출되었는지를 나름대로 분석 정리해봤다.

---

## ✅ 문제 제기

- 동일한 구조의 동적 URL인데도 어떤 쿼리는 검색 상단에, 어떤 쿼리는 아예 검색 결과에 없다.
- converter.net은 모든 경우에 대해 정적 HTML을 만들어두지 않는다.
- 그렇다면 Googlebot은 이런 URL을 **어떻게 인지하고 색인하며**, **검색 결과에 노출**시키는 걸까?

---

## 🧠 먼저, Googlebot은 어떻게 URL을 발견하는가?

Googlebot이 특정 URL을 “알고” 접근할 수 있는 경로는 다음 3가지뿐이다:

1. **내부 링크**  
   - 예: `/time/1-microsecond-to-second` 페이지에 관련 링크로 `/time/1500000000-microseconds-to-seconds`가 포함된 경우  
   - Googlebot은 이 링크를 따라가며 새 페이지를 색인함

2. **사이트맵(XML)**  
   - converter.net은 수백 개의 sitemap을 통해 다양한 변환 페이지를 미리 Google에 등록함  
   - 예: `/sitemap-conversions-201.xml` 안에 수천 개의 변환 URL 포함

3. **외부 링크 (Backlink)**  
   - Reddit, 블로그, 포럼 등에서 해당 URL이 언급되면 Googlebot은 이를 통해 해당 URL 존재를 알게 됨

> 🔒 단순히 사용자가 직접 주소창에 입력했다고 해서 Googlebot이 알게 되는 것은 **절대 아닌 것 같다**.

---

## 🔧 동적 라우팅 URL이 색인되기 위한 조건

단순히 “URL이 있다”는 것으로는 부족하다. Googlebot이 해당 URL을 크롤링했을 때 **색인에 등록**되려면 다음을 만족해야 한다:

1. **서버가 200 OK + 유의미한 HTML 반환**
   - 동적 라우팅이더라도 SSR 또는 프리렌더링으로 실제 콘텐츠가 있어야 함

2. **고유한 `<title>`과 `<meta description>`**
   - 예:  
     ```html
     <title>Convert 1500000000 Microseconds to Seconds - Converter.net</title>
     <meta name="description" content="1500000000 microseconds is equal to 1500 seconds. Learn how to convert µs to s.">
     ```

3. **본문에 텍스트 콘텐츠 포함**
   - 공식, 계산 과정, 결과 해석 등 SEO에 의미 있는 텍스트가 있어야 함

4. **robots.txt, meta robots 허용 상태**
   - `Disallow`나 `noindex` 설정이 없어야 색인 가능

---

## 🧩 converter.net의 구조 분석

- ✅ URL 패턴: `/time/{value}-{unit1}-to-{unit2}`
- ✅ 서버는 요청 시 동적으로 HTML을 생성 (SSR 기반으로 추정)
- ✅ 각 페이지에 고유한 제목, 설명, 계산 결과 포함
- ✅ sitemap 인덱스를 통해 수백만 URL을 색인 대상에 포함
- ✅ 내부 페이지에 ‘최근 검색’, ‘인기 변환’ 링크를 동적으로 추가해 **Googlebot의 탐색 범위 확장**
- ✅ UX 최적화 (빠른 로딩, 모바일 대응, 깔끔한 결과 노출)

---

## 색인되지 않은 URL은 왜 빠지는가?

예: `https://converter.net/time/9098357-microseconds-to-seconds`

이 URL이 검색에 안 뜨는 이유는 다음 중 하나일 가능성이 높다:

- ❌ sitemap에 포함되지 않음
- ❌ 내부 링크가 존재하지 않음
- ❌ 외부에 링크된 적 없음
- 🔁 Googlebot이 아직 크롤링하지 않았거나, 중복 판단으로 색인 보류
- 💡 너무 무작위 숫자라서 Google이 색인 가치가 낮다고 판단했을 수도 있음

---
## 🧠 Google이 converter.net을 최상단에 올린 이유

- 검색어와 **완전 일치하는 URL, 제목, 본문 콘텐츠**
- 계산 결과와 설명까지 포함된 **정보 밀도**
- 빠른 응답 속도 + 유저 만족도 높은 UX
- 대량 sitemap 등록 및 내부 링크 구조로 **discoverability** 확보
- 단위 변환이라는 전문성(E-E-A-T) 특화 도메인

---

## 핵심을 뽑아내보자!! (Converter.net 따라잡기 😂)

1. **동적 라우팅 URL 구조를 명확하게 설계한다.**  
   예: `/convert/{value}-{from}-to-{to}`

2. **모든 URL은 SSR or 프리렌더링으로 200 + HTML 반환한다.**

3. **고유한 메타 태그와 본문 내용 제공한다. (템플릿이라도 문제 없음)**

4. **site.com/sitemap.xml 구조로 정기적으로 sitemap 제출한다.**

5. **핵심 페이지끼리는 내부 링크로 연결하여 Googlebot 경로 확보한다.**

6. **불필요한 수백만 조합까지 색인시키지 말고, 유의미한 값 위주로 운영한다.**

---

## 🧾 정리

정적 페이지가 아니어도 검색 상단에 노출되는 건 **가능하다.**  
핵심은 다음 2가지다:

1. **Googlebot이 그 URL을 알 수 있어야 한다.** (sitemap, 링크)
2. **접속했을 때 HTML로 유의미한 내용을 반환해야 한다.**

converter.net은 이 원리를 정교하게 실현한 사례다.  
실제 서비스에서도 유사한 구조를 따라가면 **긴 쿼리(롱테일)**에서도 효과적으로 노출될 수 있다.

---

## 마무리하며..

단위 변환이라는 단순한 기능조차, SEO 구조를 전략적으로 설계하고  
사용자에게 신뢰성 있는 결과를 제공하면,  
전 세계 수많은 검색어에서 **최상단에 노출되는 서비스**로 자리잡을 수 있습니다.

이번 분석을 통해, 저 역시 SEO의 구조와 원리를 조금이나마 이해하게 되었고  
이제는 어떤 아이디어든, 기획 초기부터 **검색 노출 전략까지 포함해**  
글로벌 유저에게 닿을 수 있는 웹서비스를 설계할 수 있겠다는 가능성을 느꼈습니다.

"기능의 단순함"이 "성과의 단순함"을 뜻하진 않겠네요.  
오히려 단순할수록 **핵심에 집중한 구조와 전략이 더 중요**하다는 생각이 들었습니다.

너무 재밌는 주제를 던져준 동료분께 감사를 표하며...🙏    
읽어주셔서 감사합니다 🤓   
(그나저나 사이트맵 이렇게까지 해도되는거였나요..? https://converter.net/sitemap-conversions-28.xml - 이것도 약 300개되는 conversions들 중 하나일 뿐..)
 
## 📚 참고 자료 – Google 공식 문서

- [URL Structure Best Practices for Google Search](https://developers.google.com/search/docs/crawling-indexing/url-structure)  
  동적 URL과 정적 URL의 구조 설계 가이드

- [Dynamic Rendering as a workaround – Google Search Central](https://developers.google.com/search/blog/2018/07/dynamic-rendering)  
  JavaScript 기반 웹사이트의 검색 색인 대응 전략

- [Dynamic URLs vs static URLs – Search Central Blog](https://developers.google.com/search/blog/2008/09/dynamic-urls-vs-static-urls)  
  동적 URL을 색인에 안전하게 사용하는 방법

- [Understand JavaScript SEO Basics](https://developers.google.com/search/docs/crawling-indexing/javascript)  
  JavaScript 기반 페이지의 색인 방식과 구조 설계 팁

- [Crawling and Indexing Overview](https://developers.google.com/search/docs/crawling-indexing/how-search-works)  
  Googlebot이 어떻게 URL을 수집하고 색인하는지 개요

- [Mobile‑first Indexing Best Practices](https://developers.google.com/search/docs/crawling-indexing/mobile/mobile-sites-overview)  
  모바일 기준 색인 방식에서 유의할 점 정리

- [What Is Googlebot – Google Search Central](https://developers.google.com/search/docs/crawling-indexing/overview-googlebot)  
  Googlebot의 기능, 역할, 동작 방식 개요

- [Ask Google to Recrawl Your URLs](https://support.google.com/webmasters/answer/6065812)  
  URL Inspection 도구를 통한 색인 재요청 방법