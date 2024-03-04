---
layout: single
title: "mybatis trim ."
---
## mybatis TRIM 
### prefix : 실행 될 쿼리의 TRIM 문 안에 쿼리 가장 앞에 붙여준다.
```sql
UPDATE board <trim prefix="SET"> username=#{username},password=#{password}</trim>
```
### prefixOverrides : 실행될 쿼리의 TRIM 문 안에 쿼리 가장 앞에 해당하는 문자들이 있으면 자동으로 지워준다.
```sql
SELECT * FROM board WHERE id = #{id} 
<trim prefixOverrides="OR">OR TT LIKE '%' || #{searchContent} || '%' </if> 
```
### suffix : 실행 될 쿼리의 TRIM 문 안에 쿼리 가장 뒤에 붙여준다.
```sql
<trim suffix=")"></trim>
```
### suffixOverrides : 실행될 쿼리의 TRIM 문 안에 쿼리 가장 뒤에 해당하는 문자들이 있으면 자동으로 지워준다.
``` sql
<trim suffixOverrides=","></trim>
```
---
## 쿼리 where 1=1 대신 trim 사용
```sql
SELECT id, username, nickname
FROM member_test
WHERE
    1=1
    <if test="username != null">
        AND username = #{username}
    </if>
    <if test="nickname != null">
        AND nickname = #{nickname}
    </if>
```
```sql
DELETE FROM member_test
WHERE
    1=1
    <if test="username != null">
        AND username = #{username}
    </if>
    <if test="nickname != null">
        AND nickname = #{nickname}
    </if>
```
###  * 위 예제에서 username , nickname 이 null일 경우 각각 전체 조회, 전체 삭제가 발생

## where 전체 1=1 을 피하는 방법
  
  ### 1. where 사용

  ```sql
  SELECT id, username, nickname
FROM member
<where>
    <if test="username != null">AND username = #{username} </if>
    <if test="nickname != null">AND nickname = #{nickname} </if>
</where>
```
### 2. trim 사용(1)
```sql
SELECT id, username, nickname
FROM member
<trim prefix="WHERE" prefixOverrides="AND">
    <if test="username != null">
        AND username = #{username}
    </if>
    <if test="nickname != null">
        AND nickname = #{nickname}
    </if>
</trim>
```
prefix --> 해당 블록 안에 WHERE 을 추가  
prefixoverrides --> 태그 안에서 실행 될 쿼리의 가장 앞 쿼리가 속성값에 설정해둔 문자와 동일한 경우 문자를 제거해 줍니다.  
-> trim 결과
```sql
SELECT id, username, nickname
FROM member_test
```

### 2. trim 사용(2)
```sql
SELECT id, username, nickname
FROM member_test
WHERE
<trim prefixOverrides="AND">
    <if test="username != null">
        AND username = #{username}
    </if>
    <if test="nickname != null">
        AND nickname = #{nickname}
    </if>
</trim>
```
-> trim 결과  
```sql
SELECT id, username, nickname
FROM member_test WHERE
```
### ==> null 값이 들어올 경우 문법 오류로 exception이 발생 하므로 데이터 전체 조회 방지 가능



