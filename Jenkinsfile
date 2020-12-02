pipeline {
  agent any
  stages {
    stage('clone') {
      steps {
        git(url: 'https://github.com/gjen1996/dockerSpace.git', changelog: true, branch: 'main')
      }
    }

    stage('build') {
      steps {
        tool 'Maven3.6'
        withMaven(jdk: 'openjdk11', maven: 'Maven3.6') {
          sh '''cd glen-eureka
mvn clean install -DskipTests'''
        }

      }
    }

    stage('sonar') {
      steps {
        sh '''cd glen-eureka
/Users/gaiyucheng/software/sonarQube/sonar-scanner-4.5.0.2216-macosx/bin/sonar-scanner -X -Dsonar.host.url=http://192.168.43.166:9000 -Dsonar.login=admin -Dsonar.password=admin -Dsonar.language=java -Dsonar.projectKey=test-sonarqube -Dsonar.projectName=test-sonarqube -Dsonar.projectVersion=$BUILD_NUMBER -Dsonar.sources=src/ -Dsonar.sourceEncoding=UTF-8 -Dsonar.java.binaries=target/ -Dsonar.exclusions=src/test/**
'''
      }
    }

  }
  environment {
    credentialsId = '73159fef-c482-44d0-81ea-35577d50a3b3'
  }
}