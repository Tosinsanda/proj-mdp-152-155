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
                echo '=== Reached Deploy Stage ==='
                echo 'Deploying to Tomcat server...'
                script {
                    // Clean up old temp-container if it exists
                    sh 'docker rm -f temp-container || true'

                    // Extract the WAR from the Docker image
                    sh """
                        docker create --name temp-container calculator-app
                        docker cp temp-container:/usr/local/tomcat/webapps/WebAppCal-1.3.5.war ./app.war
                        docker rm temp-container

                        scp -i /var/lib/jenkins/.ssh/Saturday.pem app.war ec2-user@13.218.200.87:/tmp/
                        ssh -i /var/lib/jenkins/.ssh/Saturday.pem ec2-user@13.218.200.87 'sudo mv /tmp/app.war /opt/tomcat/webapps/app.war'
                    """
                }
            }
        }
    }
}

