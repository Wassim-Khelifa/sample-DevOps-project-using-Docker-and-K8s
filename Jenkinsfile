pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'wass100/mon-app:latest'
        HELM_CHART_PATH = './mon-app'
        KUBE_CONFIG = '/var/jenkins_home/.kube/config'
    }
    stages {
        stage('Checkout repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Wassim-Khelifa/sample-DevOps-project-using-Docker-and-K8s'
            }
        }

        stage('Build Docker image') {
            steps {
                dir('app') {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push Docker image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds', 
                    usernameVariable: 'DOCKER_USERNAME', 
                    passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy with Helm') {
            steps {
                script {
                    sh """
                    helm upgrade --install mon-app ${HELM_CHART_PATH} \
                        --kubeconfig ${KUBE_CONFIG} \
                        --set image.repository=wass100/mon-app \
                        --set image.tag=latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment succeeded!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
