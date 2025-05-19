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

        stage('Deploy') {
            steps {
                echo 'Deploying to Tomcat server...'
                sh """
                    scp -i ~/Downloads/Saturday.pem target/WebAppCal-1.3.5.war ec2-user@54.88.143.151:/tmp/
                    ssh -i ~/Downloads/Saturday.pem ec2-user@54.88.143.151 'sudo mv /tmp/JavaCalculator.war /opt/tomcat/webapps/'
                """
            }
        }
    }
}

