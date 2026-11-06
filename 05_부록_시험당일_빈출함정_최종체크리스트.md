# 부록: 시험 당일 핵심 총정리 (30개 챕터 빈출 함정 + 체크리스트)

> **시험 직전 통독용**: 30개 전 챕터의 핵심 포인트와 자주 출제되는 단골 함정, 시험 당일 체크리스트를 한눈에 정리한 요약본입니다.

---

## 1. 챕터별 핵심 정리 + 빈출 함정

### 1과목 — 데이터 모델링의 이해

#### [1-1-1] 데이터 모델의 이해

- 3단계: **개념 → 논리 → 물리**
- 3대 관점: **데이터 / 프로세스 / 상관**
- 3층 스키마: 외부(View) / 개념(ERD) / 내부(저장)
- 좋은 모델 6요소: 완전성·중복배제·업무규칙·재사용·의사소통·통합성

> **⚠ 함정**
> - "물리 모델링이 가장 먼저" → **X** (개념 → 논리 → 물리)
> - "정규화는 물리 단계" → **X** (논리 단계)

#### [1-1-2] 엔터티

- 6대 특징 **업·식·인·프·속·관** — 업무 필요·식별·인스턴스 ≥ 2·프로세스가 이용·속성 ≥ 2·관계
- 유무형 분류: 유형 / 개념 / 사건
- 발생 시점 분류: **기본 → 중심 → 행위**

> **⚠ 함정**
> - 인스턴스 1개도 OK → **X (≥ 2)**
> - 속성 1개도 OK → **X (≥ 2)**

#### [1-1-3] 속성

- 분류: **기본 / 설계 / 파생**
- 구성 분류: 단순 / 복합 / 다치
- 한 컬럼에 한 값(원자성) — 1NF의 기본

> **⚠ 함정**
> - 다치 속성(여러 값) 그대로 OK → **X (1NF 위반)**
> - 파생 속성은 저장하면 안 된다 → 상황에 따라(성능 시 허용)

#### [1-1-4] 관계

- 차수: **1:1 / 1:N / M:N**
- 식별 관계 vs 비식별 관계
- M:N은 분해(연관 엔터티) 필요

> **⚠ 함정**
> - M:N 그대로 RDB 구현 → **X (분해 필수)**

#### [1-1-5] 식별자

- 4대 조건 **유·최·불·존** — 유일성·최소성·불변성·존재성
- 4대 분류: 주/보조, 본질/인조, 단일/복합, 내부/외부
- **PK = UNIQUE + NOT NULL**, 테이블당 1개

> **⚠ 함정**
> - PK 여러 개 가능? → **X (1개)**
> - UNIQUE는 NULL 안 됨? → **X (NULL 허용)**

#### [1-2-1] 정규화

- **1NF → 2NF → 3NF → BCNF**
- **부분종속 = 2NF 위반** / **이행종속 = 3NF 위반**
- 이상현상 3종: 삽입·갱신·삭제 이상
- 반정규화: 의도적 중복 허용(성능 위함)

> **⚠ 함정**
> - BCNF가 3NF보다 약하다 → **X (더 강함)**
> - 정규화는 항상 성능 개선 → **X (조인 늘어 느려질 수 있음)**

#### [1-2-2] 관계와 조인의 이해

- **관계 = FK = JOIN**의 토대
- 필수성(NOT NULL)·식별성(PK 포함 여부)

> **⚠ 함정**
> - FK 없으면 조인 불가 → **X (논리적으론 가능, 권장 X)**

#### [1-2-3] 트랜잭션의 이해

- **ACID** — 원자성 / 일관성 / 고립성 / 지속성
- 모델의 필수성·식별성이 트랜잭션 무결성과 연결

> **⚠ 함정**
> - ACID는 3개 원칙 → **X (4개)**

#### [1-2-4] NULL 속성의 이해

- `= NULL` 안 됨 — **`IS NULL` / `IS NOT NULL`만 가능**
- NULL과 산술 → 결과는 모두 **NULL**
- SQL 3-값 논리: TRUE / FALSE / **UNKNOWN**
- 집계: `COUNT(*)`만 NULL 포함 / 나머지는 NULL 무시
- NULL 처리 함수: `NVL` / `NVL2` / `NULLIF` / `COALESCE`

> **⚠ 함정 (시험 단골)**
> - `NULL = NULL` → **UNKNOWN** (FALSE 아님)
> - `WHERE COL = NULL` → **결과 0건**. `IS NULL` 사용
> - `NULL + 100` → **NULL**
> - `AVG(컬럼)`의 분모는 **NULL 제외 개수** (0 포함 평균과 다름)
> - `NULLIF(A, B)` — A=B면 **NULL**
> - `COALESCE(NULL, NULL, 'X')` → **'X'**

