pipeline {
  agent any
  // JAVA와 Maven Tool 등록
  tools {
    jdk 'JDK17'
    maven 'M3'
  }

  // Docker Hub 접속 정보
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerCredential')
    AWS_CREDENTIALS = credentials('AWSCredential')
    GIT_CREDENTIALS = credentials('gitCredential')
    REGION = 'ap-northeast-2'
  }

  stages {
    // 깃허브에 가서 소스코드 가져오기.
    stage('Git Clone') {
      steps {
        echo 'Git Clone'
        git url: 'https://github.com/jojojo412411/spring-petclinic.git',
          branch: 'main', credentialsIdL: 'GIT_CREDENTIALS'
      }
    }
  }
}
