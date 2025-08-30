pipeline {
    agent any

    parameters {
        string(name: 'TARGET_PATH', defaultValue: 'test.txt', description: 'File/Folder cần xoá')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/vipvippro608-alt/phuongtest.git',
                    branch: 'main',
                    credentialsId: 'Phuongtest'   // đúng với ID bạn tạo trong Jenkins
            }
        }

        stage('Delete file') {
            steps {
                sh "rm -rf ${params.TARGET_PATH}"
            }
        }

        stage('Commit & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Phuongtest', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh '''
                        git config user.name "Jenkins Bot"
                        git config user.email "jenkins-bot@example.com"

                        git add -A
                        git commit -m "auto delete ${TARGET_PATH}" || echo "Không có gì để commit"

                        # Push có kèm username + token
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/vipvippro608-alt/phuongtest.git HEAD:main
                    '''
                }
            }
        }
    }
}
