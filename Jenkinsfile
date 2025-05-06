pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                bat '''
                    echo Upgrading pip...
                    python -m pip install --upgrade pip

                    echo Installing dependencies...
                    python -m pip install -r requirements.txt >> pip_log.txt 2>&1
                    type pip_log.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                    echo Running tests...
                    python -m pytest test_app.py
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
                    url: 'https://discord.com/api/webhooks/....' // ganti dengan milikmu
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
                    url: 'https://discord.com/api/webhooks/....' // ganti juga
                )
            }
        }
    }
}
