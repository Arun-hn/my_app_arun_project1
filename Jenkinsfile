pipeline {
    environment {
        dockerimagename = "arunhn/myapp"
        dockerImage = ""
        registryCredential = 'dockerhublogin'
        kubeConfigCredential = 'kubernetes'
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
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Pushing Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
       

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: kubeConfigCredential, kubeconfigFileVariable: 'KUBE_CONFIG']) {
                        sh 'kubectl apply -f your-kubernetes-deployment.yaml'
                    }
                }
            }
        }
    }
}