#### [1-2-5] 본질·인조 식별자

- **본질식별자**: 업무 의미가 있는 자연 키 (주민번호, ISBN 등)
- **인조식별자**: 의도적으로 만든 대리키 (시퀀스, GUID)
- 인조키는 가독성↓·중복 위험, 별도 UNIQUE 제약 필요

> **⚠ 함정**
> - 본질식별자가 항상 좋다 → **X (보안·변경 위험 시 인조 선호)**

### 2과목 — SQL 기본 (2-1)

#### [2-1-1] 관계형 DB 개요

- SQL 분류: **DDL / DML / DCL / TCL**
- DDL: CREATE/ALTER/DROP/TRUNCATE/RENAME — **자동 COMMIT**
- DML: INSERT/UPDATE/DELETE/MERGE — **ROLLBACK 가능**
- DCL: GRANT/REVOKE — 자동 COMMIT
- TCL: COMMIT/ROLLBACK/SAVEPOINT

> **⚠ 함정**
> - `TRUNCATE`는 DML이다 → **X (DDL — 자동 COMMIT, ROLLBACK 불가)**
> - `DELETE`는 DDL이다 → **X (DML — ROLLBACK 가능)**

#### [2-1-2] SELECT문

- 작성 순서: **SELECT** - FROM - WHERE - GROUP BY - HAVING - ORDER BY
- 실행 순서: **FROM → WHERE → GROUP → HAVING → SELECT → ORDER** (FWGHSO)
- DUAL: Oracle 시스템 가상 테이블 (1행 1컬럼)
- DISTINCT로 중복 제거

> **⚠ 함정**
> - SELECT가 가장 먼저 실행 → **X (거의 마지막)**
> - SELECT의 별칭(AS)을 WHERE에서 사용 → **X (WHERE는 SELECT보다 먼저 실행)**

#### [2-1-3] 함수

- 단일행 vs 다중행(집계)
- 문자: UPPER/LOWER/SUBSTR/INSTR/LENGTH/LPAD/RPAD/REPLACE/CONCAT(`||`)
- 숫자: ROUND/TRUNC/MOD/CEIL/FLOOR
- 날짜: SYSDATE/ADD_MONTHS/MONTHS_BETWEEN/LAST_DAY/NEXT_DAY
- 변환: TO_CHAR/TO_NUMBER/TO_DATE
- NULL: NVL/NVL2/NULLIF/COALESCE
- 조건: **CASE**(범위 가능) vs **DECODE**(동등비교만)

```sql
-- 문자/숫자/날짜
SELECT SUBSTR('SQLD',1,3)        → 'SQL'
     , LENGTH('양파')             → 2
     , ROUND(123.456, 1)         → 123.5
     , MOD(10, 3)                → 1
     , TO_CHAR(SYSDATE,'YYYY-MM-DD')
FROM DUAL;

-- NULL/조건
NVL(SAL, 0), NVL2(COMM,'있음','없음')
CASE WHEN SAL > 3000 THEN '高' ELSE '低' END
DECODE(JOB, 'CLERK', 1, 'SALES', 2, 0)
```

> **⚠ 함정**
> - DECODE로 BETWEEN 같은 범위 비교 가능 → **X (CASE만 가능)**
> - 날짜 - 날짜 = 날짜 → **X (일수, 숫자)**

#### [2-1-4] WHERE절

- 비교: =, <>, !=, >, <
- 논리: AND > OR (AND 우선)
- SQL 연산자: **BETWEEN**(양 끝 포함) / **IN** / **LIKE**(`_`=1글자, `%`=0+) / **IS NULL**

> **⚠ 함정**
> - BETWEEN 100 AND 200 → 100, 200 **포함**
> - `NOT IN (서브쿼리)`에 NULL 있으면 → **결과 0건** 위험
> - AND/OR 우선순위 헷갈리면 **괄호 사용**

#### [2-1-5] GROUP BY / HAVING

- **WHERE는 집계 전**, **HAVING은 집계 후**
- SELECT의 일반 컬럼은 GROUP BY에 명시 필수 (집계 함수는 예외)
- NULL은 하나의 그룹으로 처리

> **⚠ 함정**
> - HAVING 없이 집계 결과 필터 → **X**
> - SELECT의 일반 컬럼이 GROUP BY 없으면 → **에러**

#### [2-1-6] ORDER BY절

- 실행 순서 마지막. ASC(기본) / DESC
- NULLS FIRST / NULLS LAST 직접 지정 가능
- SELECT의 별칭·컬럼 위치(1, 2, ...) 사용 가능

