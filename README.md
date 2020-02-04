# 2020/01/31 ~
## 트렌젝션 : 데이터베이스의 상태를 변경해주는 것 !!외우자!!
> 2020/01/31
## Database 시작하기
+
  + 관리자 계정
    + 데이터베이스의 생성과 관리를 담당하는 슈퍼 유저 계정  
    오브젝트의 생성, 변경, 삭제 등의 작업이 가능하며  
    데이터베이스에 대한 모든권한과 책임을 가지는 계정
  + 사용자 계정
    + 데이터베이스에 대하여 질의(Query), 갱신, 보고서를 작성 등의 작업을 수행할 수 있는 계정  
    일반 계정은 보안을 위해 최소한의 필요한 권한만 가지는 것을 원칙으로 함
    
  ```SQL
    USER KH IDENTIFIED BY KH;
    -- 계정이름         비밀번호
       
    GRANT RESOURCE, CONNECT TO KH;
    -- 계정에 권한을 부여하는 
  ```

> 2020/02/03
### 
+ 행, 튜플
  + 테이블에는 기본적으로 **기본키**의 역할을 하는 컬럼이 필요하다. 
  + 각 행(튜플)을 구분할 수 있는 **구분자**이다. OR **고유값**
  + 무조건 테이블에는 한 개이상의 **기본키**가 존재해야한다.
+ 컬럼, 도메인
+ 외래키(Foreign Key)
  + 한 테이블의 기본키, 일반컬럼이 다른 테이블의 컬럼 안으로 들어가면 그 컬럼은 **외래키**가 된다.
>
### SQL
+ DQL(Data Query Language) - 데이터 검색
  + SELECT
>
+ DML(Date Manipulation Language) - 데이터 조작
  + INSERT : 삽입
  + UPDATE : 수정
  + DELETE : 삭제
>  
+ DDL(Date Definition Language) - 데이터 정의
  + CREATE : 생성
  + DROP : 삭제
  + ALTER : 수정
>
+ TCL(Transaction Control Language) - 트랜잭션 제어
  + COMMIT : 데이터베이스의 상태 변경을 확정지어준다.
  + ROLLBACK : 마지막 커밋 지점으로 되돌린다.
>
### DQL - SELECT
+ 데이터를 조회한 결과를 Result Set이라고 하는데 SELECT구문에 의해 반환된 행들의 집합을 의미함
+ Result Set은 0개 이상의 행이 포함될 수 있고 특정 기준에 의해 정렬 가능
+ 한(여러) 테이블의 특정 컬럼, 행, 행/컬럼 조회 가능


  + 작성법
  ```sql
    SELECT 컬럼명[,컬럼명, ...]  // 조회하고자 하는 컬럼명 기술
    FROM 테이블명 // 조회 대상 컬럼이 포함된 테이블명
    WHERE 조건식; // 행을 선택하는 조건 기술
  ```
    + SELECT 예시
    ```sql
      -- 직원 전부의 모든 정보를 조회하는 구문
      SELECT * FROM EMPLOYEE
      
      -- 컬럼 값에 대해 산술 연산한 결과 조회 
      SELECT EMP_NAME, SALARY * 12, (SALARY + (SALARY*BONUS)) * 12
      FROM EMPLOYEE;
      
      -- *(아스트로)는 혼자사용해야 한다.
      SELECT *, SALARY * 12 
      -- 2개의 * 가 들어갔다. 허용되지 않아 에러가 발생된다. 
      -- 전체를 뽑거나 곱하기를 하거나 둘 중 하나만 해야한다.
      FROM EMPLOYEE;
      
      -- 반올림, 올림, 내림, 버림 사용하기
      SELECT SYSDATE - HIRE_DATE 근무일수, ROUND(SYSDATE-HIRE_DATE) 반올림, CEIL(SYSDATE-HIRE_DATE) 올림,
              FLOOR(SYSDATE - HIRE_DATE) 내림, TRUNC(SYSDATE - HIRE_DATE) 버림
      FROM EMPLOYEE;      
      
      -- 컬럼 별징
      -- 컬럼명 AS 별칭
      -- 컬럼명 "별칭" // 별칭에 띄어쓰기, 특수문자, 숫자가 포함될 경우 무조건 ""으로 묶는다.
      -- 컬럼명 AS "별칭" 
      -- 컬럼명 별칭
      
      -- EMPLOYEE테이블에서 직원의 직원명(별칭 : 이름), 연봉(별칭 : 연봉(원)), 보너스를 추가한 연봉(별칭 : 총소득(원)) 조회
      SELECT EMP_NAME 이름, SALARY * 12 "연봉(원)", (SALARY * (1+BONUS))*12 AS "총소득(원)"
      FROM EMPLOYEE;

      -- EMPLOYEE테이블에서 이름, 고용일, 근무일수(오늘날짜 - 고용일) 조회
      SELECT EMP_NAME AS "이름", HIRE_DATE AS "고용일", SYSDATE-HIRE_DATE "근무일수"
      FROM EMPLOYEE;
    ```
