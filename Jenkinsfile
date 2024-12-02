pipeline {
  agent any
  // JAVA와 Maven Tool 등록
  tools {
    jdk 'jdk17'
    maven 'M3'
  }

  stages {
    // GitHub에서 Jenkins로 소스코드 복제
    stage('Git Clone') {
      steps {
        git url: 'https://github.com/sjh4616/spring-petclinic.git', branch: 'main'
      }
    }
    stage('Maven Build') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
      }
    }
    stage('Docker Image') {
      steps {
        dir("${env.WORKSPACE}") {
          sh """
          docker build -t s4616/spring-petclinic:$BUILD_NUMBER .
          docker tag s4616/spring-petclinic:$BUILD_NUMBER s4616/spring-petclinic:latest
          """
        }
      }
    }

    
  }  
}









