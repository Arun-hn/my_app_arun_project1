pipeline {
    environment {
        dockerimagename = "arunhn/myapp"
        dockerImage = ""
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/Arun-hn/my_app_arun_project1.git'
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerhublogin'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying App to Kubernetes') {
            steps {
                script {
                   // Install kubectl in a non-privileged directory
                    sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
                    sh 'chmod +x ./kubectl'
                    sh 'mkdir -p $HOME/bin'
                    sh 'mv ./kubectl $HOME/bin/kubectl'

                    // Update the PATH variable
                    env.PATH = "$HOME/bin:$PATH"
                    
                    withKubeConfig([credentialsId: 'kubernetes']) {
                        sh 'kubectl apply -f deploymentservice.yml'
                    }
                }
            }
        }
    }
}
