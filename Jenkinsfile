pipeline {

    agent any

    stages {
        stage('build') {
            steps {
                script {
                    echo "build the app"
                }
            }
        }
        stage("test") {
            steps {
                script {
                    echo "Test the app"
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploy the app"
                    def dockerComposeCmd = "docker-compose up -d --build"
                    sshagent(['ec2-server-key']) {
                        sh 'scp -o StrictHostKeyChecking=no docker-compose.yaml ubuntu@18.218.157.223:/home/ubuntu/docker-compose.yaml'
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@18.218.157.223 '${dockerComposeCmd}'"
                    }
                }
            }
        }
    }
}