pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\UsEsr\\AppData\\Local\\Programs\\Python\\Python313\\python.exe'
        PIP = 'C:\\Users\\UsEr\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pip.exe'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                bat '''
                    echo Installing dependencies...
                    "%PIP%" install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                    echo Running tests...
                    "%PYTHON%" -m pytest test_app.py
                '''
            }
        }
    }

    post {
        success {
            script {
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson([
                        content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                    ]),
                    url: 'https://discord.com/api/webhooks/1369578608624144384/tXq_cJqQaGsxMLrYLhe-f3Qkik5owCB-_5bGtyj6lq0Me0Zzkw6Zijtt39-7FQ7bvdRa'
                )
            }
        }

        failure {
            script {
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson([
                        content: "❌ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                    ]),
                    url: 'https://discord.com/api/webhooks/1369578608624144384/tXq_cJqQaGsxMLrYLhe-f3Qkik5owCB-_5bGtyj6lq0Me0Zzkw6Zijtt39-7FQ7bvdRa'
                )
            }
        }
    }
}
