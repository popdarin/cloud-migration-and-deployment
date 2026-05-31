pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                // โค้ดส่วนนี้ Jenkins จะจัดการดึงให้อัตโนมัติเมื่อผูกกับระบบ
                checkout scm
            }
        }
        stage('Deploy to Production') {
            steps {
                sh '''
                    mkdir -p ~/app
                    cp -r * ~/app/ || true
                    cd ~/app
                    docker compose down || true
                    docker compose up -d
                '''
            }
        }
    }
}
