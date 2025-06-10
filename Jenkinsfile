pipeline {
  agent {
    docker {
      image 'maven:3.9.6-eclipse-temurin-21'
      args '-v /root/.m2:/root/.m2'
    }
  }
  environment {
    PROJECT_NAME = "frammakbar-dev"
    IMAGE_NAME = "java-bni-project-git"
  }
  stages {
    stage('Build') {
      steps {
        sh './mvnw clean install -DskipTests'
      }
    }
    stage('Docker Build & Push') {
      steps {
        sh 'docker build -t image-registry.openshift-image-registry.svc:5000/${PROJECT_NAME}/${IMAGE_NAME}:latest .'
        sh 'docker push image-registry.openshift-image-registry.svc:5000/${PROJECT_NAME}/${IMAGE_NAME}:latest'
      }
    }
    stage('Deploy to OpenShift') {
      steps {
        sh 'oc rollout restart deployment/java-bni-project-git'
      }
    }
  }
}