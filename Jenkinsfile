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
                    echo 'Building Docker image for Calculator App...'
                    docker.build('calculator-app')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Running Docker container for Calculator App...'
                    sh """
                        docker rm -f temp-container || true
                        docker create --name temp-container calculator-app
                        docker cp temp-container:/usr/local/tomcat/webapps/app.war ./app.war
			docker rm temp-container
                    """
                }
            }
        }
    }
}

