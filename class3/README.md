# Gradle 철저입문. 4,5,6,7장 정리

# 4장 자바 프로젝트 빌드

## 4.2.1 SourceSet
Java 플러그인에는 main, test 두 가지 소스세트 정의되있음.

## 4.2.2 task (상세한 내용은 책을 다시보자)
compileJava 		: 프로덕션 코드 컴파일
processResources 	: 리소스 컴파일
classes 			: compileJava, processResources 수행
compileTestJava 	: 테스트 코드 컴파일
processTestResources : ..

testClasses 		: ..
jar		: JAR 파일 생성. 대상은 소스세트 main의 output 임. 
			즉 compileJava, processResources 로 생성된 파일들이 대상임.
javadoc : 프로덕션 코드의 javadoc 생성
test 	: .. 
uploadArchives : 아티팩트(빌드로 생성되는 결과물을 의미) 를 저장소에 업로드
clean	: ..
clean<TaskName> : 태스크의 출력결과만 삭제 (ex. cleanJavadoc)
assemble : 테스트 실행하지 않고 결과물(JAR 파일)만 생성
check	: 프로젝트 검사. (gradle java 에서는 test task 와 같음)
build 	: check task, assemble task 실행
buildNeeded : 빌드 대상 프로젝트와 프로젝트 의존관계에서 지정한 의존 대상 프로젝트도 빌드
buildDependent : 빌드 대상 프로젝트에 의존하는 프로젝트도 함께 빌드한다.

안드로이드의 경우 Debug, Release 가 SourceSet은 아니고,
main, test 이게 소스셋이네..
ex) 
compile<SourceSet>Java
process<SourceSet>Resources
<SourceSet>Classes

## 4.2.3 규칙
gradle의 특징중 하나인 규칙도 java 플러그인에 포함되어 있다.
ex) 자바 파일은 src/main/java 에 있다.

## 4.2.4 속성
자바 플러그인은 태스크를 제어하거나 규칙을 구현하기 위해 필요한 속성을 제공한다.
(https://docs.gradle.org/current/userguide/java_plugin.html)
ex)
libsDir : 라이브러리(JAR 파일) 출력 디렉토리
distDir : 배포용 파일(JAR 파일 이외) 출력 디렉토리
sourceSets : 프로젝트의 소스 세트

ex) 속성 접근 예시
println project.libsDir
println libsDir

속성으ㄴ 소스 세트 단위로 제공되기도 한다.
ex) 소스 세트 단위의 속성
name
output
output.classesDir
output.resourcesDir
compileClasspath
runtimeClasspath
java
java.srcDirs
resources
resources.srcDirs
allJava
allSource

ex) 아래처럼 속성에 접근 가능
println(sourceSets)
println(libsDir)
println(sourceSets.test.output)
println(sourceSets['test'].output)

## 4.3 자바 프로젝트에 적용하기
..
..

## 4.4.2 태스크 상세 사항
compileJava task
  - JavaCompile 형 태스크다.
  - 속성에 대한 설명
  - https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html
    - classpath : 자바 코드를 컴파일할 때 적용하는 클래스 패스
    - destinationDir : 컴파일 결과 클래스를 출력하는 디렉토리
    - excludes : 컴파일 대상에서 제외할 파일의 패턴 지정
    - includes : 컴파일 대상에 포함할 파일의 패턴 지정
    - options : 컴파일 옵션 지정
    - source : 컴파일 대상이 되는 모든 자바 코드 표시. 
    	excludes와 includes를 적용한 최종 결과가 설정된다.
    - sourceCompatibility : 컴파일 시에 적용할 자바 코드의 JDK 버전 지정
    - targetCompatibility : 컴파일 시에 생성하는 클래스 파일의 JDK 버전 지정

processResources task
  - Copy형 태스크다.
  - Copy형 태스크의 속성
  - https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html
    - caseSensitive : 파일 패턴 지정에서 대소문자 구분 여부
    - destinationDir : 파일 복사 위치
    - dirMode : 디렉토리를 복사할때 권한(유닉스 형식)을 지정한다. 초기값은 null.
    - duplicatesStrategy : 복사 위치가 중복되었을 때 대처 방법 지정
    	('include':중복 허가, 'exclude':중복 배제, 
    	'warn':중복을 허가하지만 경고 출력. 'fail':중복이 있으면 빌드 중단)
    - excludes : 복사 대상에서 제외할 파일의 패턴 지정
    - fileMode : 파일을 복사할때 권한(유닉스 형식)을 지정한다. 초기값은 null.
    - includeEmptyDirs : 빈 디렉토리를 복사 대상에 포함할지 여부
    - includes : 복사 대사에 포함할 파일의 패턴 지정
    - source : 복사 대상이 되는 모든 파일을 표시한다. 
    	excludes와 includes, includeEmptyDirs 등을 적용한 최종적인 결과가 설정된다

jar task
  - Jar형 태스크다.
  - https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html
    - 속성 생략..

javadoc task
  - Javadoc형 태스크다
  - https://docs.gradle.org/current/dsl/org.gradle.api.tasks.javadoc.Javadoc.html
    - 속성 생략..


## 4.5 자바 규칙
자바 파일은 src/main/java 에 위치하고 확장자가 .java 인 파일
리소스 파일은 src/main/resources 에 위치하고 확장자가 .java 가 아닌 파일

## 4.5.2 환경 구성으로 의존관계 변경하기
gradle에는 의존관계를 그룹화해서 분류하는 환경 구성 configuration 이라는 구조가 있다.
java 플러그인은 다음 환경 구성을 기본으로 제공한다.
  - compile : 소스세트 main을 컴파일할 때 클래스패스. compileJava에서 참조
  - runtime : 소스세트 main을 실행할때 클래스패스. 초기에는 compile 환경 구성과
  	같은 내용이 설정되어 있다. testRuntime 환경 구성을 경유해서 test 태스크가 참조된다.
  - testCompile : 소스세트 test를 컴파일할 때 클래스패스. test 태스크가 참조
  - archives
  - default

## 소스 세트 추가
ex) 각종 샘플 설정
sourceSets {
	integrationTest
}
sourceSets {
	integrationTest {
		java.srcDir file('src/intTest/java')
	}
}
dependencies {
	integrationTestCompile 'xxx:xxx:xxx'
	integrationRuntimeCompile 'xxx:xxx:xxx'
}
task integrationTest(type: Test) {
	// 소스 세트 integrationTest의 출력을 테스트 대상으로 한다.
	testClassesDir = sourceSets.integrationTest.output.classesDir
	// 테스크 실행시의 클래스패스로 integrationTest의 runtimeClasspath
	classpath = sourceSets.integrationTest.runtimeClasspath를 설정한다.
}

## Application 플러그인
자바 애플리케이션을 실행하거나 배포하기 위한 플러그인으로 Java 플러그인을 확장해서 만든 것
주 용도는
  - 빌드 스크립트에 메인 클래스를 지정해서 gradle에서 애플리케이션을 실행한다.
  - 애플리케이션을 실행 환경에 배포하기 위한 압축 파일을 생성한다.

ex)
apply plugin: 'application'
mainClassName = 'com.example.aaa.Simple'
applicationName = 'Simple'

gradle run 
	수행하면 컴파일후 :run 태스크 실행한다. 애플리케이션이 실행이 끝나야 빌드도 종료된다.

gradle distZip
	애플리케이션 실행용 압축 파일을 생성한다.
	build/distributions/SimpleCalc.zip 이 생성될것이다.
https://docs.gradle.org/current/userguide/application_plugin.html

## 4.7 War 플러그인
생략..













