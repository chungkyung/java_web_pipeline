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
                // 실행 중인 서버 있으면 종료 후 실행하거나
                // 새로 실행 (포트를 반드시 8081로 지정)
                bat 'java -jar target/javaweb-0.0.1-SNAPSHOT.jar --server.port=8081'
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