> **⚠ 함정**
> - Oracle ASC 기본 NULL 위치 → **마지막 (NULLS LAST)**
> - MS-SQL ASC 기본 NULL 위치 → **처음 (NULLS FIRST)**

#### [2-1-7] 조인 (JOIN)

- 5종: **EQUI / Non-EQUI / SELF / OUTER / CROSS**
- EQUI: `=`로 연결 (가장 흔함)
- Non-EQUI: BETWEEN, >, < 등으로 연결
- SELF: 같은 테이블 두 번 조인 (별칭 필수)
- OUTER: LEFT/RIGHT/FULL — 매칭 안 돼도 한쪽 다 포함
- CROSS: 카티션 곱 (조인 조건 없음)

> **⚠ 함정**
> - OUTER JOIN 조건은 **ON에**. WHERE에 두면 NULL 행 제외돼 INNER 효과
> - CROSS JOIN: 조인 조건 빼먹어도 발생 → **의도치 않으면 위험**
> - Oracle `(+)`는 **NULL로 보충될 쪽(없을 수 있는 쪽)**에 붙임

#### [2-1-8] 표준 조인

- INNER / LEFT OUTER / RIGHT OUTER / FULL OUTER / CROSS / NATURAL / USING
- **NATURAL**: 같은 이름·타입 컬럼 자동 조인 (조건 명시 X)
- **USING**: 같은 이름 컬럼 명시
- FULL OUTER: 양쪽 다 (없는 쪽 NULL)

> **⚠ 함정**
> - NATURAL JOIN: 이름 같고 **타입 다르면 에러**
> - MySQL은 FULL OUTER 미지원 → LEFT UNION RIGHT

### 2과목 — SQL 활용 (2-2)

#### [2-2-1] 서브쿼리

- 위치별: 스칼라 / 인라인뷰 / WHERE 서브쿼리
- 행 수별: 단일행(=, <, > 등) / 다중행(IN, ANY, ALL, EXISTS)
- 연관 vs 비연관 서브쿼리

> **⚠ 함정**
> - **ANY = 최소 기준**(중 어느 하나보다 작아도 OK) / **ALL = 최대 기준**(모두보다 커야)
> - `IN (a, b, c)` = OR 연결의 단축
> - 스칼라 서브쿼리는 **1행 1컬럼만 반환**

#### [2-2-2] 집합 연산자

- **UNION**: 합집합 (정렬 + 중복 제거)
- **UNION ALL**: 합집합 (그대로, 빠름)
- **INTERSECT**: 교집합 / **MINUS**(EXCEPT): 차집합
- 컬럼 수·타입 일치 필수

> **⚠ 함정**
> - UNION ALL은 정렬 안 함 → **O (UNION만 정렬)**
> - 컬럼 수 다르면 → **에러**

#### [2-2-3] 그룹 함수

- **ROLLUP(A, B)**: N+1 단계 — (A,B), (A), () 3단계
- **CUBE(A, B)**: 2^N 단계 — 모든 조합 4가지
- **GROUPING SETS**: 지정한 조합만 (가장 유연)
- `GROUPING()` 함수로 소계 행 식별 (1=소계, 0=원본)

```sql
SELECT DEPT, JOB, SUM(SAL)
FROM EMP
GROUP BY ROLLUP(DEPT, JOB);
-- (DEPT,JOB) → (DEPT) → ()  3단계

GROUP BY CUBE(DEPT, JOB);
-- (D,J),(D),(J),()  4단계 (2^2)

GROUP BY GROUPING SETS((DEPT,JOB),(DEPT));
-- 지정한 조합만
```

> **⚠ 함정**
> - ROLLUP(A,B,C) → **4단계** ((A,B,C), (A,B), (A), ())
> - CUBE(A,B,C) → **8단계 (2^3)**

#### [2-2-4] 윈도우 함수

- 순위: **RANK**(1,1,3) / **DENSE_RANK**(1,1,2) / **ROW_NUMBER**(1,2,3)
- 행 순서: LAG / LEAD (앞·뒤 행 값)
- 집계 OVER: SUM/AVG/COUNT OVER PARTITION BY
- WINDOWING: ROWS BETWEEN ... AND ... (행 범위 지정)

