pipeline {
    agent any 
   
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/mmw19b204/my_ci-cd_pipeline.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {  
                bat "docker build -t simzj89/my_node_image:${BUILD_NUMBER} ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
               withCredentials([string(credentialsId: 'dockerhub_secret', variable: 'my_node_cicd')]) {
                    script {
                        sh "docker login -u simzj89 -p ${my_node_cicd}"
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push simzj89/my_node_image:${BUILD_NUMBER}"
            }
        }

        stage('Create container') {
            steps {
                bat "docker run -p 3000:3000 --name my_app_container -d simzj89/my_node_image:${BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}

