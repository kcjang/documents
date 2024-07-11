1. 다음 쿼리를 실행해서 USER_SCM 과 관련된 constraint_name 을 확인한다. 

```sql
SELECT
    rc.CONSTRAINT_NAME,
    rc.UNIQUE_CONSTRAINT_NAME,
    kcu1.TABLE_NAME AS referencing_table,
    kcu1.COLUMN_NAME AS referencing_column,
    kcu2.TABLE_NAME AS referenced_table,
kcu2.COLUMN_NAME AS referenced_column
FROM
    INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS AS rc
    JOIN
        INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS kcu1
        ON
        rc.CONSTRAINT_NAME = kcu1.CONSTRAINT_NAME
        JOIN
        INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS kcu2
        ON
        rc.UNIQUE_CONSTRAINT_NAME = kcu2.CONSTRAINT_NAME
WHERE
    kcu2.TABLE_NAME = 'USER_SCM';

```

2. Constraint_name 에 `[REFERENCING_TABLE]`, `[ CONSTRAINT_NAME]` 을 참고해서 다음 쿼리를 실행한다.

```sql
ALTER TABLE [ CONSTRAINT_NAME] DROP CONSTRAINT [REFERENCING_TABLE];
Alter table AGENT drop constraint FKPOLC9T0H7TJNGA8TR44VM95UD;
Alter table FORTIFY_PROJECT drop constraint FKSCSNDGQWJ4XIUNFEFYS79TMD2;
Alter table FORTIFY_RUN drop constraint FKQS0666DUUB4YQLI0ELD346786;
Alter table NOTICE drop constraint FK3B2X7FL78KD3MF8PUNABJVDKC;
Alter table USER_LOGIN_KEY drop constraint FKFB4TJ720D1TQCSH18NNRFU6YF;
Alter table USER_PASSWORD drop constraint FK2FDUOCPCS40LO24B5IRLJ6IL0;
Drop table USER_SCM;
```
3. 이후에 USER 테이블을 USER_SCM 테이블로 이름을 변경한다.
```sql
ALTER TABLE USER RENAME TO USER_SCM;

alter table agent drop column user_code;
ALTER TABLE AGENT ALTER COLUMN "USER" RENAME TO USER_CODE;

alter table fortify_project drop column user_code;
ALTER TABLE FORTIFY_PROJECT ALTER COLUMN "USER" RENAME TO USER_CODE;

alter table fortify_run drop column user_code;
ALTER TABLE FORTIFY_RUN ALTER COLUMN "USER" RENAME TO USER_CODE;

alter table user_login_key drop column user_code;
ALTER TABLE USER_LOGIN_KEY ALTER COLUMN "USER" RENAME TO USER_CODE;

alter table USER_PASSWORD drop column user_code;
ALTER TABLE USER_PASSWORD ALTER COLUMN "USER" RENAME TO USER_CODE;
```


4. 다음 쿼리를 실행해서 USER_SCM 에 대한 FK Constraint 가 제대로 설정되었는지 확인한다.


```sql
SELECT
    rc.CONSTRAINT_NAME,
    rc.UNIQUE_CONSTRAINT_NAME,
    kcu1.TABLE_NAME AS referencing_table,
    kcu1.COLUMN_NAME AS referencing_column,
    kcu2.TABLE_NAME AS referenced_table,
    kcu2.COLUMN_NAME AS referenced_column
FROM
    INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS AS rc
    JOIN
    INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS kcu1
    ON
    rc.CONSTRAINT_NAME = kcu1.CONSTRAINT_NAME
    JOIN
    INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS kcu2
    ON
    rc.UNIQUE_CONSTRAINT_NAME = kcu2.CONSTRAINT_NAME
WHERE
    kcu2.TABLE_NAME = 'USER_SCM'; 
```
 

 

 

 