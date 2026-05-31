pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
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
