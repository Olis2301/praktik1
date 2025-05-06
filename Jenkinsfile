pipeline {
    agent any

   stage('Install Dependencies') {
    steps {
        bat '''
        echo Installing dependencies...
        "C:\\Users\\UsEr\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\LocalCache\\local-packages\\Python311\\Scripts\\pip.exe" install -r requirements.txt
        echo pip exited with errorlevel %ERRORLEVEL%
        exit /b %ERRORLEVEL%
        '''
    }
}


        stage('Run Tests') {
            steps {
                bat '"C:\\Users\\UsEr\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\python.exe" -m pytest test_app.py'
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: 'release/.*', comparator: 'REGEXP'
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                try {
                    def payload = [
                        content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                    ]
                    httpRequest(
                        httpMode: 'POST',
                        contentType: 'APPLICATION_JSON',
                        requestBody: groovy.json.JsonOutput.toJson(payload),
                        url: 'https://discord.com/api/webhooks/1369318069889142877/K9EAGeKlveAGDFMB6o902L8onOUp5lIR2YEYn-UvY1DSzTuXJKZLbO3nD1PEWlXIET_J'
                    )
                } catch (e) {
                    echo "Gagal kirim notifikasi Discord: ${e}"
                }
            }
        }

        failure {
            script {
                try {
                    def payload = [
                        content: "❌ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                    ]
                    httpRequest(
                        httpMode: 'POST',
                        contentType: 'APPLICATION_JSON',
                        requestBody: groovy.json.JsonOutput.toJson(payload),
                        url: 'https://discord.com/api/webhooks/1369318069889142877/K9EAGeKlveAGDFMB6o902L8onOUp5lIR2YEYn-UvY1DSzTuXJKZLbO3nD1PEWlXIET_J'
                    )
                } catch (e) {
                    echo "Gagal kirim notifikasi Discord: ${e}"
                }
            }
        }
    }
