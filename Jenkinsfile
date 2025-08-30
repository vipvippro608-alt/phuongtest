pipeline {
    agent any

    environment {
        GIT_EMAIL = "jenkins-bot@example.com"
        GIT_NAME  = "Jenkins Bot"
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Nhập tên branch để push thay đổi (nếu chưa có sẽ tạo mới)')
        string(name: 'FILE_NAME', defaultValue: 'ok.txt', description: 'Tên file cần thêm/xoá')
        choice(name: 'ACTION', choices: ['add', 'delete'], description: 'Chọn hành động với file')
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
                    if (params.ACTION == 'delete') {
                        echo "Deleting file: ${params.FILE_NAME}"
                        sh "rm -f ${params.FILE_NAME}"
                    } else {
                        echo "Adding file: ${params.FILE_NAME}"
                        sh "echo 'Tự động tạo bởi Jenkins' > ${params.FILE_NAME}"
                    }
                }
            }
        }

        stage('Commit & Push') {
            steps {
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'TOKEN')]) {
                    sh """
                        git config user.email "${GIT_EMAIL}"
                        git config user.name "${GIT_NAME}"

                        # Kiểm tra branch local, nếu chưa có thì tạo
                        if ! git show-ref --verify --quiet refs/heads/${params.BRANCH_NAME}; then
                          echo "Branch ${params.BRANCH_NAME} chưa có local -> tạo mới"
                          git checkout -b ${params.BRANCH_NAME}
                        else
                          git checkout ${params.BRANCH_NAME}
                        fi

                        git add .
                        git commit -m "Auto ${params.ACTION} ${params.FILE_NAME} via Jenkins" || echo "No changes to commit"

                        # Push bằng token
                        git remote set-url origin https://$TOKEN@github.com/vipvippro608-alt/phuongtest.git
                        git push origin ${params.BRANCH_NAME}
                    """
                }
            }
        }
    }
}
