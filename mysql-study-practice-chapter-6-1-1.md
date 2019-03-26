# 6.1.1 원하는 데이터를 가져와 주는 기본적인 SELECT ... FROM

## 189p 실습1 
1. mysql 로 접속
```
vagrant@kickgoing-server:~/code$ mysql -u homestead -p
Enter password: secret
```

2. database 명령어
```
MariaDB [(none)]> SHOW DATABASES;
MariaDB [(none)]> CREATE DATABASE IF NOT EXISTS employees;
MariaDB [(none)]> USE employees;
```

3. table(employees, titles) 생성
```
MariaDB [employees]> CREATE TABLE employees (
    emp_no      INT             NOT NULL,
    birth_date  DATE            NOT NULL,
    first_name  VARCHAR(14)     NOT NULL,
    last_name   VARCHAR(16)     NOT NULL,
    gender      ENUM ('M','F')  NOT NULL,    
    hire_date   DATE            NOT NULL,
    PRIMARY KEY (emp_no)
);

MariaDB [employees]> CREATE TABLE titles (
    emp_no      INT             NOT NULL,
    title       VARCHAR(50)     NOT NULL,
    from_date   DATE            NOT NULL,
    to_date     DATE,
    KEY         (emp_no),
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no) ON DELETE CASCADE,
    PRIMARY KEY (emp_no,title, from_date)
); 
```

4. 기타 명령어
```
MariaDB [employees]> SHOW TABLE STATUS;
-- 현재 데이터베이스에 있는 테이블의 정보 조회

MariaDB [employees]> SHOW TABLES;
-- 현재 데이터베이스에 있는 테이블의 이름 조회

MariaDB [employees]> DESCRIBE employees;
MariaDB [employees]> DESC employees;
-- 테이블의 열 조회
```

5. employees 테이블에 데이터 삽입 후 조회
```
INSERT INTO `employees` VALUES (10001,'1953-09-02','Georgi','Facello','M','1986-06-26'),(10002,'1964-06-02','Bezalel','Simmel','F','1985-11-21'),(10003,'1959-12-03','Parto','Bamford','M','1986-08-28'),(10004,'1954-05-01','Chirstian','Koblick','M','1986-12-01'),(10005,'1955-01-21','Kyoichi','Maliniak','M','1989-09-12'),(10006,'1953-04-20','Anneke','Preusig','F','1989-06-02'),(10007,'1957-05-23','Tzvetan','Zielinski','F','1989-02-10'),(10008,'1958-02-19','Saniya','Kalloufi','M','1994-09-15'),(10009,'1952-04-19','Sumant','Peac','F','1985-02-18'),(10010,'1963-06-01','Duangkaew','Piveteau','F','1989-08-24');

SELECT first_name, gender FROM employees;
-- 일부 컬럼만 조회

SELECT first_name AS 이름 , gender 성별, hire_date '회사 입사일'
FROM employees;
-- 컬럼에 별칭을 주어서 조회
```

