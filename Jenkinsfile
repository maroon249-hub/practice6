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
                // 윈도우 환경에 맞춘 pip 업그레이드 및 설치
                bat '''
                    python -m venv venv
                    call venv\\Scripts\\activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run pytest') {
            steps {
                // python -m 모듈 실행 방식으로 경로(Path) 문제 자동 해결
                bat '''
                    call venv\\Scripts\\activate
                    python -m pytest tests/ --junitxml=pytest-report.xml
                '''
            }
        }
    }

    post {
        always {
            // JUnit 테스트 결과 보고서 Jenkins에 게시
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
