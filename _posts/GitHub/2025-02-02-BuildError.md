---
title: "Setup Ruby Error"
date: 2025-02-02 01:00:00 +0900
categories: [GitHub, Error]
tags: [blogging, error]
math: false
toc: false
pin: false
---

## ⛔️ 이슈 사항
![이슈](/assets/img/20250202_001.png) 
<br>
블로그 글을 작성하고 main branch에 push 했는데 등록이 안 되어서 Action 탭에 들어가서 보니 build error가 떠 있음

<br>

---

## 🔍 원인 분석
<div style="background-color:rgb(255, 240, 240); padding: 10px; border-left: 4px solid rgb(255, 0, 0); white-space: pre-line;"><strong>오류 메세지</strong>
    build
    The current runner (ubuntu-24.04-x64) was detected as self-hosted because the platform does not match a GitHub-hosted runner image (or that image is deprecated and no longer supported).

    In such a case, you should install Ruby in the $RUNNER_TOOL_CACHE yourself, for example using https://github.com/rbenv/ruby-build

    You can take inspiration from this workflow for more details: https://github.com/ruby/ruby-builder/bloster/.github/workflows/build.yml
    $ ruby-build 3.1.4 /opt/hostedtoolcache/Ruby/3.1.4/x64

    Once that completes successfully, mark it as complete with:
    $ touch /opt/hostedtoolcache/Ruby/3.1.4/x64.complete

    It is your responsibility to ensure installing Ruby like that is not done in parallel.
</div> <br>

Ruby Setup이 제대로 안 된 모양이다..

<br>

---

## ✅ 조치 결과
<div style="background-color:rgb(244, 255, 246); padding: 10px; border-left: 4px solid rgb(64, 191, 95); white-space: pre-line;">오류가 발생한 jekyll.yml 파일 내 Ruby Version을 명시하여 Setup이 제대로 될 수 있도록 함
</div> <br>

#### AS-IS
```yml
- name: Setup Ruby
    uses: ruby/setup-ruby@8575951200e472d5f2d95c625da0c7bec8217c42 # v1.161.0
```

#### TO-BE
```yml
- name: Setup Ruby
    uses: ruby/setup-ruby@v1
```