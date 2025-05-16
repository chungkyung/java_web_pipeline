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
        always {
            junit 'target/surefire-reports/*.xml'
        }
        failure {
            mail to: 'devteam@example.com',
                 subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Check Jenkins: ${env.BUILD_URL}"
        }
    }
}
