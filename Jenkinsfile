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
                        sh 'scp docker-compose.yaml ec2-user@3.143.254.53:/home/ec2-user'
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.143.254.53 '${dockerComposeCmd}'"
                    }
                }
            }
        }
    }
}