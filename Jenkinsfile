pipeline {
  agent any

  environment {
    IMAGE_NAME = "try-jenkins"
    CONTAINER_NAME = "try-jenkins"
    APP_PORT = "3000"
    BRANCH = "master"
    REPO_URL = "https://github.com/afrinaldi-knitto/try-jenkins"
  }

  stages {

    stage("Checkout Code") {
      steps {
        git branch: "${BRANCH}", url: "${REPO_URL}"
      }
    }

    stage("Build Docker Image") {
      steps {
        sh "docker build -t ${IMAGE_NAME}:latest ."
      }
    }

    stage("Deploy Container") {
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
      echo "✅ Manual deployment successful"
    }
    failure {
      echo "❌ Deployment failed"
    }
  }
}
