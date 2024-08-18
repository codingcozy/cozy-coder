---
title: "도커에서 스프링 부트 애플리케이션에 여러 개의 데이터베이스 연결하는 방법"
description: ""
coverImage: "/assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_0.png"
date: 2024-08-18 11:22
ogImage: 
  url: /assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_0.png
tag: Tech
originalTitle: "Connecting Multiple DBs to Spring Boot Application Running on Docker"
link: "https://medium.com/@sehgal.mohit06/connecting-multiple-dbs-to-spring-boot-application-running-on-docker-83a2f9deb19c"
isUpdated: true
updatedAt: 1723951113163
---

이 글에서는 다음을 수행할 것입니다:

- 도커 컨테이너에서 POSTGRESQL 실행하기.
- 도커 컨테이너에서 MYSQL 실행하기.
- 도커 컨테이너에서 스프링 부트 애플리케이션 실행하기.
- 모든 데이터베이스 및 스프링 부트 애플리케이션을 연결한 네트워크 생성하기.

시작해보죠:

Docker Desktop이 실행 중인지 확인해주세요.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- 도커 컨테이너에 데이터베이스 설정:

- 포스트그레스 설정:

```js
 docker run --name db-postgres -e POSTGRES_PASSWORD=root -p 5432:5432 -d postgres
 docker exec -it db-postgres psql -U postgres
 CREATE DATABASE testme
```

해당 명령어는 이름이 "db-postgres"인 컨테이너를 생성하고, 사용자 이름이 postgres이며 암호가 root인 데이터베이스를 생성합니다. 그런 다음 exec 명령어를 사용하여 해당 컨테이너에 들어가 포스트그레스를 엽니다. 그리고 "testme"라는 이름의 데이터베이스를 생성합니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- MySql 설정:

```js
docker run --name mysqlDb2 -e MYSQL_ROOT_PASSWORD=root -p 3307:3306 -d mysql:latest
docker exec -it mysqlDb2 sh
mysql -u root -p
create database mysqlDb1;
```

위 명령어들은 이름이 mysqlDb2이고 사용자 이름이 root이며 비밀번호가 root인 도커 컨테이너를 생성합니다. 그런 다음 exec 명령을 사용하여 도커 컨테이너 내부에 쉘을 엽니다. 그런 다음 위의 마지막 명령을 사용하여 mysql에 로그인합니다. 마지막으로 "mysqlDb1"이라는 이름의 데이터베이스를 생성합니다.

아래 명령을 실행하여 "db_network"라는 네트워크를 생성합니다:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
docker network create db_network
```

다음으로 데이터베이스를이 네트워크에 연결하십시오:

```js
docker network connect db_network mysqlDb2
docker network connect db_network db-postgres
```

이렇게하면 데이터베이스가 가동 중입니다:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

테이블 태그를 마크다운 형식으로 변경해주세요.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

저는 두 개의 패키지를 만들었습니다: primary와 secondary입니다. 두 개의 데이터베이스(mysql과 postgres)에 연결할 것입니다. Mysql은 주 데이터베이스로 표시되고, postgres는 보조 데이터베이스로 지정될 것입니다. 어플리케이션이 실행될 때, mysql에 "Students" 테이블이 자동으로 생성되고, postgres에는 "Teachers" 테이블이 생성될 것입니다. 모든 설정은 아래에서 설명합니다:

2.1 학생 엔티티 & 레포지토리(Primary 패키지):

```js
package com.multidbdemo.app.config.primary;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
@Table(name = "students")
public class Students {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    @Column(name="st_name")
    private String name;
    @Column(name="st_roll")
    private Integer rollNo;

}
```

```js
package com.multidbdemo.app.config.primary;

import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentsRepository extends JpaRepository<Students,Long> {
}
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

2.2 선생님 Entity 및 Repository(보조 패키지):

```js
package com.multidbdemo.app.config.secondary;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
@Table(name = "teachers")
public class Teachers {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    @Column(name="t_name")
    private String name;
    @Column(name="t_sub")
    private String subject;

}
```

```js
package com.multidbdemo.app.config.secondary;

import org.springframework.data.jpa.repository.JpaRepository;

public interface TeachersRepository extends JpaRepository<Teachers, Long> {
}
```

2.3 주요 데이터베이스 및 보조 데이터베이스 구성:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
패키지 com.multidbdemo.app.config;

