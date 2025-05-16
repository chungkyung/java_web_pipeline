pipeline {
    agent any

    tools {
        maven 'Maven_3.9.9' // Jenkins 관리 > Global Tool Configuration에서 지정한 이름
        jdk 'jdk17'          // 등록된 JDK 이름 (선택)
    }

    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=C:\\Users\\User\\.m2\\repository' // 로컬 캐시 가속
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

    stage('Build') {
        steps {
            echo '🔨 빌드 시작...'
            bat 'mvn clean compile'
        }
    }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                bat 'echo Deploying... (ex: copy to server or Docker image)'
            }
        }
    }
    post {
        failure {
            echo "❌ 실패 발생! 단계별 로그를 확인해주세요."
            mail to: 'team@example.com',
                 subject: "🔴 실패 - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "자세한 로그: ${env.BUILD_URL}"
        }
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
