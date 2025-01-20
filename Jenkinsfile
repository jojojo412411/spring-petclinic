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
    //GIT_CREDENTIALS = credentials('gitCredential')
    REGION = 'ap-northeast-2'
  }

  stages {
    // GitHub에 가서 소스코드 가져오기
    stage('Git Clone') {
      steps {
        echo 'Git Clone'
        git url: 'https://github.com/chaan22/spring-petclinic.git',
          branch: 'main'
        
      }
    }
    //Maven 빌드 작업
    stage('Maven Build') {
      steps {
        echo 'Maven Build'
        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
      }
    }
    // Docker Image 생성
    stage('Docker Image Build') {
      steps {
        echo 'Docker Image Build'
        dir("${env.WORKSPACE}") {
          sh """
          docker build -t akstn519/spring-petclinic:$BUILD_NUMBER .
          docker tag akstn519/spring-petclinic:$BUILD_NUMBER akstn519/spring-petclinic:latest
          """
        }
      }
    }

    // DockerHub Login
    stage('Docker Login') {
      steps {
        sh """
        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
        docker push akstn519/spring-petclinic:latest
        """
      }
    }
    
    // Docker Image 삭제
    stage('Remove Docker Image') {
      steps {
        sh """
        docker rmi akstn519/spring-petclinic:$BUILD_NUMBER
        docker rmi akstn519/spring-petclinic:latest
        """
      }
    }


    
  }
}
