pipeline {
    agent any
    environment {
        DOCKER_IMAGE_TAG = 'v2023'
        DOCKER_REGISTRY = 'mitchey'
    }
    stages {
        stage('Build Docker images') {
            steps {
                sh 'docker --version'
                sh 'docker build -t ${DOCKER_REGISTRY}/udagram-api-feed:${DOCKER_IMAGE_TAG} ./udagram-api-feed'
                sh 'docker build -t ${DOCKER_REGISTRY}/udagram-api-user:${DOCKER_IMAGE_TAG} ./udagram-api-user'
                sh 'docker build -t ${DOCKER_REGISTRY}/udagram-frontend:${DOCKER_IMAGE_TAG} ./udagram-frontend'
                sh 'docker build -t ${DOCKER_REGISTRY}/reverseproxy:${DOCKER_IMAGE_TAG} ./udagram-reverseproxy'
            }
        }
        stage('Push Docker images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin'
                    sh 'docker push ${DOCKER_REGISTRY}/udagram-api-feed:${DOCKER_IMAGE_TAG}'
                    sh 'docker push ${DOCKER_REGISTRY}/udagram-api-user:${DOCKER_IMAGE_TAG}'
                    sh 'docker push ${DOCKER_REGISTRY}/udagram-frontend:${DOCKER_IMAGE_TAG}'
                    sh 'docker push ${DOCKER_REGISTRY}/reverseproxy:${DOCKER_IMAGE_TAG}'
                }
            }
        }
    }
}
