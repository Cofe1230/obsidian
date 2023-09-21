# [ [[SpringBoot]] ] MapStruct
## MapStruct란?
> configuration 접근 방식의 규칙을 기반으로 한 Java bean 타입의 매핑 구현을 단순화 해주는 코드생성기
### 특징
>- 컴파일 시점에 코드를 생성하여 런타임에서 안정성을 보장한다
>- 다른 매핑 라이브러리보다 속도가 빠르다
>- 반복되는 객체 매핑에서 발생하는 실수를 줄이고, 구현 코드를 자동으로 만들어 주기 때문에 사용이 쉽다
>- Annotation processor를 이용하여 객체 간 매핑을 자동으로 제공
>- Lombok의 getter, setter, builder를 이용하기 때문에 Lombok이 필수에 MapStruct보다 먼저 dependency가 추가되어야 한다
## 사용 방법
### 설정
#### Apach Maven
```xml
...
<properties>
    <org.mapstruct.version>1.5.5.Final</org.mapstruct.version>
</properties>
...
<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
...
```
#### Gradle
```groovy
...
plugins {
    ...
    id "com.diffplug.eclipse.apt" version "3.26.0" // Only for Eclipse
}

dependencies {
    ...
    implementation "org.mapstruct:mapstruct:${mapstructVersion}"
    annotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"

    // If you are using mapstruct in test code
    testAnnotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"
}
...
```

#### Apach Ant
```xml
...
<javac
    srcdir="src/main/java"
    destdir="target/classes"
    classpath="path/to/mapstruct-1.5.5.Final.jar">
    <compilerarg line="-processorpath path/to/mapstruct-processor-1.5.5.Final.jar"/>
    <compilerarg line="-s target/generated-sources"/>
</javac>
...
```

>[!info] 추가 확인 방법
>properties에 JavaCompiler>AnnotationProccessing>FactoryPath에서 확인 가능하다

