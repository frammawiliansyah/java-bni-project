pipeline {
  agent any

  tools {
    jdk 'JDK 21'
    maven 'Maven 3.9'
  }

  environment {
    PROJECT_NAME = "frammakbar-dev"
    IMAGE_NAME = "java-bni-project-git"
    REGISTRY = "image-registry.openshift-image-registry.svc:5000"
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install -DskipTests'
      }
    }

    stage('Login to OpenShift Registry') {
      steps {
        withCredentials([string(credentialsId: 'openshift-token', variable: 'TOKEN')]) {
          sh 'echo $TOKEN | docker login -u developer --password-stdin ${REGISTRY}'
        }
      }
    }

    stage('Docker Build & Push') {
      steps {
        sh 'docker build -t ${REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:latest .'
        sh 'docker push ${REGISTRY}/${PROJECT_NAME}/${IMAGE_NAME}:latest'
      }
    }

    stage('Deploy to OpenShift') {
      steps {
        sh 'oc rollout restart deployment/${IMAGE_NAME} -n ${PROJECT_NAME}'
      }
    }
  }
}