import jakarta.persistence.EntityManagerFactory;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.context.annotation.Profile;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        basePackages = {"com.multidbdemo.app.config.primary"},
        transactionManagerRef = "primaryTransactionManager",
        entityManagerFactoryRef = "primaryEntityManagerFactory"
)
@Profile("!test")
public class PrimaryDbConfig {

    @Bean(name="db1")
    @Primary
    @ConfigurationProperties(prefix = "spring.db")
    public DataSource dataSource(){
        return DataSourceBuilder.create().build();
    }

    @Bean("primaryEntityManagerFactory")
    @Primary
    public LocalContainerEntityManagerFactoryBean getEntityManagerBean(EntityManagerFactoryBuilder builder, @Qualifier("db1") DataSource dataSource){
        HashMap<String,String> prop = new HashMap<>();
        prop.put("hibernate.dialect","org.hibernate.dialect.MySQLDialect");
        return builder.dataSource(dataSource)
                .properties(prop)
                .packages("com.multidbdemo.app.config.primary").build();
    }

    @Bean("primaryTransactionManager")
    public PlatformTransactionManager getTransactionManager(@Qualifier("primaryEntityManagerFactory") EntityManagerFactory factory){
        return new JpaTransactionManager(factory);
    }
}
```

클래스가 @Profile(“!test”)로 표시되어 있어서 테스트를 실행할 때 이 구성이 활성화되지 않도록 한점에 유의해주세요. 빌드하는 동안 테스트가 실행되고 나서 jar 파일이 생성될 것입니다.

EntityManagerFactory에 dialect를 속성으로 제공하는 것을 주목해주세요.

```js
패키지 com.multidbdemo.app.config;

import jakarta.persistence.EntityManagerFactory;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.boot.orm.jpa.EntityManagerFactoryBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;
import org.springframework.context.annotation.Primary;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        basePackages = {"com.multidbdemo.app.config.secondary"},
        transactionManagerRef = "secondaryTransactionManager",
        entityManagerFactoryRef = "secondaryEntityManagerFactory"
)
@Profile("!test")
public class SecondaryDbConfig {

    @Bean(name="db2")
    @ConfigurationProperties(prefix = "spring.seconddatasource")
    public DataSource dataSource(){
        return DataSourceBuilder.create().build();
    }

    @Bean("secondaryEntityManagerFactory")
    public LocalContainerEntityManagerFactoryBean getEntityManagerBean(EntityManagerFactoryBuilder builder, @Qualifier("db2") DataSource dataSource){
        HashMap<String,String> prop = new HashMap<>();
        prop.put("hibernate.dialect","org.hibernate.dialect.PostgreSQLDialect");
        return builder.dataSource(dataSource)
                .properties(prop)
                .packages("com.multidbdemo.app.config.secondary").build();
    }

    @Bean("secondaryTransactionManager")
    public PlatformTransactionManager getTransactionManager(@Qualifier("secondaryEntityManagerFactory") EntityManagerFactory factory){
        return new JpaTransactionManager(factory);
    }
}
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

나는 이 구성이 나의 테스트를 실행할 때 활성화되지 않았으면 좋겠어요.

2.4 Main 클래스:

```js
package com.multidbdemo.app;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

 public static void main(String[] args) {
  SpringApplication.run(DemoApplication.class, args);
 }

}
```

2.5 application.properties: src/main/resource/application.properties

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

spring.application.name=demo
server.port=9001

spring.seconddatasource.jdbc-url=jdbc:postgresql://${POSTGRES_HOST:localhost}:${POSTGRES_PORT:5432}/testme
spring.seconddatasource.username=postgres
spring.seconddatasource.password=root
spring.seconddatasource.driver-class-name=org.postgresql.Driver

spring.db.jdbc-url=jdbc:mysql://${MYSQL_HOST:localhost}:${MYSQL_PORT:3307}/mysqlDb1
spring.db.username=root
spring.db.password=root
spring.db.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update

spring.jpa.generate-ddl=true

2.6 **application.properties**: src/test/resource/application.properties

spring.profiles.active=test

spring.datasource.url=jdbc:h2:./test-db/testDb;AUTO_SERVER=TRUE;DATABASE_TO_LOWER=TRUE;DB_CLOSE_DELAY=-1;MODE=MySQL
spring.datasource.driverClass=org.h2.driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.H2Dialect

spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update

