# 스프링 부트 시작하기

스프링 부트는 스프링 프레임워크의 서브 프로젝트이고 Spring과 Boot의 합성어다. 부트는 시스템을 사용 가능한 상태로 만드는 것을 의미한다. 그래서 스프링 부트의 아이콘도 커뮤터의 전원 버튼 모양이다.



## 프레임 워크

프레임 워크는 애플리케이션의 아키텍처에 해당하는 골격코드라고 할 수 있다. 애플리케이션을 개발할 때 가장 중요한것은 애플리케이션의 전체 구조를 결정하는 아키텍처이다. 아키텍처를 직접 개발하는 것이아니라 프레임 워크로 부터 빌려쓰면 개발자는 아키텍처와 결합할 비지니스 로직 개발에만 집중할 수 있다.



#### Presentation

- 스트러츠(Struts)

  스트러cmsms MVC(Model View Controller) 아키텍처를 제공하는 프레임워크다.

- 스프링 MVC

  스트러츠 같은 MVC 아키텍처를 제공하지만 스트러츠처럼 독립적으로 존재하지 않고 스프링 프레임 워크에 포함되어 있다.

##### Business

- 스프링(IoC, AOP)

  스프링 컨테이너에서 동작하는 비지니스 컴포넌트를 개발한다.

#### Persistence

- JPA

  하이버네이트를 비롯한 모든 ORM 프레임 워크의 표준이다.

- MyBatis

  XML 파일에 작성한 SQL을 자바 객체와 매핑해주는 데이터 매퍼 프레임워크다.



## 스프링 부트의 등장

### 스프링 부트의 장점

- 라이브러리 관리 자동화

  기존에도 자바 프로젝트에서는 메이븐이나 그레이들을 이용해서 라이브러리 의존성을 관리했지만, 스프링 부트에서는 스타터를 이용해 특정 기능에 필요한 라이브러리 의존성을 간단히 처리할 수 있다.

  

- 설정 자동화

  스프링 부트는 프로젝트에 추가된 라이브러리를 기반으로 실행에 필요한 환경을 자동으로 설정해준다. 타임리프를 이용하려면 라이브러리를 추가해주기만하면 스프링 부트가 알아서 관련 스프링 설정을 자동으로 처리해준다. 개발자들은 복잡한 타임리프 설정을 하지 않고도 바로 화면 개발에 들어갈 수 있다.

  

- 라이브러리 버전 자동관리

  스프링을 사용하여 개발할때 가장 신경쓰이는 것이 라이브러리와 버전 관리이다. 스프링은 스프링 라이브러리만 사용하여 개발할 수 없고 의존 관계에 있는 서드파티 라이브러리들도 사용한다. 스프링 부트는 해당 버전의 스프링 라이브러리 뿐만아니라 서드파티 라이브러리들도 호환되는 버전으로 알아서 다운로드해준다.

  이 기능 덕분에 라이브러리 버전이 높거나 낮아서 정상적으로 동작하지 않는 상황을 겪을 필요가 없다. 이전 처럼 XML 설정을 이용해서 라이브러리를 매번 설정하는 과정을 줄이고 개발자들이 개발에만 집중할 수 있는 환경을 제공한다.

  

- 테스트 환경과 내장 톰캣

  스프링 부트로 생성한 프로젝트에는 JUnit을 비롯한 테스트 관련 라이브러리들이 기본적으로 포함되어 있다. 따라서 컨트롤러를 비롯한 다양한 계층의 클래스들에대해서 테스트 케이스를 쉽게 작성할 수 있다.

  

- 독립적으로 실행가능한 JAR

  애플리 케이션을 개발하고 테스트를 마쳤으면 애플리케이션을 실제 운영서버에 배포하기 위해서 패키징을 해야하는데, 일반 자바 프로젝트를 JAR 로 간단하게 패키징하면 되지만 웹 프로젝트라면 WAR 파일로 패키징 해야한다. 스프링 부트는 독립적으로 실행 가능한 애플리케이션을 빠르게 개발하는 것을 목표로 하기 때문에 웹 애플리케이션도 WAR가 아닌 JAR 파일로 패키징할 수 있게 해준다.





## 스프링 부트 프로젝트 구조 및 실행



### 스프링 부트 프로젝트 구조

스프링 부트를 생성하면 기본적으로 제공되는 디렉터리나 파일들이 있다. 이 는 정해진 컨벤션을 따르고 있기 때문에 마음대로 변경해서는 안된다. 스프링 부트는 기본적으로 메이븐이 제공하는 프로젝트 구조를 따른다.

세 개으 소스 폴더가 제공되는데 src/main/java 에는 일반적인 자바 파일 소스를 등록하고, src/main/resources에는 자바 소스가 아닌 XML이나 프로퍼티 파일들을 등록한다. JUnit 기반의 테스트 케이스는 src/test/java 에 작성한다.

![image-20210228211138776](무제.assets/image-20210228211138776.png)



이 중에서 src/main/resources 가 기존 메이븐 기반 프로젝트와 다르다.

![image-20210228211426614](무제.assets/image-20210228211426614.png)

펼처보면 XML 파일들은 없고 웹 애플리케이션과 관련된 static 과 templates 폴더가 있다.

static 폴더는 HTML과 같은 정적인 웹 리소스가 저장되고, templates 폴더에는 타임리프 같은 템플릿 기반의 웹 리소스가 저장된다. 마지막으로 application.properties 파일에는 프로젝트 전체에서 사용할 프로퍼티 정보들을 저장한다.



```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.3</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>demo-app</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo-app</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
	
</project>
```

Pom.xml 파일이 기본적으로 제공되는데, 가장 중요한 설정은 의존 관계에 해당하는 <dependencies> 설정이다.

- Spring-boot-starter-web

  웹 애플리케이션 개발에 필요한 스프링 MVC 관련 라이브러리

- Spring-boot-starter-test

  JUnit을 비롯한 테스트 관련 라이브러리

특이한 점은 pom.xml 파일에는 의존성 설정이 Web과 Test  두개 밖에 없는데, Maven Dependencies 를 보면 엄청나게 많은 라이브러리가 등록되어 있다.

![image-20210228211905602](무제.assets/image-20210228211905602.png)

이는 <parent> 를 이용하여 다른 pom 설정을 상속받은 것이다.

스프링 부트로 만든 애플리케이션을 실행하려면 메인 클래스가 있어야 하는데, 이미 src/main/java 소스 폴더에 '프로젝트 이름Application.java' 형태로 메인 클래스로 작성되어 있다. 

스프링 부트로 만든 애플리케이션은 일반 자바 애플리케이션으로 실행할 수도있고 웹 애플리케이션으로 실행할 수도 있다. 기본적으로 제공되는 상태로 실행하면 웹 어플리케이션으로 실행되고 내장 톰캣이 구동된다. 하지만 약간의 수정을 거치면 내장 톰캣이 구동되지않는다. 

pom.xml 에 'spring-boot-starter-web' 의존성을 'spring-boot-starter' 의존성으로 바꿔주거나, main 함수에서 application.setWebApplicationType(WebApplicationType.NONE); 코드를 추가해주면 일반 자바 애플리케이션으로 동작하게 된다. 또는 외부 프로퍼티를 사용할 수 도있다.

src/main/resource 소스 폴더에서 application.properties 파일을 수정해보자.

```properties
spring.main.web-application-type=none
```

이렇게 해도 톰캣이 동작하지 않는걸 확인할 수 있다. IDE 를 사용하면 자동완성 기능을 제공해준다. 

자바 소스보다 application.properties의 설정이 우선순위가 높다는 것을 주의하자.



