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

        stage('Performance Benchmarking') {
            steps {
                sshagent(['aws-ec2-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.106.58.234 "
                            # 1. ติดตั้งเครื่องมือ Apache Benchmark (ab) หากยังไม่มีในเครื่อง
                            sudo apt-get update && sudo apt-get install -y apache2-utils
                            
                            # 2. ทำการรัน Load Test ยิงถล่มหน้าเว็บตัวเองจำนวน 1,000 Requests (พร้อมกันทีละ 50 Concurrency)
                            echo '=== STARTING LOAD TEST ==='
                            ab -n 1000 -c 50 http://localhost/
                            echo '=== LOAD TEST COMPLETED ==='
                            
                            # 3. ตรวจสอบขนาดของ Docker Image ที่ใช้งานในระบบจริง
                            echo '=== IMAGE SIZE CHECK ==='
                            docker images nginx:alpine
                        "
                    '''
                }
            }
        }
    }
}
