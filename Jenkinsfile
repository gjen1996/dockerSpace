pipeline {
  agent any
  stages {
    stage('clone') {
      steps {
        git(url: 'https://github.com/gjen1996/dockerSpace.git', branch: 'master', changelog: true)
      }
    }

    stage('sonar') {
      steps {
        tool 'Maven3.6'
        withMaven(maven: 'Maven3.6', jdk: 'openjdk11') {
          sh '''cd glen-eureka
mvn clean install -DskipTests'''
        }

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

  }
  environment {
    credentialsId = '73159fef-c482-44d0-81ea-35577d50a3b3'
  }
}