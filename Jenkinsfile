pipeline {
    agent any

    environment {
        VENV = 'venv'
        PYTHON_EXEC = 'C:\\Users\\UsEr\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\LocalCache\\local-packages\\Python311\\python.exe'
        PIP_EXEC = 'C:\\Users\\UsEr\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\LocalCache\\local-packages\\Python311\\Scripts\\pip.exe'
    }

    stages {
        stage('Setup Environment & Install Dependencies') {
            steps {
                bat '''
                    echo Checking Python Path...
                    "%PYTHON_EXEC%" --version

                    echo Checking Pip Path...
                    "%PIP_EXEC%" --version

                    echo Verifying if requirements.txt exists...
                    if exist requirements.txt (
                        echo requirements.txt found.
                    ) else (
                        echo requirements.txt not found.
                        exit /b 1
                    )

                    echo Setting up virtual environment...
                    "%PYTHON_EXEC%" -m venv %VENV%

                    echo Activating Virtual Environment...
                    call %VENV%\\Scripts\\activate.bat

                    echo Upgrading pip...
                    "%PIP_EXEC%" install --upgrade pip

                    echo Installing dependencies...
                    "%PIP_EXEC%" install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                    echo Running Tests...
                    call %VENV%\\Scripts\\activate.bat
                    "%PYTHON_EXEC%" -m pytest test_app.py
                '''
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
}