```sql
SELECT ENAME, DEPT, SAL,
  RANK()       OVER(PARTITION BY DEPT ORDER BY SAL DESC) R,
  DENSE_RANK() OVER(PARTITION BY DEPT ORDER BY SAL DESC) DR,
  ROW_NUMBER() OVER(ORDER BY SAL DESC) RN,
  LAG(SAL,1)   OVER(ORDER BY HIRE) PREV,
  SUM(SAL)    OVER(PARTITION BY DEPT) DEPT_SUM
FROM EMP;
-- RANK: 1,1,3 / DENSE_RANK: 1,1,2 / ROW_NUMBER: 1,2,3
```

> **⚠ 함정**
> - RANK는 동률 다음 순위 **건너뜀**, DENSE_RANK는 **연속**
> - LAG/LEAD 기본 오프셋은 **1**
> - PARTITION BY 없으면 전체가 한 그룹

#### [2-2-5] Top N 쿼리

- Oracle **ROWNUM**: 의사컬럼, 인라인뷰 + ORDER BY 후 사용
- 표준 **FETCH FIRST** N ROWS ONLY/WITH TIES (Oracle 12c+)
- MS-SQL **TOP** N / TOP N WITH TIES
- 윈도우 **ROW_NUMBER() OVER** + PARTITION BY (그룹별 TopN)

> **⚠ 함정**
> - ROWNUM은 **정렬 전 매겨짐** — 인라인뷰로 정렬 먼저
> - `ROWNUM = N` (N≥2) → **결과 0건**
> - `ROWNUM > N` → **항상 거짓**

#### [2-2-6] 계층형 질의

- `START WITH` ... `CONNECT BY PRIOR`
- **LEVEL** 의사컬럼 (계층 깊이)
- SYS_CONNECT_BY_PATH (경로 출력)
- ORDER SIBLINGS BY (같은 LEVEL 정렬)

> **⚠ 함정**
> - PRIOR 위치가 **부모 결정** (PRIOR 컬럼 = 부모 컬럼)
> - 일반 ORDER BY는 LEVEL 순서를 흩트림 → ORDER SIBLINGS BY 사용

#### [2-2-7] PIVOT / UNPIVOT

- **PIVOT**: 세로 → 가로 변환
- **UNPIVOT**: 가로 → 세로 변환
- PIVOT은 **집계 함수 필수** (SUM, COUNT 등)
- UNPIVOT는 NULL 기본 제외 → `INCLUDE NULLS` 옵션으로 포함

> **⚠ 함정**
> - PIVOT은 집계 없이도 가능 → **X (집계 필수)**
> - UNPIVOT은 NULL 자동 포함 → **X (기본 제외)**

#### [2-2-8] 정규 표현식

- 5함수: REGEXP_LIKE / REPLACE / SUBSTR / INSTR / COUNT
- 메타문자: `.` `*` `+` `?` `[]` `^` `$` `|` `{n}`
- 한글: `[가-힣]`

```sql
-- 한글만 / 휴대폰 형식
WHERE REGEXP_LIKE(NAME, '^[가-힣]+$')
WHERE REGEXP_LIKE(PHONE, '^010-[0-9]{4}-[0-9]{4}$')

-- 숫자 추출
SELECT REGEXP_SUBSTR('A123B45', '[0-9]+') FROM DUAL;
→ '123'

SELECT REGEXP_REPLACE(STR, '[^0-9]', '') FROM ...;
-- 숫자만 남김
```

> **⚠ 함정**
> - Oracle은 **POSIX 1003.2 기반** — `\d`·`\w`·`\s`는 12c 이후 일부 동작
> - 호환 안전: `[0-9]`·`[[:digit:]]` 사용

### 2과목 — 관리 구문 (2-3) · 명령어 분류 포함

#### [2-3-1] DML

- **분류**: INSERT / UPDATE / DELETE / MERGE (SELECT 포함 시 5종)
- ROLLBACK 가능 → TCL로 확정 필요
- MERGE: INSERT + UPDATE 합쳐진 형태 (UPSERT)

```sql
INSERT INTO EMP (ID, NAME, SAL) VALUES (1, '홍길동', 3000);

UPDATE EMP SET SAL = SAL * 1.1 WHERE DEPT = 10;

DELETE FROM EMP WHERE ID = 1;

MERGE INTO TGT USING SRC ON (TGT.ID = SRC.ID)
  WHEN MATCHED THEN UPDATE SET TGT.NAME = SRC.NAME
  WHEN NOT MATCHED THEN INSERT (ID, NAME) VALUES (SRC.ID, SRC.NAME);
```

> **⚠ 함정 (단골)**
> - `DELETE`는 DDL이다 → **X (DML)**
> - DELETE WHERE 빠뜨림 → **전체 삭제 위험** (다만 ROLLBACK 가능)

#### [2-3-2] TCL

