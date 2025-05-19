pipeline {
    agent any

    stages {
        stage('Clean') {
            steps {
                bat 'mvnw.cmd clean'
            }
        }
        stage('Compile') {
            steps {
                bat 'mvnw.cmd compile'
            }
        }
        stage('Test') {
            steps {
                bat 'mvnw.cmd test site'
            }
        }
        stage('Package') {
            steps {
                bat 'mvnw.cmd package'
            }
        }
        stage('Build') {
            steps {
                bat './mvnw clean package'
            }
        }
        stage('Deploy') {
            steps {
                // 실행 중인 Spring Boot 프로세스를 먼저 종료 (충돌 방지)
                // 새 콘솔에서 Spring Boot 앱을 비동기 실행 → Jenkins Job 종료됨
                bat 'start java -jar target/javaweb-0.0.1-SNAPSHOT.jar --server.port=8081'
            }
        }
    }

    post {
        failure {
            echo "❌ 빌드 실패! Jenkins 콘솔 로그를 확인하세요."
        }
        success {
            echo "✅ 모든 단계 성공!"
            junit '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: 'target/site/surefire-report.html', fingerprint: true
        }
    }
}
