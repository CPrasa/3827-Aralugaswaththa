
pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/CPrasa/3827-Aralugaswaththa'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                script {
                    bat 'docker build -t crprasad/myapp-cuban:%BUILD_NUMBER% .'
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhub_variable')]) {
                    script {
                        bat "docker login -u crprasad -p ${dockerhub_variable}"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    bat 'docker push crprasad/myapp-cuban:%BUILD_NUMBER%'
                }
            }
        }
    }
    post {
        always {
            script {
                bat 'docker logout'
            }
        }
    }
}
