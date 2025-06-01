pipeline {
    agent any

    environment {
        TOMCAT_IP = '3.83.119.255'
        SSH_KEY = '/var/lib/jenkins/.ssh/Saturday.pem'
    }

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
                        docker stop calc-app || true
                        docker rm calc-app || true
                        docker run -d -p 9090:8080 --name calc-app calculator-app
                    """
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    echo 'Packaging WAR file from Docker image...'

                    sh """
                        docker rm -f temp-container || true
                        docker create --name temp-container calculator-app
                        docker cp temp-container:/usr/local/tomcat/webapps/WebAppCal-1.3.5.war ./app.war
                        docker rm temp-container
                    """

                    echo "Copying WAR to Tomcat server ($TOMCAT_IP)..."
                    sh """
                        scp -o StrictHostKeyChecking=no -i $SSH_KEY app.war ec2-user@$TOMCAT_IP:/tmp/
                        ssh -o StrictHostKeyChecking=no -i $SSH_KEY ec2-user@$TOMCAT_IP 'sudo mv /tmp/app.war /opt/tomcat/webapps/app.war && sudo chown tomcat:tomcat /opt/tomcat/webapps/app.war'
                    """
                }
            }
        }
    }
}

