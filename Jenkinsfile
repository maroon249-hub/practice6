pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Git repository에서 코드 체크아웃
                checkout scm
            }
        }

        stage('Set Up Python Environment') {
            steps {
                // 맥(Mac)/Linux 환경에 맞춘 Python 가상환경 생성 및 의존성 설치
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run pytest') {
            steps {
                // pytest 실행하여 테스트 수행 및 JUnit XML 리포트 생성
                sh '''
                    source venv/bin/activate
                    pytest tests/ --junitxml=pytest-report.xml
                '''
            }
        }
    }

    post {
        always {
            // JUnit 테스트 결과 보고서 Jenkins에 게시 (Post-build 설정 자동화)
            junit 'pytest-report.xml'
        }
        success {
            echo '테스트 자동화가 성공적으로 완료되었습니다!'
        }
        failure {
            echo '테스트 자동화 중 일부 테스트가 실패했습니다. 리포트를 확인해주세요.'
        }
    }
}
