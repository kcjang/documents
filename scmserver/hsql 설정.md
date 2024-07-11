

1. **sqltool.rc 파일 설정**: 홈 디렉토리에 `~/.sqltool.rc` 파일을 생성합니다.

   ```bash
   vi ~/.sqltool.rc
   ```

   파일 내용은 다음과 같이 작성합니다:

   ```plaintext
   urlid scmserver
   url jdbc:hsqldb:file:/home/kcjang/.safe/scmserver/hsqldb/scm;sql.syntax_mys=true;crypt_key=BFCA8B0836EB899E3770BED48CFDBDBC;crypt_iv=86AA4265BC4B0CE732090BE3C91E7303;crypt_type=AES/CBC/PKCS5Padding
   username SA
   password 
   ```

2. **sqltool 명령 실행**: 터미널에서 `sqltool`을 실행하여 데이터베이스에 연결합니다. sqltool.jar 파일이 있는 경로를 지정합니다.

   ```bash
   java -classpath /home/kcjang/다운로드/hsqldb-2.7.3/hsqldb/lib/sqltool.jar org.hsqldb.cmdline.SqlTool --rcFile ~/.sqltool.rc scmserver
   ```

   SQL 프롬프트가 나타나면 SQL 쿼리를 입력하여 실행할 수 있습니다.


3. **스크립트로 실행**: 

   ```bash
   java -classpath /home/kcjang/다운로드/hsqldb-2.7.3/hsqldb/lib/sqltool.jar org.hsqldb.cmdline.SqlTool --rcFile ~/.sqltool.rc scmserver < queries.sql
   ```

쿼리 모음

1. **테이블 목록 조회**:

   ```sql
   select table_name from information_schema.tables where table_type = 'BASE TABLE' and table_schema = 'PUBLIC';
   ```

2. **모든 테이블 삭제 스크립트 만들기**:
 
   A. 아래 내용으로 `drop_tables.sql` 파일을 생성합니다. 

   ```bash
   SELECT 'DROP TABLE "' || TABLE_NAME || '";' AS drop_query
   FROM INFORMATION_SCHEMA.TABLES
   WHERE TABLE_TYPE = 'BASE TABLE' AND TABLE_SCHEMA = 'PUBLIC';
   ```
   B. sqltool을 사용하여 스크립트를 실행합니다.

   ```bash
   java -classpath /home/kcjang/다운로드/hsqldb-2.7.3/hsqldb/lib/sqltool.jar org.hsqldb.cmdline.SqlTool --rcFile ~/.sqltool.rc scmserver < drop_tables.sql > drop_commands.sql
   ```   
   C. `drop_commands.sql` 파일을 열어 확인합니다. 아래와 같은 쿼리가 아닌 불필요한 내용이 포함되어 있으므로, 필요한 부분만 저장합니다.
   
   ```bash
   SqlTool v. 6632.
   JDBC Connection established to a HSQL Database Engine v. 2.7.3 database
   as "SA" with R/W TRANSACTION_READ_COMMITTED Isolation.
   SqlFile processor v. 6559.
   Distribution is permitted under the terms of the HSQLDB license.
   (c) 2004-2011 Blaine Simpson and the HSQL Development Group.
   ```
   
   D. sqltool을 사용하여 스크립트를 실행합니다.  

   ```bash
   java -classpath /home/kcjang/다운로드/hsqldb-2.7.3/hsqldb/lib/sqltool.jar org.hsqldb.cmdline.SqlTool --rcFile ~/.sqltool.rc scmserver < drop_commands.sql
   ```
