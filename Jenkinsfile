pipeline {
    agent any

    parameters {
        string(name: 'TARGET_PATH', defaultValue: 'test.txt', description: 'File hoặc thư mục cần xoá')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/vipvippro608-alt/phuongtest.git', branch: 'main', credentialsId: 'Phuongtest'
            }
        }

        stage('Delete file') {
            steps {
                sh "rm -rf ${params.TARGET_PATH}"
            }
        }

        stage('Commit & Push') {
            steps {
                sh """
                git config user.name "Jenkins Bot"
                git config user.email "jenkins-bot@example.com"
                git add -A
                git commit -m "auto delete ${params.TARGET_PATH}" || echo "Không có gì để commit"
                git push origin main
                """
            }
        }
    }
}
