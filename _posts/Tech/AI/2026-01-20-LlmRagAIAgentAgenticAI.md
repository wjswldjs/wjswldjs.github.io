---
title: " LLM vs RAG vs AI Agent vs Agentic AI"
date: 2026-01-20 21:49:44 +0900
categories:
  - Tech
  - AI
tags:
  - ai
math: true
toc: true
pin: false
---

## 📑 Table of contents 
* TOC
{:toc}

---

## 💬 Intro

> "밥그릇 지키기 프로젝트"

---

## 1. LLM(Large Language Model)
```markdown
Think text prediction, memory (within training), CoT reasoning, few-shot learning. Smart, but passive.
```

LLM은 말 그대로 Large(거대한) Language(언어) Model(모델)이다.

- **Large**: 많은 양의 매개변수와 전 세계의 방대한 데이터를 학습하여 압도적인 지식 규모를 갖춤
- **Language**: 단순 문법을 넘어 인간의 의도, 유머, 코드 등 복잡한 언어 체계를 자유자재로 다룸
- **Model**: 입력된 질문에 대해 학습된 데이터를 바탕으로 가장 확률 높은 답변을 계산해 내는 지능 시스템

LLM(ex. GPT)은 엄청나게 많은 레시피를 머릿속에 외우고 있는 요리사처럼 말도 잘하고 아는 것도 많아서 물어보면 뭐든 답해주지만, 어제 나온 뉴스나 우리 집 냉장고에 뭐가 들어있는지는 모른다.

학습된 데이터 안에서만 답하기 때문에 최신 정보나 개인적인 질문에는 '할루시네이션(거짓말)'을 할 수 있다.


## 2. RAG(Retrieval-Augmented Generation)
```markdown
Now it has external memory. Vector search, hybrid retrieval, grounded answers. Smarter, but still reactive.
```
RAG는 LLM이 가진 한계(최신 정보 부족, 거짓말 등)를 극복하기 위해 '외부에서 정보를 찾아와서(검색), 그 내용을 바탕으로 답변을 보강(증강)하여 생성'한다는 뜻이다.

- **Retrieval (검색 / 찾아오기)**: 질문과 관련된 외부 정보를 데이터베이스에서 먼저 찾아낸다.
- **Augmented (증강 / 덧붙이기)**: 찾아낸 정보를 질문과 합쳐서 AI가 참고할 수 있도록 지식을 보충한다.
- **Generation (생성 / 답변하기)**: 보충된 정보를 바탕으로 정확하고 최신화된 답변을 생성한다.

LLM처럼 요리사에 비유하면 레시피를 책(DB)에서 찾아보는 요리사..?

사용자가 질문하면, 관련 정보가 담긴 문서를 먼저 찾아보고 그 내용을 바탕으로 답변을 생성하기 때문에 "우리 회사 규정 알려줘" 같은 질문에 정확히 답할 수 있게 된다.


## 3. AI Agent
```markdown
Add tools, memory, APIs, task execution, ReAct, planning loops. It’s thinking, planning, and doing.
```
RAG가 레시피 북을 보는 요리사였다면.. AI Agent는 도구(API, 계산기, 검색 엔진 등)를 쓰는 요리사

예를 들어 "내일 날씨에 맞춰서 메뉴 추천하고 재료 주문해줘"라고 시키면, 날씨 앱을 확인하고 쇼핑몰 앱에 접속하는 식이다.

이는 단순히 지식을 전달하는 수준을 넘어 특정 목적을 달성하기 위해 외부 시스템과 상호작용한다.


## 4. Agentic AI
```markdown
The final form. Role delegation. Multi-agent protocols. Autonomous goal planning. Long-term memory. Collaboration across agents.
```
Agentic AI는 단순히 도구를 쓰는 걸 넘어 '추론 과정'이 훨씬 정교해진 상태이다.

한 번에 답을 내는 게 아니라 '계획 수립 -> 실행 -> 결과 검토 -> 수정'의 과정을 **스스로** 반복한다.

"이 답변이 맞나?"라고 스스로 되물으며 오류를 수정하거나, 복잡한 업무를 여러 단계로 쪼개서 해결한다. 

like 주방 상황에 따라 전략을 수정하는 '총괄 셰프'


## 5. AI Agent vs Agentic AI
'AI Agent'와 'Agentic AI'라는 용어는 종종 함께 사용되지만 미묘한 차이가 있다. 
[AI Agent](https://cloud.google.com/discover/what-are-ai-agents)는 Agentic AI의 구성요소입니다. AI Agent는 도구 상자 속의 개별 도구로 Agentic AI는 집 전체를 짓기 위해 이러한 도구를 조정하여 사용하는 것으로 생각할 수 있다.



---

## 🔗 참고 자료

- [LinkedIn 게시물](https://www.linkedin.com/posts/brijpandeyji_llm-rag-ai-agent-agentic-ai-most-people-activity-7352557799333158913-MSFD)

