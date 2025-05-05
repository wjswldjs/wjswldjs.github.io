---
title: "GitHub 블로그에 댓글 기능 달기 (Utterances 활용)"
date: 2025-05-05 10:00:00 +/-TTTT
categories: [GitHub, Log]
tags: [blogging, log]
math: false
toc: true
pin: false
image:
  path: /assets/img/202505/20250505_011.png
  alt: "Thumbnail"
---

내가 쓴 글에 댓글을 달 수 있었으면 좋겠다는 생각에, GitHub 기반 블로그에서 댓글 기능을 구현해보았습니다.

## 1. 목표
* 블로그 게시글에 **댓글 작성 기능** 추가
* **댓글 내용을 GitHub Issue로 관리**

## 2. 구현 순서
### 1️⃣ 댓글용 GitHub Repository 만들기

댓글을 따로 관리할 수 있도록 **전용 레포지토리**를 생성합니다.

> `Issues` 탭이 활성화되어 있으면 준비 완료입니다

![issue](/assets/img/202505/20250505_003.png)

### 2️⃣ Utterances 설치

[Utterances 공식 사이트](https://utteranc.es/)에 접속하여 아래 과정을 따릅니다

1. 페이지 중간쯤에 나오는 `utterances app` 링크 클릭
![utterances](/assets/img/202505/20250505_004.png)
2. Install 버튼 클릭
3. Only select repositories 선택 후, 아까 만든 댓글용 레포 선택
4. 다시 Install

설치가 완료되면 다시 Utterances 메인으로 돌아옵니다.

### 3️⃣ Script 생성 및 삽입

1. **owner/repo 형식으로 댓글용 저장소 입력**
예: `wjswldjs/wjswldjs.github.io.comment`
![repo](/assets/img/202505/20250505_005.png)

2. 하단 스크립트 복사
![script](/assets/img/202505/20250505_006.png)

3. 복사한 스크립트를 블로그 테마 파일 `/_layouts/post.html`의 가장 하단에 붙여넣기

```html
  ...
  </div>
  <!-- div.post-tail-wrapper -->

  <script
    src="https://utteranc.es/client.js"
    repo="wjswldjs/wjswldjs.github.io.comment"
    issue-term="pathname"
    theme="github-light"
    crossorigin="anonymous"
    async
  ></script>
</article>
```

## 3. 결과

정상적으로 적용되면 블로그 게시글 하단에 댓글 창이 생깁니다 ✨

![결과](/assets/img/202505/20250505_007.png)

실제로 댓글을 달아보면 GitHub의 Issue 탭에서 게시글별로 댓글이 잘 정리되어 있는 걸 확인할 수 있습니다.

![댓글](/assets/img/202505/20250505_008.png)
![issueTab](/assets/img/202505/20250505_009.png)
![issue](/assets/img/202505/20250505_010.png)

## 4. 정리

* Utterances를 이용하면 **GitHub 기반 Blog 댓글 시스템**을 간편하게 붙일 수 있다.
* 별도의 DB 없이도 **GitHub 저장소로 댓글을 관리**할 수 있다는 점이 큰 장점인 것 같다.
