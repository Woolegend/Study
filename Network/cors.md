# Cross-Origin Resource Sharing

"**Cross-Origin Resource Sharing**"를 직역하면 "**교차 출처 자원 공유**"라고 해석할 수 있다. 여기서 교차 출처는 서로 다른 출처를 의미한다.

즉, CORS는 서로 다른 출처에서 자원을 공유하기 위한 정책이다.

## 출처란 무엇인가

출처란 네트워크 사에서 자원의 위치를 나타내는 문자열로 URL을 의미한다.

URL은 다음과 같이 여러 구성요소로 이루어져 있다.

1. Protocol(Scheme) : `http` `https`
2. Host : `사이트 도메인` `ex) example.com`
3. Port : 포트 번호
4. Path : 사이트 내부 경로
5. Query string : 쿼리 파라미터
6. Fragment : 해시 태그

이 중 출처를 구분하는데 사용되는 구성요소는 `Protocol`, `Host`, `Port`다

- Origin = Protocol + Host + Port <sub>포트 번호가 80인 경우 생략된다.</sub>

`Protocol`, `Host`, `Port`가 하나라도 다르다면 서로 다른 출처로 구분된다. 다음 출처는 모두 구분되는 다른 출처라고 볼 수 있다.

- `http://example.com:8080`
- `http://example.com`
- `https://example.com:8080`

## CORS의 작동방식

CORS는 서로 다른 출처의 리소스 공유레 대한 허용/비허용 정책이다. CORS 정책을 위반한 요청을 할 경우 CORS 오류가 발생할 것이다.

이 [교차 출처 공유 표준](https://fetch.spec.whatwg.org/#http-cors-protocol)은 다음과 같은 경우에 교차 출처 HTTP 요청을 가능하게 합니다.

### 예비 요청 Preflight Request

예비 요청을 보내 서버와 통신이 잘 되는지 확인한다. 본 요청을 보내기 전에 안전한 요청인지 확인하는 것이다.

이때 보내는 예비 요청을 **Preflight**라고 부른다. 이 예비 요청의 HTTP 메소드를 GET이나 POST가 아닌 OPTIONS라는 요청이 사용된다.

Preflight는 보안 강화의 목적으론 좋지만 실제 요청에 대한 응답까지의 시간이 늘어 성능에 영향을 미치는 단점이 있다.

### 단순 요청 Simple Request

단순 요청은 예비 요청을 생략하고 바로 서버에 본 요청을 보낸 후, 서버가 이에 대한 응답 헤더에 Access-Control-Allow-Origin 헤더를 보내주면 브라우저에서 CORS 위반 여부를 검사한다.

다만, 특정 조건을 모두 만족해야 예비 요청을 생략할 수 있다.

대표적으로 다음 3가지 경우가 있다.

- 요청의 메소드는 `GET`, `HEAD`, `POST` 중 하나
- `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width` 헤더일 경우
- Content-Type 헤더가 `application/X-www-form-urlencoded`, `multipart/form-data`, `text/plane` 중 하나

까다로운 조건들을 만족해야 하기 때문에 단순 요청이 일어나는 상황은 드물다.

보통 HTTP API 요청은 `text/xml` 이나 `application/json`으로 통신하기 때문에 3번에 조건을 위반한다.

따라서, 대부분의 API 요청은 그냥 예비 요청으로 이루어진다.

### 인증된 요청 Credentialed Request

인증된 요청은 클라이언트에서 서버에게 [자격 인증 정보](, "Credential")를 포함한 요청이다.

자격 인증 정보란 세션 ID가 저장되어 있는 쿠키 혹은 Authorization 헤더에 성정한 토큰 값을 일컫는다.

기본적으로 브라우저가 제공하는 요청 API들은 별도 옵션 없이 브라우저의 쿠키와 같은 인증과 관련된 데이터를 함부로 요청 데이터에 담지 않도록 해야한다.

이때, 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 `credentials` 옵션이다. 이 옵션에는 3가지 값을 사용할 수 있으며, 각 값들이 가지는 의미는 다음과 같다.

| 옵션 값              | 설명                                            |
| -------------------- | ----------------------------------------------- |
| same-origin(기본 값) | 같은 출처 간 요청에만 인증 정보를 담을 수 있다. |
| include              | 모든 요청에 인증 정보를 담을 수 있다.           |
| omit                 | 모든 요청에 인증 정보를 담지 않는다.            |

이러한 별도의 설정을 해주지 않으면 쿠키 등의 인증 정보는 절대로 자동으로 서버에게 전송되지 않는다.

서버에서도 마찬가지로 인증된 요청에 대해 일반적인 CORS 요청과는 다르게 대응해야한다.

- 응답 헤더의 `Access-Control-Allow-Credentials` 항목을 true로 설정
- 응답 헤더의 `Access-Control-Allow-Origin`의 값에 와일드카드 문자는 사용할 수 없다.
- 응답 헤더의 `Access-Control-Allow-Method`의 값에 와일드카드 문자는 사용할 수 없다.
- 응답 헤더의 `Access-Control-Allow-Headers`의 값에 와일드카드 문자는 사용할 수 없다.

응답의 설정이 되어 있지 않다면 CORS 정책에 의해 응답이 거부된다. 인증 정보는 민간하기 때문에 출처를 명확하게 설정해야한다.

## CORS 해결 방법

### 서버 Acess-Control-Allow-Origin 헤더 세팅

직접 서버에서 HTTP 헤더 설정을 통해 출처를 허용할 수 있다. 각 서버의 문법에 맞게 HTTP 헤더를 추가하면 된다.

## 참고

> [CORS | 나무위키](https://namu.wiki/w/CORS)  
> [악명 높은 CORS 개념 & 해결법 - 정리 끝판왕 | Inpa Dev](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F)  
> [교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
