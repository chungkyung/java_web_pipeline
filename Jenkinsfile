
 
pipeline {
    agent any

    environment 
    {
        JAR_NAME = 'javaweb-0.0.1-SNAPSHOT.jar' //빌드 결과물 파일명
        EC2_USER = 'ubuntu' // EC2 인스턴스 로그인 사용자 이름
        EC2_HOST = '3.27.222.58' // EC2의 퍼블릭 IP 주소
        EC2_PATH = '/home/ubuntu/app' // EC2 내부의 JAR을 배포할 경로
        PEM_PATH = '/c/Users/User/.ssh/ec2-key_V2.pem' // EV2 접속을 위한 개인 키 경로(Window 기준준)
    }

    stages {
        stage('Checkout') 
        {
            steps {
                //GitHub에서 지정된 브랜치의 코드를 클론(pull) 해서 워크스페이스에 다운로드
                //즉, 최신 소스를 Jenkins로 가져오는 단계
                echo '✅ GitHub에서 코드 체크아웃'
                git branch: 'main', url: 'https://github.com/chungkyung/java_web_pipeline.git'
            }
        }
        stage('Build') 
        {
            steps {
                /* Maven Wrapper를 사용해 프로젝트를 빌드
                clean: 이전 빌드 결과물 삭제
                package: .jar 등 실행 가능한 패키지 생성
                bat는 Windows 명령어 실행을 위한 키워드 */
                echo '🔨 Maven 빌드 시작'
                bat 'mvnw.cmd clean package'
            }
        }
        /*
        stage('Test') 
        {
            steps {
                /*Maven 테스트 단계 실행 (src/test 내의 단위 테스트)
                코드 품질을 확인하고 오류를 사전에 방지
                테스트 실패 시 파이프라인 자동 중단 가능
                echo '🧪 단위 테스트 수행'
                bat 'mvnw.cmd test'
            }
        }

        stage('Archive') 
        {
            steps {
                /*archiveArtifacts: 빌드 결과물(JAR)을 Jenkins에 저장
                fingerprint: 파일 추적 기능
                junit: 테스트 리포트를 수집하여 Jenkins UI에서 확인 가능하게 함
                echo '📦 아티팩트 저장'
                archiveArtifacts artifacts: "target/${env.JAR_NAME}", fingerprint: true
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Generate HTML Report') {
            steps {
                /*Maven으로 HTML 테스트 리포트 생성 (site 플러그인)
                생성된 리포트를 Jenkins에 저장 (확인용)
                echo '📄 HTML 테스트 리포트 생성'
                bat 'mvnw.cmd site'
                archiveArtifacts artifacts: 'target/site/surefire-report.html', fingerprint: true
            }
        }*/

        stage('Deploy to EC2') 
        {
            steps {
                /*scp: EC2에 .jar 파일 복사
                ssh: EC2에 접속해서 systemctl restart 명령 실행
                결과적으로 EC2에서 웹 앱이 자동 재시작됨 */
                echo '🚀 EC2로 배포 시작'
                bat '"C:\\Program Files\\Git\\usr\\bin\\scp.exe" -o StrictHostKeyChecking=no -i C:/Users/User/.ssh/ec2-key_V2.pem target/javaweb-0.0.1-SNAPSHOT.jar ubuntu@3.27.222.58:/home/ubuntu/app/'
                bat '"C:\\Program Files\\Git\\usr\\bin\\ssh.exe" -o StrictHostKeyChecking=no -i C:/Users/User/.ssh/ec2-key_V2.pem ubuntu@3.27.222.58 sudo systemctl restart javaweb'
            }

        }
    }

    post {
        failure {
            echo "❌ 빌드 실패! Jenkins 콘솔 로그를 확인하세요."
        }
        success {
            echo "✅ 모든 단계 성공!"
        }
    }
}