- **분류**: COMMIT / ROLLBACK / SAVEPOINT
- SAVEPOINT 후 `ROLLBACK TO SP` = SP 이후 작업만 취소
- 전체 ROLLBACK 또는 COMMIT 시 SAVEPOINT 소멸

```sql
INSERT INTO EMP VALUES (1, 'A');
SAVEPOINT SP1;
UPDATE EMP SET NAME = 'B' WHERE ID = 1;
ROLLBACK TO SP1;   -- UPDATE만 취소 (INSERT는 살아있음)
COMMIT;             -- INSERT 확정
```

> **⚠ 함정**
> - COMMIT 후 ROLLBACK 가능 → **X (불가)**
> - DDL 실행 시 펜딩 트랜잭션 → **자동 COMMIT됨**

#### [2-3-3] DDL

- **분류**: CREATE / ALTER / DROP / TRUNCATE / RENAME
- **자동 COMMIT** — ROLLBACK 불가
- 제약조건: PRIMARY KEY / FOREIGN KEY / UNIQUE / NOT NULL / CHECK / DEFAULT
- PK = UNIQUE + NOT NULL (1개) / UNIQUE는 NULL 허용 (여러 개)

```sql
CREATE TABLE EMP (
  ID      NUMBER PRIMARY KEY,
  NAME    VARCHAR2(20) NOT NULL,
  EMAIL   VARCHAR2(50) UNIQUE,
  SAL     NUMBER DEFAULT 0 CHECK (SAL >= 0),
  DEPT_ID NUMBER REFERENCES DEPT(ID)
);

ALTER TABLE EMP ADD (HIRE_DT DATE);
ALTER TABLE EMP MODIFY (NAME VARCHAR2(30));
ALTER TABLE EMP DROP COLUMN EMAIL;

TRUNCATE TABLE EMP;   -- 전체 삭제, 자동 COMMIT
DROP TABLE EMP;
```

> **⚠ 함정 (단골)**
> - `TRUNCATE`는 DML이다 → **X (DDL — ROLLBACK 불가)**
> - TRUNCATE는 WHERE 가능 → **X (전체 삭제만)**
> - FK는 부모의 일반 컬럼 참조 가능 → **X (PK or UNIQUE만)**

#### [2-3-4] DCL

- **분류**: GRANT / REVOKE
- 시스템 권한: **WITH ADMIN OPTION** (ex: CREATE TABLE)
- 객체 권한: **WITH GRANT OPTION** (ex: SELECT ON EMP)
- ROLE: 권한 묶음. GRANT/REVOKE 한 번에

```sql
-- 객체 권한
GRANT SELECT, INSERT ON EMP TO USER1;
GRANT SELECT ON EMP TO USER1 WITH GRANT OPTION;

-- 시스템 권한
GRANT CREATE TABLE TO USER1 WITH ADMIN OPTION;

-- 회수
REVOKE SELECT ON EMP FROM USER1;

-- ROLE
CREATE ROLE DEV_ROLE;
GRANT SELECT ON EMP TO DEV_ROLE;
GRANT DEV_ROLE TO USER1;
```

> **⚠ 함정**
> - GRANT는 자동 COMMIT 안 됨 → **X (자동 COMMIT)**
> - REVOKE WITH GRANT OPTION → **연쇄 회수** (위임 받은 사용자도 잃음)
> - WITH ADMIN OPTION 회수 → **위임 받은 사용자는 유지** (객체 권한과 차이)

---

## 2. 시험 당일 체크리스트

### 📋 준비물
- **신분증** (필수 — 없으면 응시 불가)
- 수험표 출력본
- 컴퓨터용 사인펜 또는 검정 볼펜 2자루
- 수정테이프 (필요 시)
- 아날로그 시계 (스마트워치 X)

### ⏰ 시간 관리
- 시험 시작 **30분 전 도착**
- 총 **50문항 / 90분** (1문항 평균 1.8분)
- 막히면 일단 ① 표시 후 다음으로
- 마지막 5분 OMR 검토

### 💪 컨디션
- 전날 **충분한 수면** (밤샘 X)
- 아침 가벼운 식사
- 커피·차는 평소 양만
- 화장실 시험 전 미리
- 긴장될 땐 깊게 호흡 3번

### ★ 마음가짐
- 모르는 문제가 보여도 당황 X. 보기만 봐도 답이 좁혀지는 경우 많음.
- 함정 패턴(`NULL`, `ROWNUM`, `ON` vs `WHERE`) 떠올리면서 침착하게.
- 완전히 막히면 패스 후 끝까지 푼 다음 돌아오기.
- 합격 기준 60점(60/100). **킬러 문제 욕심 X**, 풀 수 있는 문제부터.
