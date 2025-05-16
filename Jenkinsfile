pipeline {
    agent any

    stages {
        stage('Clean') {
            steps {
                bat './mvnw.cmd clean'
            }
        }
        stage('Compile') {
            steps {
                bat './mvnw compile'
            }
        }
        stage('Test') {
            steps {
                bat './mvnw test site'
            }
        }
        stage('Package') {
            steps {
                bat './mvnw package'
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
