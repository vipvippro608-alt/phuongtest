pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'Phuongtest1'   // ID Credential trong Jenkins
        GIT_REPO = 'https://github.com/vipvippro608-alt/phuongtest.git'
        GIT_BRANCH = 'main'
    }

    parameters {
        choice(name: 'ACTION', choices: ['delete', 'add'], description: 'Chọn hành động: delete hoặc add')
        string(name: 'FILE_NAME', defaultValue: 'example.txt', description: 'Tên file cần thêm hoặc xoá')
        text(name: 'FILE_CONTENT', defaultValue: 'Nội dung mặc định cho file mới', description: 'Chỉ dùng khi ACTION=add')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}",
                    url: "${GIT_REPO}",
                    credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }

        stage('Process File') {
            steps {
                script {
                    if (params.ACTION == 'delete') {
                        sh """
                            echo "Deleting file: ${FILE_NAME}"
                            rm -f ${FILE_NAME} || true
                        """
                    } else if (params.ACTION == 'add') {
                        sh """
                            echo "Adding file: ${FILE_NAME}"
                            echo "${FILE_CONTENT}" > ${FILE_NAME}
                        """
                    }
                }
            }
        }

        stage('Commit & Push') {
            steps {
                withCredentials([string(credentialsId: "${GIT_CREDENTIALS_ID}", variable: 'GIT_TOKEN')]) {
                    sh '''
                        git config user.name "jenkins-bot"
                        git config user.email "jenkins-bot@example.com"

                        git add -A
                        git commit -m "Auto: ${ACTION} file ${FILE_NAME}" || echo "Nothing to commit"

                        git remote set-url origin https://x-access-token:${GIT_TOKEN}@github.com/vipvippro608-alt/phuongtest.git
                        git push origin ${GIT_BRANCH}
                    '''
                }
            }
        }
    }
}
