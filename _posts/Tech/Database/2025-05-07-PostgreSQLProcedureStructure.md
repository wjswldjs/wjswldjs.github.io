---
title: "PostgreSQL 프로시저 구조 완벽 정리"
date: 2025-05-07 18:00:00 +0900
categories: [Tech, Database]
tags: [sql, postgresql, procedure]
math: true
toc: true
pin: false
---

## 📋 Table of contents
1. [들어가며](#-들어가며)
2. [Create or Replace](#1-create-or-replace는-왜-쓸까)
3. [Parameter](#2-파라미터-모드-in-out-inout)
4. [Parameter String Type](#3-문자열-타입-정리-char-varchar-text)
5. [Security Invoker and Security Definer](#4-security-invoker-vs-security-definer)
6. [AS $$](#5-as---as-procedure의-정체는)
7. [Language](#6-language는-필수)
8. [요약](#7-요약)

<br>

---

## 👀 들어가며
> "순간들을 소중히 여기다보면, 긴 세월은 저절로 흘러간다. - 마리아 에지워스"

<br>

---

PostgreSQL(또는 EDB)에서 프로시저를 작성할 때 꼭 알아야 할 **기본 구조, 키워드 의미, 실무 팁**까지 한눈에 정리해보았습니다.

```sql
CREATE OR REPLACE PROCEDURE sp_test(p_input_1 varchar, OUT return_value integer)
SECURITY INVOKER
AS $procedure$
DECLARE 
  p_input_1 varchar(13);
BEGIN
  -- 비교 로직 구현 예정
  return_value := 1;
END;
$procedure$
LANGUAGE edbspl;
```

## 1. `CREATE OR REPLACE`는 왜 쓸까?

#### ✅ 기능
**프로시저를 새로 만들거나 기존 것을 덮어씀**

* `CREATE`만 쓰면 **이미 존재하는 경우 오류 발생**
* `CREATE OR REPLACE`는 있으면 덮어쓰기, 없으면 새로 생성 👉 유지보수 시 편리

#### 💡 예시

```sql
CREATE OR REPLACE PROCEDURE sp_update_user_status(p_user_id integer)
LANGUAGE plpgsql
AS $$
BEGIN
  -- 사용자 상태 업데이트 로직
END;
$$;
```

* 기존 프로시저가 있어도 그대로 덮어써집니다.

---

## 2. 파라미터 모드: `IN`, `OUT`, `INOUT`

#### ✅ PostgreSQL 파라미터 모드 종류

| 모드      | 설명           | 명시 필요 여부 |
| ------- | ------------ | -------- |
| `IN`    | 입력값 (기본값)    | ❌ 생략 가능  |
| `OUT`   | 출력값          | ✅ 명시 필요  |
| `INOUT` | 입력도 하고 출력도 함 | ✅ 명시 필요  |

#### 💡 예시

```sql
CREATE OR REPLACE PROCEDURE sp_get_user_info(
  p_user_id integer,         -- IN (입력용)
  OUT p_user_name text,      -- OUT (출력용)
  INOUT p_status text        -- INOUT (양방향)
)
LANGUAGE plpgsql
AS $$
BEGIN
  SELECT name INTO p_user_name FROM users WHERE id = p_user_id;
  p_status := 'FOUND';
END;
$$;
```

- `OUT`, `INOUT` 파라미터가 있으면 프로시저 내부에서 값을 `:=`로 설정해주면 외부로 반환됩니다.

---

## 3. 문자열 타입 정리 (`char`, `varchar`, `text`)

#### ✅ 문자열 타입 비교

| 타입           | 설명         | 특징          |
| ------------ | ---------- | ----------- |
| `char(n)`    | 고정 길이 문자열  | 짧으면 공백으로 패딩 |
| `varchar(n)` | 가변 길이 문자열  | 최대 n자       |
| `text`       | 무제한 길이 문자열 | 가장 유연함      |

#### 💡 예시

```sql
DECLARE
  v_code char(3);         -- 항상 3자리, 짧으면 공백
  v_name varchar(50);     -- 최대 50자
  v_desc text;            -- 길이 제한 없음
```

- 대부분의 실무에선 `text`나 `varchar(n)`을 선호하므로 길이 제약이 꼭 필요한 경우에만 `char(n)` 사용

---

## 4. `SECURITY INVOKER` vs `SECURITY DEFINER`

#### ✅ 역할
**프로시저 실행 시 어떤 권한을 사용할지 지정**

| 옵션                 | 실행 권한 기준    | 사용 예시      |
| ------------------ | ----------- | ---------- |
| `SECURITY INVOKER` | 호출자 권한으로 실행 | 기본값        |
| `SECURITY DEFINER` | 작성자 권한으로 실행 | 권한 위임 시 필요 |

#### 💡 예시

```sql
CREATE OR REPLACE FUNCTION sp_get_admin_data()
RETURNS TABLE(id int, name text)
SECURITY DEFINER
LANGUAGE plpgsql
AS $$
BEGIN
  RETURN QUERY SELECT id, name FROM admin_table;
END;
$$;
```

- `SECURITY DEFINER` 사용 시에는 **`SET search_path`를 설정해 보안 문제를 예방**해야 합니다.

---

## 5. `AS $$` / `AS $procedure$`의 정체는?

#### ✅ 역할
**본문(body) 구분자**

* `$$`, `$abc$`, `$procedure$` 등 어떤 이름도 가능 (앞뒤만 일치하면 됨)
* 복잡한 SQL 본문일수록 의미 있는 태그 사용 권장

#### 💡 예시

```sql
AS $procedure$
BEGIN
  -- 프로시저 내용
END;
$procedure$;
```

- `$$`와 완전히 동일하지만, **가독성 향상을 위해** `$procedure$` 등 태그를 붙여줍니다.

---

## 6. `LANGUAGE`는 필수

#### ✅ 역할
**프로시저 구현 언어 지정**

| 언어        | 설명                    |
| --------- | --------------------- |
| `plpgsql` | PostgreSQL 기본 절차형 언어  |
| `edbspl`  | EDB (Oracle 호환 확장 언어) |

#### 💡 예시

```sql
CREATE OR REPLACE PROCEDURE sp_example()
LANGUAGE edbspl
AS $procedure$
BEGIN
  -- EDB SPL 프로시저 로직
END;
$procedure$;
```

⚠️ `LANGUAGE`는 **생략 불가** (에러 발생)

---

## 7. 요약

| 항목      | 실무 추천 방식                                                              |
| ------- | --------------------------------------------------------------------- |
| 프로시저 생성 | `CREATE OR REPLACE`                                                   |
| 파라미터 선언 | `IN`, `OUT`, `INOUT` 명시적으로 사용                                         |
| 문자열 타입  | 제약이 없다면 `text` 또는 `varchar(n)`                                        |
| 권한 제어   | 일반적으로 `SECURITY INVOKER`, 필요 시 `SECURITY DEFINER` + `SET search_path` |
| 본문 구분자  | 가독성을 위해 `$procedure$`, `$body$` 등 사용                                  |
| 언어 지정   | `LANGUAGE plpgsql` 또는 `edbspl` 명시 필수                                  |


