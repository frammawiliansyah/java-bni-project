pipeline {
  agent any

  environment {
    PROJECT_NAME = "frammakbar-dev"
    IMAGE_NAME = "java-bni-project-git"
    REGISTRY = "image-registry.openshift-image-registry.svc:5000"
  }

  stages {
    stage('Check Git') {
      steps {
        sh 'git --version || true'
      }
    }

    stage('Login to OpenShift Registry') {
      steps {
        withCredentials([string(credentialsId: 'openshift-token', variable: 'TOKEN')]) {
          sh 'echo $TOKEN | docker login -u developer --password-stdin ${REGISTRY}'
        }
      }
    }

    stage('Docker Build (with Maven inside)') {
      steps {
        sh 'docker build -t ${REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:latest .'
      }
    }

    stage('Push to OpenShift Registry') {
      steps {
        sh 'docker push ${REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:latest'
      }
    }

    stage('Deploy to OpenShift') {
      steps {
        sh 'oc rollout restart deployment/${IMAGE_NAME} -n ${PROJECT_NAME}'
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completed successfully!'
    }
    failure {
      echo '❌ Pipeline failed.'
    }
  }
}
