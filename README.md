# gitlab_CICD

```
此範例為 java project 使用 maven 進行 compile ，並串接 sonarqube
```

```

variables:
  MAVEN_CLI_OPTS: "-s /usr/lib/apache-maven-3.6.3/settings.xml --batch-mode"
  MAVEN_OPTS: "-Dmaven.repo.local=/home/gitlab-runner/.m2/repository"
  SONAR_URL: sonorqube 的網址
  SONAR_LOGIN: sonarqube 的帳號
  SONAR_PASSWORD: sonarqube 的密碼

  
stages:
  - build
  - sonarqube

build-for-testing:
  stage: build
  tags:
    - OpenDataBackStage
  only:
    - develop
    - master
  script:
    - echo "build for testing"
    - export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
    - export PATH=$JAVA_HOME/bin:$PATH
    - export MAVEN_HOME=/usr/lib/apache-maven-3.6.3
    - export PATH=$MAVEN_HOME/bin:$PATH
    - mvn $MAVEN_CLI_OPTS clean package -Dmaven.test.skip=true
  artifacts:
    paths:
      - target/*.war
      
sonarqube:
  stage: sonarqube
  tags:
    - OpenDataBackStage
  only:
    - develop
    - master
  script:
    - echo "sonarqube for testing"
    - export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
    - export PATH=$JAVA_HOME/bin:$PATH
    - export MAVEN_HOME=/usr/lib/apache-maven-3.6.3
    - export PATH=$MAVEN_HOME/bin:$PATH
    - mvn $MAVEN_CLI_OPTS clean sonar:sonar -Dsonar.projectKey=OpenDataBackStage -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.password=$SONAR_PASSWORD -Dsonar.java.binaries=**/* -Dsonar.language=java


```
