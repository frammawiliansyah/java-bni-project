pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './mvnw clean install -DskipTests'
      }
    }
    stage('Docker Build & Push') {
      steps {
        sh 'docker build -t image-registry.openshift-image-registry.svc:5000/${PROJECT_NAME}/my-app:latest .'
        sh 'docker push image-registry.openshift-image-registry.svc:5000/${PROJECT_NAME}/my-app:latest'
      }
    }
    stage('Deploy to OpenShift') {
      steps {
        sh 'oc rollout restart deployment/java-bni-project-git'
      }
    }
  }
}