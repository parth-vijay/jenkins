pipeline {
  environment {
    imagename = "parth10/monitor-k8s"
    registryCredential = 'docker-credentials'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/parth1625/monitor-k8s.git', branch: 'master', credentialsId: 'github-credentials'])
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image to Docker Hub') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$GIT_COMMIT")
             dockerImage.push('latest')
          }
        }
      }
    }
    stage('Deploy to kubernetes cluster') {
      steps{
        script {
           withKubeConfig([credentialsId: 'kubeconfig']) {
             sh 'helm upgrade --install django-chart django-chart'
           }
        }
      }
    }
  }
}