## 193p 실습2 
```
DROP DATABASE IF EXISTS sqlDB; -- 만약 sqlDB가 존재하면 우선 삭제한다.
CREATE DATABASE sqlDB;

USE sqlDB;

CREATE TABLE userTbl -- 회원 테이블
( userID  	    CHAR(8) NOT NULL PRIMARY KEY, -- 사용자 아이디(PK)
  name    	    VARCHAR(10) NOT NULL, -- 이름
  birthYear     INT NOT NULL,  -- 출생년도
  addr	  	    CHAR(2) NOT NULL, -- 지역(경기,서울,경남 식으로 2글자만입력)
  mobile1	    CHAR(3), -- 휴대폰의 국번(011, 016, 017, 018, 019, 010 등)
  mobile2	    CHAR(8), -- 휴대폰의 나머지 전화번호(하이픈제외)
  height    	SMALLINT,  -- 키
  mDate    	    DATE  -- 회원 가입일
) DEFAULT CHARSET=utf8;

CREATE TABLE buyTbl -- 회원 구매 테이블
(  num 		    INT AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 순번(PK)
   userID  	    CHAR(8) NOT NULL, -- 아이디(FK)
   prodName 	CHAR(6) NOT NULL, --  물품명
   groupName 	CHAR(4)  , -- 분류
   price     	INT  NOT NULL, -- 단가
   amount    	SMALLINT  NOT NULL, -- 수량
   FOREIGN KEY (userID) REFERENCES userTbl(userID)
) DEFAULT CHARSET=utf8;

INSERT INTO userTbl VALUES('LSG', '이승기', 1987, '서울', '011', '1111111', 182, '2008-8-8');
INSERT INTO userTbl VALUES('KBS', '김범수', 1979, '경남', '011', '2222222', 173, '2012-4-4');
INSERT INTO userTbl VALUES('KKH', '김경호', 1971, '전남', '019', '3333333', 177, '2007-7-7');
INSERT INTO userTbl VALUES('JYP', '조용필', 1950, '경기', '011', '4444444', 166, '2009-4-4');
INSERT INTO userTbl VALUES('SSK', '성시경', 1979, '서울', NULL  , NULL      , 186, '2013-12-12');
INSERT INTO userTbl VALUES('LJB', '임재범', 1963, '서울', '016', '6666666', 182, '2009-9-9');
INSERT INTO userTbl VALUES('YJS', '윤종신', 1969, '경남', NULL  , NULL      , 170, '2005-5-5');
INSERT INTO userTbl VALUES('EJW', '은지원', 1972, '경북', '011', '8888888', 174, '2014-3-3');
INSERT INTO userTbl VALUES('JKW', '조관우', 1965, '경기', '018', '9999999', 172, '2010-10-10');
INSERT INTO userTbl VALUES('BBK', '바비킴', 1973, '서울', '010', '0000000', 176, '2013-5-5');
INSERT INTO buyTbl VALUES(NULL, 'KBS', '운동화', NULL   , 30,   2);
INSERT INTO buyTbl VALUES(NULL, 'KBS', '노트북', '전자', 1000, 1);
INSERT INTO buyTbl VALUES(NULL, 'JYP', '모니터', '전자', 200,  1);
INSERT INTO buyTbl VALUES(NULL, 'BBK', '모니터', '전자', 200,  5);
INSERT INTO buyTbl VALUES(NULL, 'KBS', '청바지', '의류', 50,   3);
INSERT INTO buyTbl VALUES(NULL, 'BBK', '메모리', '전자', 80,  10);
INSERT INTO buyTbl VALUES(NULL, 'SSK', '책'    , '서적', 15,   5);
INSERT INTO buyTbl VALUES(NULL, 'EJW', '책'    , '서적', 15,   2);
INSERT INTO buyTbl VALUES(NULL, 'EJW', '청바지', '의류', 50,   1);
INSERT INTO buyTbl VALUES(NULL, 'BBK', '운동화', NULL   , 30,   2);
INSERT INTO buyTbl VALUES(NULL, 'EJW', '책'    , '서적', 15,   1);
INSERT INTO buyTbl VALUES(NULL, 'BBK', '운동화', NULL   , 30,   2);

INSERT INTO buyTbl(userID, prodName, groupName, price, amount) VALUES('BBK', '운동화', NULL   , 30,   2);
-- AUTO_INCREMENT 로 설정된 컬럼을 NULL 로 하지 않을 때에는 테이블명 뒤에 컬럼명을 넣어서 삽입

SELECT * FROM userTbl;
SELECT * FROM buyTbl;
```

## 데이터 공간 할당 
- MySQL 5.7 은 CHAR, VARCHAR 모두 UTF-8 코드를 사용 -> 영문/숫자/기호 1Byte, 한글/중국어/일본어 3Byte
- CHAR(10) 설정 시 영문이든 한글이든 10글자까지 입력 가능, 내부적으로는 MySQL이 공간 할당하므로 사용자는 신경쓰지 않아도 됨
- 일부 DBMS 에서는 영문자를 입력할 계획일 경우 1Byte 할당하는 CHAR와 VARCHAR 사용하고, 한글이면 유니코드 기준 2Byte 할당하는 NCHAR, NVARCHAR 사용

## 데이터베이스 개체
- 데이터베이스, 테이블, 인덱스, 열, 뷰, 트리거, 스토어드 프로시저 등과 같은 개체
- 식별자(Identifier): 데이터베이스(=스키마) 개체의 이름 
- 식별자 정의 규칙
    - 알파벳 a~z, A~Z, 0~9, $, _
    - 대문자, 소문자 어떤 것을 사용해도 소문자로 생성 
    - 개체 이름은 최대 64자로 제한
    - 예약어 사용 제한  
        예) CREATE TABLE select ... (X)
    - 원칙적으로 중간에 공백이 있으면 안되지만, 중간에 공백 사용하려면 `(백틱)으로 묶어야 함    
        예) CREATE TABLE `My Table`(...)
    - 되도록 간결하고 파악하기 쉽게 주는 것이 좋음  
        예) CREATE TABLE abc ... (X, 어떤 테이블인지 의미 파악할 수 없음)
        예) CREATE TABLE sales(`Price of Production` int, ...) (X, 열 이름이 의미 파악은 쉽지만 긺)

## 참고
- AUTO_INCREMENT 로 설정한 컬럼은 NULL로 값을 지정
- TYPE이 TIMESTAMP 일 때 NULL 로 지정 시 SELECT CURRENT_TIMESTAMP(); 했을 때의 값이 DEFAULT로 설정
- 한글 입력 시 에러(```Incorrect string value: '\xEC\x98\xAC \xEA\xB0...' for column 'title' at row 1```) 발생하여 테이블 생성할 때 ```DEFAULT CHARSET=utf8``` 해야 함