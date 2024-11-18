pipeline {
    agent any
    
    tools {
        dockerTool 'docker-latest'
    }

    stages {
        stage('Docker Build') {
            agent any
            steps {
                sh 'docker build -t cloudacademydevops/stocks-app:latest .'
            }
        }

        stage('Docker Push') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPassword')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh 'docker push cloudacademydevops/stocks-app:latest'
                }
            }
        }
    }
}