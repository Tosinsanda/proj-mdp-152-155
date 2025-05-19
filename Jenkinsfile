pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'project-1',
                    url: 'https://github.com/tosinsanda/proj-mdp-152-155.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('calculator-app')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('calculator-app').run('-p 8080:8080')
                }
            }
        }
    }
}

