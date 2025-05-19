
 
pipeline {
    agent any

    environment 
    {
        JAR_NAME = 'javaweb-0.0.1-SNAPSHOT.jar'
        EC2_USER = 'ubuntu'
        EC2_HOST = '3.27.222.58'
        EC2_PATH = '/home/ubuntu/app'
        PEM_PATH = '/c/Users/User/.ssh/ec2-key_V2.pem'
    }

    stages {
        stage('Checkout') 
        {
            steps {
                echo '✅ GitHub에서 코드 체크아웃'
                git branch: 'main', url: 'https://github.com/chungkyung/java_web_pipeline.git'
            }
        }

        stage('Build') 
        {
            steps {
                echo '🔨 Maven 빌드 시작'
                bat 'mvnw.cmd clean package'
            }
        }

        stage('Test') 
        {
            steps {
                echo '🧪 단위 테스트 수행'
                bat 'mvnw.cmd test'
            }
        }

        stage('Archive') 
        {
            steps {
                echo '📦 아티팩트 저장'
                archiveArtifacts artifacts: "target/${env.JAR_NAME}", fingerprint: true
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Deploy to EC2') 
        {
            steps {
                echo '🚀 EC2로 배포 시작'
                // 1. .jar 파일 EC2로 전송
                bat '"C:\\Program Files\\Git\\usr\\bin\\scp.exe" -i C:/Users/User/.ssh/ec2-key_V2.pem target/javaweb-0.0.1-SNAPSHOT.jar ubuntu@3.27.222.58:/home/ubuntu/app/'

                // 2. EC2에서 Spring Boot 재시작 2
                bat '"C:\\Program Files\\Git\\usr\\bin\\ssh.exe" -i C:/Users/User/.ssh/ec2-key_V2.pem ubuntu@3.27.222.58 sudo systemctl restart javaweb'           }
        }
        /*
        stage('Build') 
        {
            steps 
            {
                sh 'mvn clean package'
            }
        }

        stage('Test') 
        {
            steps 
            {
                sh 'mvn test'
            }
        }
        */
        /* 초보자 버전
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
                 bat 'java -jar target/javaweb-0.0.1-SNAPSHOT.jar --server.port=8081'
            }
        }
        */
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
