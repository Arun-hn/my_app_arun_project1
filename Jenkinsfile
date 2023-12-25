pipeline {
    environment {
        dockerImageName = "arunhn/myapp"
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
                    dockerImage = docker.build(dockerImageName)
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

        stage('Install kubectl') {
            steps {
                script {
                    sh '''
                        curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
                        chmod +x ./kubectl
                        mv ./kubectl /usr/local/bin/kubectl
                    '''
                }
            }
        }

        stage('Verify kubectl') {
            steps {
                script {
                    sh 'kubectl version --client'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: kubeConfigCredential, kubeconfigFileVariable: 'KUBE_CONFIG']) {
                        sh 'kubectl apply --validate=false -f deploymentservice.yaml'
                    }
                }
            }
        }
    }
}
