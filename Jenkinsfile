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
        withSonarQubeEnv(installationName: 'ScanQube', credentialsId: '2e35e422-febf-4af6-b1e5-455afb511042') {
          sh '''cd glen-eureka
${sonarqubeScannerHome}/bin/sonar-scanner -X 
-Dsonar.host.url=${SONAR_HOST_URL} 
-Dsonar.language=java 
-Dsonar.projectKey=workflow
-Dsonar.projectName=workflow 
-Dsonar.projectVersion=$BUILD_NUMBER  
-Dsonar.sources=src/ 
-Dsonar.sourceEncoding=UTF-8 
-Dsonar.java.binaries=target/ 
-Dsonar.exclusions=src/test/**  


'''
        }

      }
    }

  }
  environment {
    credentialsId = '73159fef-c482-44d0-81ea-35577d50a3b3'
  }
}