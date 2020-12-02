pipeline {
  agent any
  stages {
    stage('clone') {
      steps {
        git(url: 'https://github.com/gjen1996/dockerSpace.git', branch: 'master', changelog: true)
      }
    }

stage('sonar') { 
         echo 'This is a sonar step' 
         def sonarqubeScannerHome = tool name: 'SonarQube'
         echo sonarqubeScannerHome
                withSonarQubeEnv('sonar') {
                sh "${sonarqubeScannerHome}/bin/sonar-scanner -X "+
                "-Dsonar.host.url=${SONAR_HOST_URL} " +
                "-Dsonar.language=java " + 
                "-Dsonar.projectKey=workflow " + 
                "-Dsonar.projectName=workflow " + 
                "-Dsonar.projectVersion=$BUILD_NUMBER " + 
                "-Dsonar.sources=src/ " + 
                "-Dsonar.sourceEncoding=UTF-8 " + 
                "-Dsonar.java.binaries=target/ " + 
                "-Dsonar.exclusions=src/test/** "   
         }
    }
    stage("QualityGate") {
        echo 'QualityGate'
         timeout(time: 1, unit: "HOURS") {       // 防止获取回调出现异常情况，设置超时时间
             def qg = waitForQualityGate()
            if (qg.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
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
