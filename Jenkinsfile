pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = "mannamnaveen"
        APP_NAME = "mann-gitops"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'docker'

    }
    stages {
        stage('Cleanup the workspace'){
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/mannamnaveen/mann-gitops-code.git'
            }
        }
        stage('Build the docker image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push the image'){
            steps{
                script{
                    docker.withRegistry('', REGISTRY_CREDS){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push("latest")

                    }
                }
            }
        }
        stage('Delete the docker image'){
            steps{
                sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker rmi ${IMAGE_NAME}:latest"
            }
        }
    }
}