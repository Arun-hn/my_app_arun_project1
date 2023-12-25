pipeline {

  environment {
    dockerimagename = "arunhn/myapp"
    dockerImage = ""
  }

  agent any

  stages {
        stage('Checkout Source') {
            steps {
                // Checkout the 'main' branch from the specified Git repository
                git branch: 'main', url: 'https://github.com/Arun-hn/my_app_arun_project1.git'
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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
