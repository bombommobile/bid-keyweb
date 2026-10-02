# 키웹 — 입찰가 암호화 키 관리

분양사와 유니피커가 입찰마다 RSA 키 쌍을 만들어 **자기 회사 GitHub 비공개 저장소**에 보관하는 웹 페이지다.
HTML 파일 하나(`index.html`)이고, GitHub Pages 로 서비스한다.

> 키는 브라우저 안에서 만들고, GitHub API 로 귀사 비공개 저장소에 커밋한다. 토큰은 페이지 메모리에만 있고 페이지를 닫으면 사라진다.

## 왜 GitHub Pages 인가

유니피커 입찰시스템이 이 페이지를 직접 내려주면, 유니피커가 언제든 코드를 바꿔 분양사의 개인키나 토큰을 가져갈 수 있다는 의심을 풀 수 없다.
이 저장소를 **공개**로 두고 GitHub Pages 로 서비스하면, 코드가 바뀔 때마다 공개 커밋 기록이 남고 누구나 확인할 수 있다.

- 페이지는 `connect-src https://api.github.com` CSP 로 GitHub 외의 통신을 막는다.
- 외부 스크립트를 쓰지 않는다. 파일 하나만 읽으면 검증할 수 있다.

## 사용

입찰시스템 관리자 페이지가 입찰 ID 를 붙인 링크를 준다.

```
https://<소유자>.github.io/bid-keyweb/?bid=B-2026-014
```

## 입찰시스템과 맞춰야 하는 값

| 항목 | 값 |
|---|---|
| 알고리즘 | RSA-OAEP 2048, SHA-256 |
| 공개키 형식 | SPKI PEM |
| 개인키 형식 | PKCS#8 PEM |
| 공개키 해시 | SHA-256(SPKI DER) 16진수 — 입찰시스템 `BidCrypto.publicKeyHash` 와 같다 |
| 저장 경로 | `keys/{입찰ID}/public.pem`, `keys/{입찰ID}/private.pem`, `keys/history.jsonl` |
| 브랜치 | `main` 고정 |
