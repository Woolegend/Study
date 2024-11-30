# Search Engine Optimization

SEO는 검색엔진 최적화의 줄임말로, 사용자 혹은 검색엔진이 내 콘텐츠를 발견할 수 있게 웹 페이지를 구성하는 것이다. 검색엔진이 콘텐츠를 이해하도록 돕고, 사용자가 사이트를 찾고, 검색엔진을 통해 사이트를 방문할지 여부를 결정하도록 돕는다.

## 검색 엔지 최적화 개선

React와 Next.js에서 검색엔진 최적화를 개선하기 위한 방법은 다음과 같다

### 메타 태그 최적화

Next.js의 next/head 컴포넌트를 사용하여 title, description 등의 메타 태그를 동적으로 설정합니다.
Next.js 13 이후 버전에서는 Metadata API를 활용하여 메타 태그를 더욱 효과적으로 관리할 수 있습니다.

### SSR 활용

Next.js의 SSR 기능을 활용하여 검색 엔진이 페이지 내용을 더 잘 이해할 수 있도록 합니다.

### 구조화된 데이터 추가

JSON-LD를 사용하여 schema.org 마크업을 추가합니다. 이는 검색 엔진이 콘텐츠를 더 잘 이해하고 리치 결과를 표시하는 데 도움이 됩니다3.
이미지 최적화
Next.js의 Image 컴포넌트를 사용하여 이미지를 최적화하고, 필수 alt 태그를 추가합니다.

### 성능 최적화

Next.js의 자동 코드 분할 및 최적화 기능을 활용합니다.
CSS와 JavaScript 파일을 최소화하고 최적화합니다.

### 콘텐츠 품질 향상

고품질의 SEO 최적화된 콘텐츠를 작성하여 사용자 경험을 개선하고 검색 엔진 랭킹을 높입니다.

### 기타 최적화 방법

- 사이트맵 생성 및 robots.txt 파일 설정
- 내부 링크 구조 최적화
- 페이지 로딩 속도 개선

## 참고

> https://velog.io/@zinukk/d-v8gyfq4x  
> https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=ko