+
  + 컬럼명 "별칭" // 별칭에 띄어쓰기, 특수문자, 숫자가 포함될 경우 무조건 ""으로 묶는다.
  + 가급적 ""(더블 커텐션)으로 묶도록 연습하자.!

> 2020/02/04
### DQL - SELECT
+ 위에 SELECT를 이어서 작성해보자..!
+ 
  + 리터럴
    + 임의로 정한 문자열을 SELECT절에 사용하면 테이블에 존재하는 데이터처럼 사용 가능  
  문자나 날짜 리터럴은 ' ' 기호가 사용되며 모든 행에 반복 표시됨
  
  ```SQL
    SELECT EMP_ID "직원번호", EMP_NAME "사원명", SALARY "급여",'원' "단위"
    FROM EMPLOYEE;
  ```
  
  + DISTINCT
    + 컬럼에 포함된 중복 값을 한 번씩만 표기하고자 할 때 사용  
    **SELECT절에 딱 한 번만 쓸 수 있고 맨 앞에 써야한다.**
  ```SQL
    -- EMPLOYEE테이블에서 직원의 직급코드를 중복제거 하여 조회
    SELECT DISTINCT JOB_CODE
    FROM EMPLOYEE;
    
    -- EMPLOYEE테이블에서 부서코드와 직급코드를 중복제거 하여 조회
    SELECT DISTINCT DEPT_CODE, DISTINCT JOB_CODE
    FROM EMPLOYEE;
    -- 에러가 발생한다.!! DISTINCT는 하나만 써야한다.!!
    
    -- 올바른 사용법
    SELECT DISTINCT DEPT_CODE, JOB_CODE
    FROM EMPLOYEE;
  ```
>
### DQL - WHERE
+ WHERE절이란?
  + SELECT에 걸리는 조건문이 들어가는 절  
  조회할 행들에서 조건이 맞는 값만 가진 행을 골라냄
>
+
  + 연결연산자
    + '||'를 사용하여 여러 컬럼을 하나의 컬럼인 것 처럼 연결하거나 컬럼과 리터럴을 연결함.
    >

  + 논리 연산자
    + 여러 개의 제한 조건 결과를 하나의 논리 결과로 만들어줌  
    
    >
    |연산자|설명|
    |:------:|:---:|
    |AND|여러 조건이 동시에 TRUE일 경우에만 TRUE값 반환|
    |OR|여러 조건들 중에 어느 하나의 조건만 TRUE이면 TRUE값 반환|
    |NOT|조건에 대한 반대 값으로 반환(NULL 제외)|
 
  + 비교연산자  
  
      |연산자|설명|
      |:------:|:---:|
      |=|같다|
      |>,<|크다/작다|
      |>= , =< |크거나 같다 / 작거나 같다|
      |<> , != , ^=|같지 않다|
      |BETWEEN AND|특정 범위에 포함되는지 비교|
      |LIKE / NOT LIKE|문자 패턴 비교|
      |IS NULL / IS NOT NULL|NULL 여부 비교|
      |IN / NOT IN|비교 값 목록에 포함/미포함 되는지 여부 비교|

  
### 함수
+
  + 자바에서의 메소드를 의미 
  + 하나의 큰 프로그램에서 반복적으로 사용되는 부분들을 분리하여 작성해 놓은 작은 서브 프로그램
  + 호출하며 값을 전달하면 결과를 리턴하는 방식으로 사용
    + 단일 행 함수 : 각 행마다 반복적으로 적용되어 입력 받은 행의 개수만큼 결과 반환
    + 그룹 함수 : 특정 행들의 집합으로 그룹이 형성되어 적용됨, 그룹 당 1개의 결과 반환
