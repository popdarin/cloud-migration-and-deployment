pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Vulnerability Scan') {
            steps {
                sshagent(['aws-ec2-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.106.58.234 "
                            docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image nginx:alpine
                        "
                    '''
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                sshagent(['aws-ec2-key']) {
                    sh '''
                        mkdir -p ~/app
                        scp -o StrictHostKeyChecking=no Jenkinsfile README.md docker-compose.yml ubuntu@3.106.58.234:~/app/
                        ssh -o StrictHostKeyChecking=no ubuntu@3.106.58.234 "
                            cd ~/app &&
                            docker compose down || true &&
                            docker compose up -d
                        "
                    '''
                }
            }
        }
    }
}
