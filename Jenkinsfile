pipeline {
    agent any

    stages {
        stage('Clean') {
            steps {
                bat './mvnw clean'
            }
        }
        stage('Compile') {
            steps {
                bat './mvnw compile'
            }
        }
        stage('Test') {
            steps {
                // 테스트 + 리포트 HTML 생성을 동시에 수행
                bat './mvnw test surefire-report:report'
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

            // ✅ JUnit XML 테스트 리포트 수집 (테스트 통계용)
            junit '**/target/surefire-reports/*.xml'

            // ✅ HTML 리포트 파일 아카이빙
            archiveArtifacts artifacts: 'target/site/surefire-report.html', fingerprint: true
        }
    }
}
