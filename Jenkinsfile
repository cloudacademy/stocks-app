pipeline {
    agent any

    environment {
        IMAGE = 'cloudacademydevops/stocks-app'
        TAG = "v1.0.${env.BUILD_NUMBER}"
    }

    tools {
        dockerTool 'docker-latest'
    }

    stages {
        stage('Docker Build') {
            agent any
            steps {
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