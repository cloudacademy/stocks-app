pipeline {
    agent any

    environment {
        DOCKER_BUILDKIT = '1'
        DOCKER_IMAGE = 'cloudacademydevops/stocks-app'
        TAG = 'latest'
    }

    tools {
        dockerTool 'docker-latest'
    }

    stages {
        stage('Docker Build') {
            agent any
            steps {
                sh "DOCKER_BUILDKIT=1 docker build -t ${DOCKER_IMAGE}:${TAG} ."
            }
        }

        stage('Docker Push') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPassword')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push ${DOCKER_IMAGE}:${TAG}"
                }
            }
        }
    }
}