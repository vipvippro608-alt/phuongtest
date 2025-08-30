pipeline {
    agent any

    parameters {
        choice(name: 'ACTION', choices: ['add', 'delete', 'view'], description: 'Chọn hành động')
        string(name: 'FILE_NAME', defaultValue: 'ok.txt', description: 'Tên file để thêm hoặc xoá (bỏ qua nếu view)')
        text(name: 'FILE_CONTENT', defaultValue: 'Hello Jenkins', description: 'Nội dung file khi thêm (bỏ qua nếu xoá hoặc view)')
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Tên branch Git')
    }

    environment {
        REPO_URL = "https://github.com/vipvippro608-alt/phuongtest.git"
        GIT_USER = "vipvippro608-alt"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                    url: "${env.REPO_URL}"
            }
        }

        stage('Process File') {
            steps {
                script {
                    if (params.ACTION == 'add') {
                        echo "Adding file: ${params.FILE_NAME}"
                        writeFile file: "${params.FILE_NAME}", text: "${params.FILE_CONTENT}"
                    } else if (params.ACTION == 'delete') {
                        echo "Deleting file: ${params.FILE_NAME}"
                        sh "rm -f ${params.FILE_NAME}"
                    } else if (params.ACTION == 'view') {
                        echo "Listing all files and folders in branch ${params.BRANCH_NAME}"
                        sh "ls -la"
                    }
                }
            }
        }

        stage('Commit & Push') {
            when {
                anyOf {
                    expression { params.ACTION == 'add' }
                    expression { params.ACTION == 'delete' }
                }
            }
            steps {
                withCredentials([string(credentialsId: 'Phuongtest1', variable: 'GIT_TOKEN')]) {
                    sh """
                        git config user.name "Jenkins Bot"
                        git config user.email "jenkins-bot@example.com"
                        git add .
                        git commit -m "Jenkins ${params.ACTION} file ${params.FILE_NAME}" || echo "Nothing to commit"
                        git push https://${GIT_USER}:${GIT_TOKEN}@github.com/${GIT_USER}/phuongtest.git ${params.BRANCH_NAME}
                    """
                }
            }
        }
    }
}