이렇게 만들어서 문제 없이 jar 파일을 생성해야 했어요. 테스트할 때 test 프로필을 만들었는데, TESTS가 실행될 때 활성화될 거예요. 단위 테스트 중에는 H2 데이터베이스를 사용할 거에요.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

2.7 pom.xml:

```js
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
 <modelVersion>4.0.0</modelVersion>
 <parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.3.2</version>
  <relativePath/> <!-- lookup parent from repository -->
 </parent>
 <groupId>com.multidbdemo.app</groupId>
 <artifactId>demo</artifactId>
 <version>0.0.1-SNAPSHOT</version>
 <name>demo</name>
 <description>Demo project for Spring Boot</description>
 <packaging>jar</packaging>
 <url/>
 <licenses>
  <license/>
 </licenses>
 <developers>
  <developer/>
 </developers>
 <scm>
  <connection/>
  <developerConnection/>
  <tag/>
  <url/>
 </scm>
 <properties>
  <java.version>17</java.version>
 </properties>
 <dependencies>
  <dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  <dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
  </dependency>

  <dependency>
   <groupId>com.mysql</groupId>
   <artifactId>mysql-connector-j</artifactId>
   <scope>runtime</scope>
  </dependency>
  <dependency>
   <groupId>org.postgresql</groupId>
   <artifactId>postgresql</artifactId>
   <scope>runtime</scope>
  </dependency>
  <dependency>
   <groupId>com.h2database</groupId>
   <artifactId>h2</artifactId>
   <scope>test</scope>
  </dependency>
  <dependency>
   <groupId>org.projectlombok</groupId>
   <artifactId>lombok</artifactId>
   <optional>true</optional>
  </dependency>
  <dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-test</artifactId>
   <scope>test</scope>
  </dependency>
 </dependencies>

 <build>
  <plugins>
   <plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
     <excludes>
      <exclude>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
      </exclude>
     </excludes>
    </configuration>
   </plugin>
  </plugins>
 </build>

</project>
```

2.8 Dockerfile:

```js
FROM amazoncorretto:17-alpine
MAINTAINER @mohit-sehgal
COPY target/demo-0.0.1-SNAPSHOT.jar demo.jar
ENTRYPOINT ["java", "-jar", "demo.jar"]
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

우리는 어플리케이션을 코딩했어요. 이번에는 자바 파일을 만들고 스프링 부트 어플리케이션용 이미지를 만들어볼게요.

3. Intellij를 사용하여 jar 파일을 만드는 방법:

![Intellij에서 Jar 파일 만들기](/assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_2.png)

target 폴더 안에 생성된 Jar 파일;

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<img src="/assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_3.png" />

4. Docker 이미지 만들기: Dockerfile이 있는 폴더로 이동하여 터미널을 열고 아래 명령을 실행하세요.

```js
docker build -t demo-image .
```

5. 위 이미지를 Docker 컨테이너로 실행하는 명령어는 다음과 같습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
도커를 실행하여 9001번 포트로 데모 애플리케이션 컨테이너를 시작합니다. 컨테이너 이름은 demoAppContainer이며 db_network 네트워크에 연결하고, MYSQL_HOST는 mysqlDb2, MYSQL_PORT는 3306, POSTGRES_PORT는 5432, POSTGRES_HOST는 db-postgres로 설정합니다. demo-image 이미지를 백그라운드에서 실행합니다.
```

위와 같이 설정하면 데모 애플리케이션 컨테이너가 작동 중입니다:

<img src="/assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_4.png" />

이제 요구 사항에 따라 테이블이 생성되었는지 데이터베이스를 확인해 봅시다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

6. 해당 명령을 실행하여 포스트그레스에 "Teachers" 테이블이 생성되었는지 확인해 보세요:

```js
docker exec -it db-postgres psql -U postgres
\c testme
\dt
```

![이미지](/assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_5.png)

7. 아래 명령을 실행하여 MySQL에 "Students" 테이블이 생성되었는지 확인해 보세요:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
도커 실행 -it mysqlDb2 sh
mysql -u root -p
비밀번호 입력: root

use mysqlDb1;
show tables;
```

![이미지](/assets/img/2024-08-18-ConnectingMultipleDBstoSpringBootApplicationRunningonDocker_6.png)

깃허브 저장소에서 모든 코드를 찾을 수 있습니다.

더 많은 이런 글을 원하시면 팔로우 해주세요. 이 글을 읽어주셔서 감사합니다. 즐거운 하루 되세요.
