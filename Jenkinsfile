pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Nhập tên branch để push thay đổi (nếu chưa có sẽ tạo mới)')
        string(name: 'FILE_NAME', defaultValue: 'ok.txt', description: 'Tên file cần thêm/xoá')
        choice(name: 'ACTION', choices: ['add', 'delete'], description: 'Chọn hành động với file')
        text(name: 'FILE_CONTENT', defaultValue: 'Nội dung mặc định...', description: 'Nhập nội dung file khi chọn add')
    }

    environment {
        GIT_CREDENTIALS = credentials('Phuongtest1') // cần cấu hình trước trong Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                    url: 'https://github.com/vipvippro608-alt/phuongtest.git',
                    credentialsId: 'Phuongtest1'
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
                    }
                }
            }
        }

        stage('Commit & Push') {
            steps {
                withCredentials([string(credentialsId: 'Phuongtest-token', variable: 'GITHUB_TOKEN')]) {
                    sh """
                        git config user.name "Jenkins Bot"
                        git config user.email "jenkins-bot@example.com"
                        git add .
                        git commit -m "Auto ${params.ACTION} file ${params.FILE_NAME}" || echo "No changes to commit"
                        git push https://oauth2:${GITHUB_TOKEN}@github.com/vipvippro608-alt/phuongtest.git ${params.BRANCH_NAME}
                    """
                }
            }
        }
    }
}
