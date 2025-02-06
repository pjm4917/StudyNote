>[!SUMMARY]- 목차
>- [**1️⃣ 현재 실행 중인 Java 버전 확인**](#**1%EF%B8%8F%E2%83%A3%20%ED%98%84%EC%9E%AC%20%EC%8B%A4%ED%96%89%20%EC%A4%91%EC%9D%B8%20Java%20%EB%B2%84%EC%A0%84%20%ED%99%95%EC%9D%B8**)
 >- [**2️⃣ 시스템 환경 변수 수정 (Java 21 설정)**](#**2%EF%B8%8F%E2%83%A3%20%EC%8B%9C%EC%8A%A4%ED%85%9C%20%ED%99%98%EA%B2%BD%20%EB%B3%80%EC%88%98%20%EC%88%98%EC%A0%95%20(Java%2021%20%EC%84%A4%EC%A0%95)**)
>- [**3️⃣ IDE에서 실행하는 JDK 버전 확인**](#**3%EF%B8%8F%E2%83%A3%20IDE%EC%97%90%EC%84%9C%20%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94%20JDK%20%EB%B2%84%EC%A0%84%20%ED%99%95%EC%9D%B8**)
>- [**4️⃣ 클래스 파일 버전 확인**](#**4%EF%B8%8F%E2%83%A3%20%ED%81%B4%EB%9E%98%EC%8A%A4%20%ED%8C%8C%EC%9D%BC%20%EB%B2%84%EC%A0%84%20%ED%99%95%EC%9D%B8**)
>- [**5️⃣ 빌드 툴 (Gradle) 설정 확인**](#**5%EF%B8%8F%E2%83%A3%20%EB%B9%8C%EB%93%9C%20%ED%88%B4%20(Gradle)%20%EC%84%A4%EC%A0%95%20%ED%99%95%EC%9D%B8**)
>- [🔥 **최종 요약**](#%F0%9F%94%A5%20**%EC%B5%9C%EC%A2%85%20%EC%9A%94%EC%95%BD**)

## **1️⃣ 현재 실행 중인 Java 버전 확인**

```sh
java -version
```

#### **출력 예시:**

```
openjdk version "17.0.1" 2021-10-19
OpenJDK Runtime Environment (build 17.0.1+12)
OpenJDK 64-Bit Server VM (build 17.0.1+12, mixed mode, sharing)
```

💡 **Java 21이 아니라면, 환경 변수를 수정하여 최신 버전을 사용해야 함**

---

## **2️⃣ 시스템 환경 변수 수정 (Java 21 설정)**

#### **Windows**

1. `Win + R` → `sysdm.cpl` → `고급` → `환경 변수`
2. `JAVA_HOME`을 Java 21 경로로 설정 (예: `C:\Program Files\Java\jdk-21`)
3. `Path` 변수에서 기존 Java 경로 제거 후, `%JAVA_HOME%\bin` 추가
4. 명령 프롬프트 재실행 후 확인

```sh
echo %JAVA_HOME%
java -version
```

#### **macOS/Linux**

```sh
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
```

적용 후 확인:

```sh
echo $JAVA_HOME
java -version
```

---

## **3️⃣ IDE에서 실행하는 JDK 버전 확인**

#### **IntelliJ IDEA**

1. `File` → `Project Structure (Ctrl + Alt + Shift + S)`
2. `Project` → `Project SDK`를 Java 21로 설정
3. `Run/Debug Configurations`에서 실행할 JDK 확인

---

## **4️⃣ 클래스 파일 버전 확인**

```sh
javap -verbose YourClass.class | grep "major version"
```

#### **출력 예시:**

```
major version: 65
```

|Major Version|JDK Version|
|---|---|
|65|Java 21|
|64|Java 20|
|63|Java 19|
|62|Java 18|
|61|Java 17|

💡 **major version이 62 이상이면 Java 18 이상이 필요함**

---

## **5️⃣ 빌드 툴 (Gradle) 설정 확인**

#### **Gradle (`build.gradle`)**

```gradle
java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(21))
    }
}
```

✅ 적용 후 클린 빌드

```sh
gradle clean build  # Gradle
```

---

## 🔥 **최종 요약**

1️⃣ `java -version` 실행 → **Java 21인지 확인**  
2️⃣ 시스템 환경 변수에서 **JAVA_HOME 수정**  
3️⃣ IDE에서 **실행 JDK 설정 확인**  
4️⃣ `.class` 파일의 **major version 확인 (`javap -verbose`)**  
5️⃣ Gradle **설정 수정 후 클린 빌드**