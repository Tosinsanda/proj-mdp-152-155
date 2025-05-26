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

        stage('Deploy to Tomcat') {
            steps {
                echo '=== Reached Deploy Stage ==='
                echo 'Deploying to Tomcat server...'
                script {
                    // Extract the WAR from the image
                    sh 'docker create --name temp-container calculator-app'
                    sh 'docker cp temp-container:/usr/local/tomcat/webapps/app.war ./app.war'
                    sh 'docker rm temp-container'

                    // Deploy to Tomcat via SCP and SSH
                    sh """
                        scp -i ~/Downloads/Saturday.pem app.war ec2-user@13.218.200.87:/tmp/
                        ssh -i ~/Downloads/Saturday.pem ec2-user@13.218.200.87 'sudo mv /tmp/app.war /opt/tomcat/webapps/app.war'
                    """
                }
            }
        }
    }
}

