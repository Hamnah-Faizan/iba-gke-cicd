pipeline {

  environment {
    dockerimagename = "todo-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Hamnah-Faizan/iba-gke-cicd.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "load-balancer.yaml", "web-application.yaml")
        }
      }
    }

  }

}