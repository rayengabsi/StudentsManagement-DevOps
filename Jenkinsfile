pipeline {
  agent any

  tools {
    maven 'M2_HOME'
  }

  environment {
    DOCKER_IMAGE = "rayengabsi/studentsmanagement"
    DOCKER_TAG   = "1.0.0"
  }

  stages {
    stage('GIT') {
      steps {
        git branch: 'main',
            url: 'https://github.com/rayengabsi/StudentsManagement-DevOps.git'
      }
    }

    stage('Maven Version') {
      steps {
        sh 'mvn -version'
      }
    }

    stage('Maven Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Docker Build & Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                          usernameVariable: 'DOCKER_USER',
                                          passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo $DOCKER_PASS | sudo docker login -u $DOCKER_USER --password-stdin
            sudo docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
            sudo docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
          '''
        }
      }
    }
  }
}
