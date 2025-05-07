---
title: "Temp Table"
date: 2025-02-02 18:00:00 +0900
categories: [Tech, Docs]
tags: [data, sql]
math: true
toc: true
pin: false
---

## 📋 Table of contents
1. [들어가며](#-들어가며)
2. [Temp Table이란?](#-temp-table이란)
3. [적용해보기](#-적용해보기)

<br>

---

## 👀 들어가며
> "학문에는 끝이 없다."

<br>

---

## 🤔 Temp Table이란?

### 정의
Temp Table이란, **DB 세션 동안만 존재하여 일시적으로 데이터를 저장할 수 있는 임시 테이블**이다.
일반적으로 쿼리를 실행하는 동안만 존재하며 작업이 끝나면 자동으로 삭제되거나 세션이 종료되면 사라진다.

### 유형
- **Local Temporary Tables**: 현재 세션에서만 접근 가능
- **Global Temporary Tables**: 여러 세션에서 접근 가능 (데이터는 세션별로 독립적)

### 특징
- **일시적 저장**: 임시 테이블은 세션 단위로 존재하며, 세션이 종료되거나 명시적으로 삭제할 때까지 데이터를 저장한다.
- **성능 최적화**: 일반 테이블과 동일한 기능을 제공하기 때문에 대량의 데이터를 처리하는 쿼리나 복잡한 조인 작업에서 임시 테이블을 사용하면 성능을 최적화할 수 있다.
- **디스크 I/O 절약**: 복잡한 계산이나 처리 중간 결과를 임시 테이블에 저장해두면 디스크 I/O를 줄여 성능을 개선할 수 있으며 DB 부하 감소에도 도움이 된다.
> 메모리를 효율적으로 사용하고 트랜잭션 관리가 용이하며 동시성 문제에 대한 우려도 적다는 점이 장점이다. 또한 복잡한 데이터 처리 작업에서 효율적이고 안전하게 일시적인 데이터를 처리하는 데 유용하다.

### 주의 사항
1. 세션 종료 시 자동 삭제되므로 중요 데이터는 영구 테이블에 저장
2. 데이터베이스 엔진별로 문법과 기능이 다를 수 있음
3. 과도한 사용은 시스템 리소스를 소모할 수 있음

<br>

---

## ✅ 적용해보기

### 주요 활용 사례
- 복잡한 쿼리의 중간 결과 저장
- 대량 데이터 처리 시 임시 저장
- 데이터 정제 작업
- 성능 최적화

### 쿼리 예시
```sql
-- PostgreSQL
CREATE TEMP TABLE temp_users (
    id INTEGER,
    name VARCHAR(50)
);

-- Oracle
CREATE GLOBAL TEMPORARY TABLE temp_users (
    id NUMBER,
    name VARCHAR2(50)
);

-- SQL Server
CREATE TABLE #temp_users (
    id INT,
    name VARCHAR(50)
);

```

### 실제 적용
우리 파트에서만 DB에 접근하고 데이터를 처리하는 것이 아니라 타 영역에서도 동시간대에 작업 할 수 있기 때문에 대량의 데이터를 일괄로 처리할 경우 DB Lock이 걸릴 수 있다.
<div style="background-color: #f0f8ff; padding: 10px; border-left: 4px solid #007bff; white-space: pre-line;"><strong>DB Lock이란?</strong>
데이터베이스에서 데이터의 일관성과 무결성을 보장하기 위한 동시성 제어 메커니즘
- 여러 사용자가 동시에 같은 데이터에 접근할 때 발생할 수 있는 문제를 방지
- 데이터의 일관성 유지
- 트랜잭션의 격리성 보장
</div> <br>

이에 복잡한 쿼리 작업에 대하여 단계와 단계 사이의 쿼리 결과를 저장하는 용도로 Temp Table을 사용했다. 
저장한 데이터로 다른 쿼리를 실행하는데 유용하기도 했고 DB에 영향도 적게 줄 수 있었다.

**실제 적용 코드를 간소화 한 예시**

```sql

-- 임시 테이블 생성
CREATE TEMP TABLE temp_cmdt (
    cmdt_id VARCHAR(13),
    temp_ysno CHAR(1)
);

-- 임시 데이터 삽입
INSERT INTO temp_cmdt (cmdt_id, temp_ysno)
SELECT t2.cmdt_id, 'N' AS temp_ysno
  FROM dev.table_1 t1
  LEFT JOIN dev.table_2 t2
    ON t1.sale_id = t2.sale_id
 WHERE t1.cmdt_code IN ('001', '003')
   AND t2.cmdt_id IS NOT NULL;

-- 임시 테이블 사용
SELECT * 
  FROM temp_cmdt;

-- 작업 완료 후 삭제
DROP TABLE temp_cmdt;
```

