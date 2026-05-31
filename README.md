# cloud-migration-and-deployment
Cloud Migration &amp; Deployment (AWS-Jenkins)

# Cloud Migration & Automated CI/CD Deployment Pipeline

โปรเจกต์นี้จัดทำขึ้นเพื่อสาธิตการย้ายระบบแอปพลิเคชันขึ้นสู่ระบบคลาวด์สาธารณะ (AWS) ควบคู่กับการออกแบบและวางโครงสร้างกระบวนการส่งมอบซอฟต์แวร์อัตโนมัติ (CI/CD Pipeline) ภายใต้แนวคิด DevSecOps Hardening และ Performance Optimization

## 🏗️ สถาปัตยกรรมระบบ (System Architecture)

ระบบถูกออกแบบและรันการทำงานบนระบบนิเวศน์แบบ Cloud-Native 100% โดยมีองค์ประกอบดังนี้:
- **Infrastructure:** AWS EC2 Instance (`t2.micro` / Ubuntu 26.04 LTS)
- **CI/CD Automation Server:** Jenkins (รันในรูปแบบ Isolated Docker Container)
- **Containerization Engine:** Docker Engine & Docker Compose
- **Security Scanner:** Trivy Vulnerability Scanner
- **Load Testing Tool:** Apache Benchmark (ab)

---

## 🚀 ขั้นตอนการติดตั้งและใช้งาน (Installation & Setup Guide)

### 1. เงื่อนไขเบื้องต้น (Prerequisites)
- บัญชีผู้ใช้งานระบบ AWS และมีการตั้งค่ากลุ่มความปลอดภัย (Security Group) เปิดพอร์ต 22 (SSH), 80 (HTTP), และ 8080 (Jenkins)
- ระบบปฏิบัติการปลายทางติดตั้ง Docker และ Docker Compose เรียบร้อยแล้ว

### 2. การสั่งรันระบบผ่าน Jenkins Pipeline
ระบบจัดการโครงสร้าง Pipeline ผ่านคำสั่งใน `Jenkinsfile` โดยจะทำงานอัตโนมัติเมื่อมีการกดสั่ง Build ดังนี้:

```groovy
// ขั้นตอนหลักใน Jenkinsfile
stages {
    stage('Checkout Code') { steps { checkout scm } }
    stage('Vulnerability Scan') { ... } // สแกนช่องโหว่ด้วย Trivy
    stage('Deploy to Production') { ... } // ส่งงานและรัน Docker Compose บน Production
    stage('Performance Benchmarking') { ... } // ทดสอบโหลดด้วย Apache Benchmark
}
