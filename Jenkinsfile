pipeline {

    agent any

    stages {
        stage("test") {
            steps {
                script {
                    echo "Test the app"
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    echo "Build and push image"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        dir('./backend') {
                            sh 'docker build -t chinmayapradhan/backend:1.0 .'
                            sh "echo $PASS | docker login -u $USER --password-stdin"
                            sh 'docker push chinmayapradhan/backend:1.0'
                        }
                        dir('./frontend') {
                           sh 'docker build -t chinmayapradhan/frotend:1.0 .'
                           sh "echo $PASS | docker login -u $USER --password-stdin"
                           sh 'docker push chinmayapradhan/frotend:1.0' 
                        }
                    }
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploy the app"
                    def dockerComposeCmd = "docker-compose up -d"
                    sshagent(['ec2-server-key']) {
                        sh 'scp -o StrictHostKeyChecking=no docker-compose.yaml ubuntu@18.188.170.189:/home/ubuntu/docker-compose.yaml'
                        sh 'scp -o StrictHostKeyChecking=no ./backend/.env.sample ubuntu@18.188.170.189:/home/ubuntu/'
                        sh 'scp -o StrictHostKeyChecking=no ./frotend/.env.sample ubuntu@18.188.170.189:/home/ubuntu/'
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@18.188.170.189 '${dockerComposeCmd}'"
                    }
                }
            }
        }
    }
}