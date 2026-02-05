pipeline {
  agent any

  environment {
    IMAGE_NAME = "try-jenkins"
    CONTAINER_NAME = "try-jenkins"
    APP_PORT = "3000"
  }

  triggers {
    githubPush()
  }

  stages {

    stage("Checkout") {
      steps {
        checkout scm
      }
    }

    stage("Build Docker Image") {
      steps {
        sh """
          docker build -t ${IMAGE_NAME}:latest .
        """
      }
    }

    stage("Deploy") {
      steps {
        sh """
          docker stop ${CONTAINER_NAME} || true
          docker rm ${CONTAINER_NAME} || true

          docker run -d \
            --name ${CONTAINER_NAME} \
            --restart unless-stopped \
            -p ${APP_PORT}:3000 \
            ${IMAGE_NAME}:latest
        """
      }
    }
  }

  post {
    success {
      echo "✅ Deployment successful"
    }
    failure {
      echo "❌ Deployment failed"
    }
  }
}
