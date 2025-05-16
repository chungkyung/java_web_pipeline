pipeline {
    agent any

    stages {
        stage('Clean') {
            steps {
                bat 'C:\\Windows\\System32\\cmd.exe /c .\\mvnw clean'
            }
        }
        stage('Compile') {
            steps {
                bat 'C:\\Windows\\System32\\cmd.exe /c .\\mvnw compile'
            }
        }
        stage('Test') {
            steps {
                bat 'C:\\Windows\\System32\\cmd.exe /c .\\mvnw test'
            }
        }
        stage('Package') {
            steps {
                bat 'C:\\Windows\\System32\\cmd.exe /c .\\mvnw package'
            }
        }
    }
    post {
        failure {
            echo "❌ 빌드 실패! Jenkins 콘솔 로그를 확인하세요."
        }
        success {
            echo "✅ 모든 단계 성공!"
            
            // ✅ JUnit XML 테스트 결과 리포트 수집
            junit '**/target/surefire-reports/*.xml'

            // ✅ HTML 리포트 아카이빙
            archiveArtifacts artifacts: 'target/site/surefire-report.html', fingerprint: true
        }
    }
}
