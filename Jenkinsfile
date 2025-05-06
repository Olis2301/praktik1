pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\UsEr\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\LocalCache\\local-packages\\Python311\\python.exe'
        PIP = 'C:\\Users\\UsEr\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\LocalCache\\local-packages\\Python311\\Scripts\\pip.exe'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                bat '''
                    echo Python version:
                    "%PYTHON%" --version
                    echo Pip version:
                    "%PIP%" --version

                    echo Upgrading pip...
                    "%PIP%" install --upgrade pip

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
                    url: 'https://discord.com/api/webhooks/1369318069889142877/K9EAGeKlveAGDFMB6o902L8onOUp5lIR2YEYn-UvY1DSzTuXJKZLbO3nD1PEWlXIET_J'
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
                    url: 'https://discord.com/api/webhooks/1369318069889142877/K9EAGeKlveAGDFMB6o902L8onOUp5lIR2YEYn-UvY1DSzTuXJKZLbO3nD1PEWlXIET_J'
                )
            }
        }
    }
}
