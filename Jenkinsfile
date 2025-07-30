pipeline {
    agent {
        node {
            label 'docker'
        }
    }

    environment {
        IMAGE = 'cloudacademydevops/stocks-app-jdemo'
        TAG = "v1.0.${env.BUILD_NUMBER}"
    }

    stages {
        stage('Docker Build') {
            agent any
            steps {
                sh "sed -i 's|CONTAINER_IMAGE|${IMAGE}:${TAG}|g' ./public/index.html"
                sh "docker build -t ${IMAGE}:${TAG} ."
            }
        }

        stage('Docker Push') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPassword')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push ${IMAGE}:${TAG}"
                }
            }
        }
        
        stage('Manifest Update') {
            agent any
            steps {
                echo "triggering update manifest job..."
                build job: 'Update K8s Manifest File', parameters: [string(name: 'IMAGE', value: "${IMAGE}"), string(name: 'TAG', value: "${TAG}")]
            }
        }
    }
}